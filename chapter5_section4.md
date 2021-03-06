# Section 4. 패킷 필터링

리눅스 커널에는 IP 패킷 필터링 프레임워크 'Netfilter'와 이를 이용하기 위한 커맨드라인 프론트엔트 `iptables`/`ip6tables`가 준비되어 있다.

IP 패킷 필터링은 IP 패킷의 헤더를 참조하여 통과/파기/전송 등의 처리를 하는 것을 말한다. `iptables`를 이용하여 아래와 같은 네트워크 환경을 구축할 수 있다.

* 파일 필터링으로 한 대의 호스트를 요새화
* 인터넷과 LAN을 연결하나 인터넷에서 LAN 호스트를 감춤
* LAN 호스트에서 인터넷으로의 접속성을 확보하고 출발지 주소를 변환
* LAN의 특정 호스트에 패킷을 전송
* 한정적 기능이기는 하나 로드밸런싱, 부하 분산

### 4.1 리눅스에서 iptables/Netfilter를 동작

CentOS에서는 다음과 같은 명령어로 iptables 패키지 설치 여부를 알 수 있다.\(설치되어있음\)

![](/assets/iptables.png)

iptables를 이용하려면 리눅스 커널이 Netfilter 프레임워크를 이용할 수 있도록 설정되어 있어야 한다.

확인해보니 Netfilter 옵션이 유효하게 설정되어 있다.

### 4.2 TCP/IP, ICMP, 패킷 필터

`iptables`는 패킷 헤더를 보고 필터링을 실시한다. 출발지, 목적지, TCP/UDP/ICMP 프로토콜, 80/http나 25/smtp 등의 목적지 포트와 출발지 포트 등 패킷 헤더에 포함된 정보를 바탕으로 필터링을 실시한다.

`iptables`는 '상태 추적형 방화벽\(stateful firewall\)'을 구축할 수 있다. LAN에서의 인터넷 접근을 허가하고 인터넷에서 LAN에 배치된 단말기를 보호하는 방화벽을 구축하는 경우 **LAN에서 인터넷으로 나가는 패킷을 통과**시키는 것은 물론, 이에 대한 **LAN 단말기로 들어오는 응답 패킷 통과**도 허가해야만 한다. `iptables`는 이를 '**연결 추적**'이라는 기능으로 지원한다. TCP는 접속 확립\(Establisted\)을 위해 실행되는 '3way handshake' 작업의 패킷 상태를 바탕으로 패킷이 '접속 상태'에 있는지 판단한다. 이를 **stateful filtering\(stateful inspection\)**이라고 한다.

### 4.3 iptables 작성법

#### 규칙=매칭+타깃

규칙 작성은 패킷 헤더의 추출\(매칭\)과 해당 패킷의 처리\(타킷\)를 정의하는 것을 말한다. 규칙 포맷은 다음과 같다.

```
iptables -A <체인> <매칭 옵션> -j <타깃>
```

예시는 다음과 같다.

![](/assets/iptables example.png)

`-A`로 INPUT 체인에 규칙을 추가한다. `-p`로 TCP 프로토콜을 지정, `-d`로 도착지 IP주소, `--dport`로 도착지 포트, `-s`로 출발지 IP주소를 지정한다. `-j`로 타깃에 ACCEPT를 지정하여 매치된 패킷을 통과시킨다. 출발지 IP주소인 192.168.2.0/24의 네트워크 주소에 속하는 호스트로부터 도착지 IP주소인 192.168.2.1 주소를 가진 호스트의 22/TCP로의 접근을 지정하고 있다.

`iptables`의 주요 옵션은 다음과 같다.

* `-t <테이블>` : 이용할 테이블을 지정한다. 기본은 filter.
* `-A <체인>` : 지정하는 체인에 규칙을 추가한다.
* `-m <모듈>` : 매칭 모듈을 지정한다. 이 옵션 다음에 모듈별 옵션을 추가할 수 있다.\(확장 매칭 모듈의 예 : addtype, conntrack, connlimit, iprange, mac, multiport, state\)
* `-p <프로토콜>` : tcp/udp/icmp/all 중 하나를 지정한다. 기본은 all.
* `-d <도착지 주소>` : 도착지 주소 지정
* `-s <출발지 주소>` : 출발지 주소 지정
* `--dport <포트>` : 도착지 포트 지정. `-p tcp` 또는 `-p udp`와 함께 사용한다.
* `--sport <포트>` : 출발지 포트 지정. `-p tcp` 또는 `-p udp`와 함께 사용한다.
* `-i <인터페이스>` : 패킷이 들어오는 네트워크 인터페이스를 지정한다.
* `-o <인터페이스>` : 패킷이 나가는 네트워크 인터페이스를 지정한다.
* `-j <타깃>` : ACCEPT나 DROP 등의 타깃을 지정한다.

주요 옵션 중 세번째인 `-m`의 확장 매칭 모듈 중에서도 연결 추적을 시행하는 `state` 모듈의 이용 방법은 특별히 잘 알아두라고 한다.

`state` 모듈은 `iptables`의 `--state`를 이용하여 다음과 같이 기술한다.

![](/assets/iptables module example.png)

`--state`에 대한 인수는 탐지할 패킷의 상태를 지정한다. 이것은 eth0에서 eth1로 전송하는 패킷에서 ESTABLISHED\(접속 상태\) 또는 RELATED\(접속 상태에 있는 패킷의 관련 패킷\)을 ACCEPT하는 규칙이다. 복수의 상태를 지정할 때는 위의 예시와 같이 반점\(,\)을 넣어 구분한다.

다음은 `--state`로 지정 가능한 옵션이다.

* NEW : 처음 보는 패킷
* ESTABLISHED : 접속 요구\(SYN\) 후 응답\(SYN/ACK\)이 있었던 또는 응답\(SYN/ACK\) 후에 \(ACK\)가 와서 접속 상태가 되는 패킷
* RELATED : 접속 상태인 패킷과 관련된 패킷. FTP의 데이터 포트\(20/tcp\)는 이것을 이용한다.
* INVALID : NEW, ESTABLISHED, RELATED 이외의 패킷. 대부분이 DROP해도 되는 패킷이다.

다음은 타깃으로 매치된 패킷을 처리한다. 타깃으로 이용할 수 있는 파라미터는 다음과 같다.

* ACCEPT : 패킷 통과
* DROP : 패킷 버림
* QUEUE : 사용자 공간으로 패킷을 들여보냄
* RETURN : 체인 종료

#### 체인=규칙+규칙+....

체인이라는 것은 규칙의 집합이다. 규칙을 정의하는 순번도 중요하다. **패킷은 규칙 순서대로 매칭**되기 때문에 때로는 중요한 패킷을 누락시켜버리는 경우도 발생한다. 다음은 체인 구축 예이다.

![](/assets/iptables chain example.png)

INPUT이라는 체인에서 22/TCP, 80/TCP, 53/UDP를 받아들이고 나머지는 DROP하는 규칙의 집합이다. 먼저 정책을 DROP 등으로 지정하고 그 뒤에 통과시킬 패킷을 지정하는 것이 표준 체인의 사용법이다. 여러 개의 규칙으로 INPUT이라는 체인을 구축하게 되며 체인은 사용자가 자유롭게 정의할 수도 있다.

#### 테이블=체인+체인

특별히 -t로 테이블을 지정하지 않는 경우 filter 테이블이 생성된다. 테이블은 다음과 같다.

| 테이블 | 목적 | 이용 가능한 표준 체인 |
| :--- | :--- | :--- |
| filter | 목적 패킷을 필터링 | INPUT, OUTPUT, FORWARD |
| nat | 출발지 또는 도착지 주소를 변환 | OUTPUT, PREROUTING, POSTROUTING |
| mangle | TTL 등 패킷 헤더를 수정 | INPUT, OUTPUT, PREROUTING, POSTROUTING |
| raw | 연결 추적을 하지 않는 변환 실시 | OUTPUT, PREROUTING |

패킷 헤더 수정 등이 필요하지 않으면 filter와 nat 테이블만으로도 충분하다. filter 테이블은 INPUT과 OUTPUT과 FORWARD 체인을 이용하여 ACCEPT/ DROP 등의 필터링을 실시한다. nat 테이블은 NAT\(Network Address Translation\)을 실시하여 출발지나 도착지 주소를 변환한다. mangle 테이블은 TTL\(Time To Live, 라우터를 거칠 때마다 감소하는 패킷 생존수치\) 값을 조정하여 LAN에서 얼마 만큼의 라우터를 거쳤는가 등의 정보들을 외부 네트워크로 새어나가지 않도록 막거나 TOS 타깃을 이용해 라우팅할 곳을 조정한다.

체인 용도 설명은 다음과 같다.

* INPUT : 패킷 입력 규칙을 정의한다.
* OUTPUT : 패킷 출력 규칙을 정의한다.
* FORWARD : 패킷을 전송하는 규칙을 정의한다.
* PREROUTING : 입력 전에 패킷을 변환한다.
* POSTROUTING : 입력 후에 패킷을 변환한다.

체인은 사용자가 독자적으로 만들 수 있다. `-N`으로 사용자 정의 체인을 만들고 이를 `-A`로 지정, 패킷 매칭과 타깃을 지정한다. 표준 타깃을 지정하는 `-P`는 기본 내장 체인\(INPUT, OUTPUT, FORWARD, PROROUTING, POSTROUTING\)만 이용할 수 있다. 사용자가 독자적으로 체인을 만들 때에는 가장 마지막에 DROP이나 REJECT를 지정해야 한다.

체인 작성 예는 다음과 같다.

```
iptables -N tcp-allow   # tcp-allow 체인 생성

iptables -A tcp-allow -p TCP --syn -j ACCEPT
iptables -A tcp-allow -p TCP -m state --state ESTABLISHED,RELATED -j ACCEPT
iptables -A tcp-allow -j DROP   # tcp-allow 체인 종료

iptables -A INPUT -p ICMP -j ACCEPT
iptables -A INPUT -p TCP -j tcp-allow   # tcp-allow 체인 호출
```

### 4.4 logging

타깃에 LOG나 ULOG를 지정하면 syslog나 ulog에 패킷 로그를 기록할 수 있다. **패킷 로그는 기본으로 커널 로그에 기록된다**. 로그 facility를 변경할 때에는 `--log-level`을 이용한다. 또한 로그 기록 시에는 prefix를 지정할 수도 있다. `--log-prefix`로 문자열을 지정해 두면 기록된 패킷이 어느 규칙에 매치되었는지 판단할 수 있다. 타깃에 LOG/ULOG를 지정하면 해당 규칙을 통과한 후 다음 규칙의 검토에 들어간다. 그리고 LOG/ULOG 타깃 뒤에는 타깃의 DROP이나 REJECT 규칙을 다시 한 번 추가해두도록 한다.

```
iptables -N LOGGING # LOGGING 체인 생성

iptables -A INPUT -j LOGGING
iptables -A INPUT -p ICMP -d 192.168.2.121 -j ACCEPT
iptables -A LOGGING -j LOG # 상기 규칙 이외의 패킷을 로그 기록
iptables -A LOGGING -j DROP # LOGGING 체인 종료
```

### 4.5 패킷 필터 스크립트 작성

iptables로 패킷 필터링을 구축할 때 규칙 정의에는 iptables을 실행하게 되지만, **원격으로 수동 입력하는 경우**에 첫 기본 정책에서 타깃에 DROP을 지정하면 그 후에는 원격 접근을 할 수 없게 된다. 원격 환경에서는 가능한 한 iptables를 건드는 것은 피하는 것이 좋지만, 불가피할 때는 **스크립트를 사용해서 규칙을 기술**한다. 기본 정책 실행 후에 원격 접근 가능한 규칙까지 한꺼번에 추가하면 이와 같은 문제를 피할 수 있다.

그리고 iptables의 옵션에서는 네트워크 인터페이스\(예를 들면 eth0\)와 IP주소를 지정한다. 셸 스크립트에서 **주소를 지정하는 변수**를 만들어 두면 다른 호스트에도 이를 쉽게 활용할 수 있다.\(범용성을 높일 수 있다.\)

철자법 실수 등으로 변수가 무효화되어 있지는 않은지 셸에 `-x`를 붙여 변수를 풀어보면 실제 값을 확인할 수 있다.

### 4.6 디버그 방법

iptables 규칙을 스크립트로 만들어 디버그할 때에는 셸의 `echo`를 이용한다. 예시는 다음과 같이 두 가지 방법이 있다.

1. 스크립트의 각 행에 `echo 'rule 1'`등 표시할 문자열을 알기 쉽게 바꾸어 기술.
2. 체인째로 `echo`

```
IPT=/sbin/iptables
EXDEV=eth0

1) 에러일 때 echo
$IPT -A INPUT -i $EXDEV -p TCP --sport 22 -j ACCEPT || echo "ssh/tcp input rule failed."
$IPT -A INPUT -i $EXDEV -p TCP --sport 80 j ACCEPT || echo "http/tcp input rule failed."
$IPT -A INPUT -i $EXDEV -p UDP --sport 53 -j ACCEPT || echo "domain/udp input rule failed."
2) INPUT 체인째로 echo
echo "INPUT"
$IPT -A INPUT -i $EXDEV -p TCP --sport 22 -j ACCEPT
$IPT -A INPUT -i $EXDEV -p TCP --sport 80 j ACCEPT
$IPT -A INPUT -i $EXDEV -p UDP --sport 53 -j ACCEPT
```

1\)의 경우 두번째 줄의 `j`가 틀렸으므로 `http/tcp input rule failed`가 출력된다.

2\)의 경우 INPUT체인에서 틀린 게 있다고 알려준다.

### 4.7 iptables 규칙 부팅 스크립트

CentOS에서는 /etc/init.d/iptables에 iptables 규칙을 유효화하는 스크립트가 준비되어 있다. 이 스크립트는 iptables-restore에 규칙의 덤프 파일을 입력하면 유효화된다.

사용자가 규칙을 만든 뒤에는 이를 적용하여 iptables-save로 덤프한다. 설치 시에 생성된 규칙을 백업한 후에 실행하도록 한다.


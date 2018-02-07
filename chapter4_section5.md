# Section 5. X 디스플레이 매니저

### 5.1 X 디스플레이 매니저란?

CUI에서 구동했을 때 init은 mgetty을 실행하여 터미널에 `login:`을 표시한다. GUI인 경우에는 login 대신 디스플레이 매니저가 실행된다. 이는 inittab에서의 런레벨 설정, 각 런레벨에서의 xdm 실행 여부에 따라 달라진다.

### 5.2 XDMCP

XDMCP\(X Display Manager Control Protocol\)는 **X 서버가 실행하는 호스트와 X 클라이언트가 XDM과 통신하기 위해 X 단말기에서 이용하는 프로토콜**을 말한다. xdm은 원격 또는 로컬 X 클라이언트의 요청을 받아 로그인 화면을 반환하고 세션을 시작한다.

원격\(remote\)에서 실행되는 X 디스플레이 매니저를 로컬 X 환경에 표시하는 경우에는 Xephyr이 사용된다. 먼저 원격 호스트에서 gdm 설정을 변경한다. CentOS에서는 /etc/gdm/custom.conf의 \[xdmcp\] 섹션을 다음과 같이 변경한다.

```
[xdmcp]
Enable=true
```

기본값으로는 XDMCP가 무효화되어 있으므로 이를 변경하고 저장한 다음 gdm을 재시작하여 유효화한다. gdm을 다시 시작하면 177/udp가 열리는 것을 알 수 있다.

```
$ netstat -uan | grep 177
```

으로 XDMCP 이용이 가능해졌는지 알 수 있고

```
$ sudo iptables -A INPUT -p udp --dport 177 -j ACCEPT
```

로 GDM이 실행되는 원격 호스트에서의 iptables 룰 추가를 할 수 있다.\(포트 자체가 막혀있는 경우 열어주는 것이다.\)

다음은 Xephyr로 gdm을 표시하는 예시이다.\(원격 호스트 'hopper'에서 로컬 호스트의 X 세션 번호 1에 표시하라는 지시이다.\)

```
$ Xephyr :1 -query hopper
```




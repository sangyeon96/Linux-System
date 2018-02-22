# Section 1. syslogd

### 1.1 syslog란?

**로그**란 시스템의 동작이나 장애 등을 기록한 것으로 문제 해결의 단서가 되기 때문에 시스템 관리에서 매우 중요하다.

대부분의 UNIX 시스템에서는 **syslog**라고 불리는 데몬이 실행되고 있으며, 커널, 시스템 애플리케이션\(cron 데몬이나 sendmail 등의 SMTP 데몬 등\)에서 메시지를 받는다. 또한, TCP/IP를 이용하여 네트워크상의 원격 호스트의 syslog 데몬의 메시지도 기록할 수 있다. syslog는 **RFC\(Request for Comments\)**에 정의되어 있는 사양으로, 이에 근거한 구현이라면 리눅스 서버뿐만 아니라 라우터에서도 송신이 가능하다.

로그로 기록되는 메시지에는 날짜, 호스트, 송신자, 메시지가 한 행에 기록된다. 기본값으로는 텍스트 파일로 저장되지만, PostgreSQL이나 MySQL 등의 RDBMS에 저장하여 도구를 사용해 나중에 편집할 수도 있다.

### 1.2 syslog의 기록 내용

날짜, 호스트 이름, 태그 이름, 메시지를 순서대로 볼 수 있다.

syslog를 정의하는 RFC 3164 'The BSD syslog Protocol'에서는** PRI, HEADER, MSG** 세 가지를 정의하고 있다.

PRI 부분에 facility, priority를 나타내는 정수를 기술한다. 이 두 가지 변수에 대해서는 나중에 설명하겠다고 한다.

syslog를 실제로 구현한 rsylog나 syslogd 등의 애플리케이션은 이 PRI 부분은 생략되고 HEADER, MSG 부분이 기록된다.

Header 부분에는 날짜와 호스트 이름이 들어간다.

syslog는 클라이언트/서버 모델을 채택하고 있다. 로그를 기록하는 서버인 syslogd는 UNIX도메인 소켓 /dev/log를 준비하여 구동한다. 기록하려는 메시지를 송신하는 클라이언트인 애플리케이션 쪽에서는 syslog 함수를 이용하여 /dev/log 경유로 메시지를 syslogd 서버에 송신한다.

### 1.3 facility와 priority

#### facility

* auth/security : 인증용\(4, 10, 13, 14\)
* authpriv : private 인증 메시지
* cron : cron 등\(9, 15\)
* daemon : facility 지정이 없는 데몬\(3\)
* kern : 커널\(0\)
* ipr : 프린터\(6\)
* mail : 메일 서버\(2\)
* news : 뉴스 서버\(7\)
* syslog : syslog 데몬\(5\)
* user : 사용자 프로세스용\(1\)
* uucp : UUCP용\(8\)
* local\[0-7\] : 임의 용도 7개 \(16~23\)

#### priority

* debug : 디버그 레벨\(7\)
* info : 정보 제공 레벨\(6\)
* notice : 통보 레벨\(5\)
* warning/warn : 주의 레벨\(4\)
* error/err : 과실, 에러 레벨\(3\)
* crit : 위험 레벨\(2\)
* alert : 경보 레벨\(1\)
* emerg/panic : 긴급 상황\(0\)

### 1.4 설정 파일\(rsyslog\)

![](/assets/:etc:rsyslog conf.png)

### 1.5 syslog 데몬 운용상의 주의점

#### 각 서버 간 시간을 통일

로그를 로컬 syslog에서 원격 syslog 데몬에 전송할 때 로그를 보내는 쪽인 출발지 syslog 데몬에서 기록된 로그의 시각이 기록되어 원격 로그 서버로 전송된다. 로그를 원격 서버에 기록할 때 기본으로는 패킷 손실이 허용되는 UDP를 이용하게 된다. 따라서 로그 서버가 정지해있는 경우 등에 로그 누락이 발생할 수 있다. 로그 누락을 허용할 수 없는 시스템이라면 TCP를 이용하는 설정을 변경하거나 로컬 파일에 기록하여 이것을 원격 로그 서버에 전송하는 등의 운용 방식을 고려하는 것이 좋다.


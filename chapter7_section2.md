# Section 2. cron

cron은 정해진 시간에 커맨드를 실행하는 데몬이다. 사용자가 직접 커맨드를 프롬프트에 입력하지 않아도 원하는 시간에 특정 커맨드를 실행할 수 있다. cron을 통해 다음과 같은 작업을 수행할 수 있다.

* 시스템의 로그 파일 용량이 지나치게 커지지 않도록 파일을 치환하여 압축하는 logrotate를 실행
* 주기적으로 데이터베이스의 덤프를 가져와 백업에 이용함
* locate용으로 시스템에 존재하는 파일 리스트를 생성함
* 휴식시간, 점심시간을 wall을 이용해 사용자에게 알림.

### 2.1 cron 데몬

cron은 데몬으로 시스템 부팅 시 기동된다. cron은 다양한 실제 구현이 있으나, 여기에서는 vixie-cron, cronie에 대해 설명할 것이다.

### 2.2 cron 설정 파일

cron 데몬이 기본으로 읽어오는 것은 다음 3가지이다.

* /etc/crontab
* /etc/cron.d
* /var/spool/cron/crontab

cron 데몬이 읽어들이는 실행 파일의 포맷과 그 내용은 다음과 같다.

```
분 시 일 월 요일     실행 사용자명     커맨드와 옵션
```

* 분\(0~59\)
* 시\(0~23\)
* 일\(1~31\)
* 월\(1~12\)
* 요일\(0~7\)

### 2.3 사용자별 cron 설정 파일

cron 데몬은 각 사용자가 `crontab`으로 생성하는 파일까지도 읽어들여 시스템용 cron 설정 파일과 마찬가지로 실행한다.

#### 사용자별 접근 제어

crontab은 시스템에 계정이 있는 사용자라면 이용할 수 있는데, 사용자가 실수로 또는 고의로 부하를 높이는 처리를 실행하도록 지시하는 때가 있다. 따라서 crontab에서는 각 사용자 단위로 접근을 제어할 수 있게 만들어져 있다.

### 2.4 설정 확인

cron 설정 후에 동작을 확인하려면 실행하는 커맨드에 `echo`나 `logger`를 부여해 해당 커맨드가 지정된 시각에 실행되었는지 디버그하면 된다.

### 2.5 로그 로테이션

#### 로그 로테이션 이란?

커널이나 각 데몬의 메시지를 계속 기록하게 되면 로그 파일 크기가 커져 파일 시스템의 파일 용량 상한에 도달하게 되고 메시지를 기록할 수 없게 된다. 이를 방지하기 위한 것이 logrotate이다.

logrotate는 cron 경유로 구동되며 지정된 로그 파일을 이동, 압축하고 새로운 로그 파일을 생성한다. 또한, 필요하면 데몬을 재구동하기도 한다.

#### 로그 로테이션 처리 순서

1. cron이 /etc/cron.daily/logrotate를 읽어들임
2. logrotate가 /etc/logrotate.conf를 설정파일로써 실행하여 logrotate.conf에 지정된 로그 파일을 처리함
3. /etc/logrotate.d/아래의 파일을 읽어들여 지정된 로그 파일을 처리함

##### 주요 logrotate 지시어 리스트

* create
* missingok
* rotate
* daily, weekly, monthly, yearly
* compress
* delaycompress
* compresscmd
* postrotate/endscript
* notifempty
* sharedscripts
* size/minsize
* nocreate
* mail




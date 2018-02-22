# Section 3. anacron

### 3.1 anacron이란?

cron은 프로그램을 계속해서 그리고 주기적으로 실행하기 위한 것이다. 이와 같이 **비 연속적인 시스템 **가동을 위한 cron으로 anacron\(anac\(h\)ronistic cron\)이라는 것이 있다.

### 3.2 anacron의 실행

/etc/anacrontab의 내용은 다음과 같다.

![](/assets/:etc:anacrontab.png)

anacrontab의 실행 시간이나 커맨드를 지정하는 포맷은 '주기', '지연', 'job ID', '커맨드'이다.


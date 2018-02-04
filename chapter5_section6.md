# Section 6. httpd 부팅 스크립트

### 6.1 httpd 부팅 스크립트\(CentOS\)

`chkconfig`는 특정 런레벨에서의 데몬 실행 상태를 설정, 조회하는 도구이다.

```
### BEGIN INIT INFO
# Provides: httpd
# Required-Start: $local_fs $remote_fs $network $named
# Required-Stop: $local_fs $remote_fs $network
# Should-Start: distcache
# Short-Description: start and stop Apache HTTP Server
# Description: The Apache HTTP Server is an extensible server
# implementing the current HTTP standards.
### END INIT INFO
```

CentOS에서는 `chkconfig`가 이것을 보고 초기 부팅 설정을 실시한다.

Required-Start는 서비스 실행 전에 실행되고 있어야 한다. Required-Stop은 서비스 중지 후에 중지시켜야 하는 서비스이다. Should-Start는 서비스 실행 후에 시작하는 서비스이다.

다음은 /etc/rc.d/init.d/functions에 있는 함수의 일부를 가져온 것이다.

![](/assets/:etc:rc.d:init.d:functions.png)

### 6.2 httpd 부팅 스크립트\(Debian\)




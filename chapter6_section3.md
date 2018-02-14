# Section 3. Apache \(httpd 서버\)

### 3.1 Apache란?

리눅스 배포판의 httpd 서버 중 대표적인 것이 Apache HTTP Server이다. RHEL/CentOS에서는 httpd, Debian/Ubuntu에서는 apache2라는 패키지 이름으로 제공되고 있는데, 이 책에서는 Apache라고 부른다. 한글로는 아파치라고 읽는다.

Apache는 모듈로써 다양한 기능을 추가/설정할 수 있다. 사용자도 많아 노하우나 참고 자료가 풍부하고 정보가 많이 나와 있으며 개발을 지휘하는 Apache Foundation의 꾸준한 활동과 개발을 통해 안심하고 이용할 수 있는 웹 서버 중 하나이다.

### 3.2 Apache의 설정 파일

Apache의 설정 파일은 알기 쉬워서 다른 OSS의 설정 파일에서도 같은 스타일을 채택하는 경우가 많다. 설정 파일은 텍스트 파일로 기본 틀이 되는 httpd.conf를 생성한다.

httpd.conf의 안에는 공백으로 구분하여 변수명과 값을 기술하는 **지시어\(directive\)**라는 설명문을 기술한다. 그 예로 include를 이용해 다른 설정 파일을 읽어들이거나 alias로 별명을 붙이는 기능 등이 있다. 지시어는 &lt;Directory&gt;~&lt;/Directory&gt;와 같이 &lt; &gt;와 &lt;/ &gt;로 감싼 **context**를 작성한다. 이렇게 감싼 지시어로 해당 값의 유효 범위가 결정된다. 또한, 지시어에 따라서는 특정 context에 들어가 있지 않으면 이용할 수 없는 때도 있다.

여기서는 CentOS를 중심으로 살펴볼 것이다. CentOS에서는 /etc/httpd/conf/httd.conf를 사용한다.

메인 설정 파일은 httpd.conf로, 가상 호스트는 include 지시어로 지정된 /etc/httpd/conf.d/ 아래의 \*.conf 확장자를 가진 텍스트 파일로 지정한다. 이 디렉터리에는 기본으로 welcome.conf가 유효하게 읽히는 파일로 존재하고 있다.

우선은 httpd.conf의 Apache 전체에 적용되는 지시어를 살펴볼 것이다.

```
ServerTokens OS  # 클라이언트로의 서버 응답 헤더에 적재할 문자열을 지정
ServerRoot "/etc/httpd"  # Apache를 설치한 디렉터리를 지정
PidFile run/httpd.pid  # 프로세스 ID를 기록하는 파일
Timeout 60  # 수신 시간제한 설정
KeepAlive Off  # HTTP의 지속적인 접속을 유효화
```

다음은 MPM\(Multi Processing Module\) 설정이다. MPM은 클라이언트의 요청 처리 방법을 모듈로 구분한 것이다.

MPM의 종류는 다음과 같다.

* event : worker를 멀티 프로세스+멀티 스레드로 구성한 MPM
* mpm\_netware : Novel Netware에 최적화된 MPM
* mpmt\_os2 : OS/2에 최적화된 멀티 프로세스 멀티 스레드 MPM
* prefork : 미리 fork하여 프로세스를 생성, 대기하는 MPM
* mpm\_winnt : Windows NT에 최적화된 멀티 프로세스 MPM
* worker : 멀티 프로세스, 멀티 스레드 MPM

```
<IfModule prefork.c>
    StartServers 8  # 구동 시 준비할 프로세스 수
    MinSpareServers 5  # 대기할 자식 프로세스의 최솟값
    MaxSpareServers 20  # 대기할 자식 프로세스의 최댓값
    ServerLimit 256  # 생성하는 서버 프로세스와 자식 프로세스의 최대 수와 같은 값
    MaxClients 256 # 생성하는 서버 프로세스와 자식 프로세스의 최대 수와 같은 값
    MaxRequestsPerchild 4000 # 자식 프로세스가 취급할 수 있는 요청 최댓값
</IfModule>
```

```
<IfModule worker.c>
    StartServers 4  # 구동 시 준비할 프로세스 수
    MaxClients 300  # 모든 프로세스, 모든 스레드에서 받아들일 수 있는 요청의 최댓값
    MinSpareThreads 25  # 대기 스레드 최솟값
    MaxSpareThreads 75  # 대기 스레드 최댓값
    ThreadsPerChild 25  # 자식 프로세스의 스레드 수
    MaxRequestsPerchild 0 # 자식 프로세스가 취급할 수 있는 요청 최댓값
</IfModule>
Listen 80  # 서버가 대기할 IP주소와 포트번호 지정
```

PHP에서는 prefork를 권장하고 있으며 메모리 용량이 작은 서버에 일정 수준의 액세스 수가 존재하는 경우에는 worker의 멀티 스레드 쪽이 처리 속도가 더 빠를 수도 있다.

Listen 지시어는 기본으로 OS에 마련되어 있는 모든 네트워크 인터페이스로 대기하지만, 포트 번호의 기본 값은 없으므로 이 지시어가 없으면 httpd는 구동되지 않는다.

**DirectoryIndex** 지시어는 요청이 디렉터리 이름으로 되어 있을 때 해당 디렉터리에 지정된 파일이 있으면 그 파일을 반환한다.

**htaccess**는 '**분산 설정 파일**'이라고도 불리며 .htaccess 파일에 기술된 지시어의 내용은 이 파일이 위치한 디렉터리와 그 서브 디렉터리에 모두 적용된다. 지시어는 지정한 파일명에 지시어 설정 내용을 적용한다. 파일명은 ?, \* 등의 와일드 카드 기호를 이용할 수 있으며 ~를 넣으면 파일명에 정규표현을 쓸 수 있다.

```
TypesConfig /etc/mime.types
-생략-
DefaultType text/plain
```

**MIME 타입**이란 파일의 확장자로 **데이터 형식을 판별할 수 있게 해주는 것**이다.

MIME 타입에는 이미지를 나타내는 image, 음성 파일을 나타내는 audio, 동영상 파일을 나타내는 video 등이 있다.

MIME 타입은 IANA\(Internet Assigned Numbers Authority\)가 정의한다. /etc/mime/types의 기술을 기본으로 이용하며 뒤에 기술하는 AddType 지시어로 특정 파일 취급을 사용자 정의할 수 있다.

**HostnameLookups** 지시어는 로그 기술 시에 Off면 IP 주소를, On이면 이름 분석 가능한 호스트는 FQDN을 기록한다. Look up은 찾는다는 의미이므로 "이름 분석을 On한다."라는 뜻이 된다. 하지만, 이름 분석에 시간이 오래 걸리는 시스템에서는 큰 부하로 작용할 수 있으므로 로그 해석 시스템이 따로 있을 때에는 이쪽에 이름 분석을 맡기는 것도 좋은 방법이 될 수 있다.


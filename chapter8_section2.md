# Section 2. PAM 인증

### 2.1 PAM이란?

Pluggable Authentication Modules.

PAM이 등장하기 전까지는 시스템에 설치된 각 프로그램이 저마다 인증을 실시했다.

PAM은 인증 실행을 위한 프레임워크로, 이용하는 프로그램에 인증 방식\(로컬 shadow를 이용할 것인가, 원격 LDAP 서비스를 참조할 것인가 등\) 지정이나 비밀번호 설정 강도 지정, 소속 그룹의 실행 허가 제어 등의 기능을 제공한다. 이들 설정 파일을 생성, 변경, 삭제하면 재부팅 없이 바로 적용된다.

이 프레임워크의 등장으로 각 프로그램은 인증을 PAM에 맡길 수 있게 되었고 코드 중복을 없애고 개별 실제 구현 때문에 발생하는 보안상의 위험을 줄일 수 있게 되었다.

현재는 다수의 리눅스 시스템에서 이와 같은 PAM을 이용한 중앙집중형 인증 시스템을 사용하고 있다. PAM은 나중에도 추가할 수 있는 모듈 아키텍처를 채택하고 있으므로 관리자가 애플리케이션별로 인증 방식을 shadow에서 LDAP로 변경하는 등 유연하게 추가, 변경, 삭제할 수 있다.

PAM을 이용하려면 PAM 라이브러리를 링크해야 한다. 특정 프로그램 인증이 PAM을 지원하는지는 `ldd`로 PAM 라이브러리를 링크하고 있는지 확인하면 된다.

### 2.2 PAM 설정 파일

PAM 설정 파일은 /etc/pam.d에 있다. /etc/pam.d 아래 설정 파일을 하나의 /etc/pam.conf로 합칠 수도 있으나 서비스별로 나뉘어 있는 편이 관리가 더 쉽다.

PAM 설정 파일의 포맷은 간단하게 다음과 같이 표현할 수 있다. /etc/pam.conf와 /etc/pam.d 아래의 설정파일은 맨 처음에 \[서비스\]를 기술하느냐 그렇지 않으냐에 차이가 있다.

```
(/etc/pam.conf)
[서비스][타입][컨트롤 플래그][모듈명][모듈 인수]
(/etc/pam.d)
[타입][컨트롤 플래그][모듈명][모듈 인수]
```

#### /etc/pam.d 작성 형식

##### \[타입\]

* auth : 사용자 인증에 이용
* account : 유효기간이나 비밀번호 제한 등의 계정 제한에 이용
* password : 비밀번호 변경에 이용
* session : 사용자 인증 전후로 실행할 것을 지정함

##### \[컨트롤 플래그\]

* required : 조건에 맞지 않으면 거부하지만, 사용자에게 거부를 통보하기 전에 서비스에 준비된 같은 \[타입\]을 시험해본다.
* requisite : 조건에 맞지 않을 때에는 바로 거부한다.
* sufficient : 조건을 통과하면 required/requisite의 조건 검증 결과를 알린다.
* optional : 조건 검증은 되지만, 무시된다.
* include : 지정된 /etc/pam.d 아래의 다른 파일을 포함한다.
* substack : include와 동일하게 파일을 가져오지만, 컨트롤 플래그에 쓸 수 있는 done, die에 따라 동작이 달라진다.

컨트롤 플래그 \[반환코드\]와 \[액션\]과 조합하여 복수 지정하면 복잡한 컨트롤을 작성할 수 있다.

반환코드에서 축약된 영어 몇 개를 살펴보자면, 

perm\_denied는 permission denied,

cred\_insufficient에서 cred는 credential\(personal information\),

acct\_expired에서 acct는 account이다.

##### PAM의 주요 모듈

* pam\_unix
* pam\_selinux : SELinux의 시큐리티 컨텍스트를 참조
* pam\_env : environment, 즉 환경 변수 설정과 해제
* pam\_deny
* pam\_permit
* pam\_limits




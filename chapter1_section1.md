# Section 1. UNIX의 기초

### 1.1 로그인/로그아웃

**로그인\(Login\)** : 사용자명\(username\)과 비밀번호 등을 통한 시스템의 인증 과정을 거쳐 사용자가 시스템을 이용할 수 있게 되는 과정

**로그아웃\(Logout\)** : 시스템 사용을 끝내는 것

**세션\(Session\)** : 로그인한 후부터 로그아웃할 때까지

![](/assets/Login Form with Sessions.png)

### 1.2 셸과 커맨드

**셸**\(**Shell**, 많은 사이트에서는 **쉘**이라고 부른다**\)** : 사용자가 입력한 지시에 따라 명령\(Command\)를 실행하고 그 결과를 출력하는 프로그램

**커맨드\(Command\)** : 시스템을 처리하는 명령

커맨드는 내부에서 OS의 중추 역할을 하는 커널의 기능을 이용한다. 셸은 커맨드로 이용하는 함수나 시스템에 설치된 프로그램과 커널을 연결해 주는, 사용자와 커널을 잇는 다리 역할을 한다.

![](/assets/Linux Architecture Diagram.jpg)

```
ls (list)
ls -a (list all)
ls -l (list with long format - show permissions)
ls -al /etc/skel/ (list all with long format - show permissions, skeleton directory)
```

여기서 ls의 다양한 커맨드 옵션들을 볼 수 있다.\([https://www.rapidtables.com/code/linux/ls.html](https://www.rapidtables.com/code/linux/ls.html)\)

### 1.3 매뉴얼 커맨드 man

**man **\(커맨드\) : 각 커맨드의 사용법, 동작을 사용자가 확인할 수 있는 매뉴얼 참조 커맨드

![](/assets/man rm.png)

NAME, SYNOPSIS, DESCRIPTION, OPTION 등을 볼 수 있다.

**whatis** \(커맨드\) : 검색 문자열로 지정한 커맨드명과 매치되는 man 파일의 요약문을 표시

**apropos** \(커맨드\) : 지정된 검색 문자열을 커맨드명과 요약문에서 검색하여 표시

whatis 와 apropos를 비교하자면, apropos가 좀 더 넓은 범위로 찾는다고 할 수 있다. printf를 예로 들자면, whatis printf는 printf 관련 man 파일의 요약문을 표시한다면, apropos는 asprintf, dprintf, fprintf 등도 같이 찾는다.

\(CENTOS 7에서는 mandb를 해도 printf\(1\)만 나온다..\)

### 1.4 사용자와 그룹

리눅스는 여러 명의 사용자가 동시에 하나의 시스템을 이용할 수 있는 '멀티 사용자 시스템'이다.

허가 혹은 제한을 실시하기 위하여 리눅스에서는 **사용자**와 **그룹**이라는 개념을 사용한다.

사용자 계정\(user account\)은 한 사람에게 여러 개 부여할 수 있지만, 하나만 부여하는 것이 일반적이다. 사용자를 하나로 묶는 경우는 그룹을 설정하여 해당 그룹에만 시스템 리소스에 대한 접근을 허가하도록 설정할 수 있다.

### 1.5 허가권과 소유권

한 사용자의 파일을 다른 사용자가 읽고 쓸 수 없도록 파일에 대한 사용자의 읽기 쓰기 실행 속성을 제어하는 '**허가권\(permission\)**'

![](/assets/permission.png)

읽기\(rEAD\), 쓰기\(wRITE\), 실행\(ExECUTE\)

| 기호 | 의미 | 이진수 표현 | 정수 표현 |
| :--- | :--- | :--- | :--- |
| r-- | 읽기 가능 | 100 | 4 |
| -w- | 쓰기 가능 | 010 | 2 |
| --x | 실행 가능 | 001 | 1 |
| rw- | 읽기, 쓰기 가능 | 110 | 6 |
| r-x | 읽기, 실행 가능 | 101 | 5 |
| rwx | 읽기, 쓰기, 실행 가능 | 111 | 7 |

**chmod** 커맨드로 파일의 허가권을 변경할 수 있다. ex\) chmod -x 디렉토리이름 : 해당 디렉토리의 실행 권한을 없앰

여기서 rwx앞에 d는 파일 타입이 directory라는 걸, -는 일반 파일이라는걸 나타낸다.\(나머지 타입은 p.32 참조\)

**suid** \(set _user_ id\) : 파일의 소유자 권한으로 커맨드를 실행 할 수 있다

**sgid** \(set _group_ id\) : 파일은 소유자 권한, 디렉터리인 경우에 그 상위 디렉터리\(부모 디렉터리\)와 동일한 권한으로 실행할 수 있다.

**ps** \(process status\) : 터미널에서 실행되고 있는 프로세스 상태 표시

```
ps a (show processes for all users)
ps u (display the process's user/owner)
ps x (show processes not attached to a terminal)
보고 싶은 옵션에 따라 ps ax, ps aux 이렇게 묶어 쓰면 됨.
```

### 1.6 파일

### 1.7 디렉터리




# Section 1. 셸을 사용해보자

**사용자와 리눅스를 이어주는 셸을 이해하고 그 사용법을 익히는 것은 리눅스 시스템을 이해하는 지름길의 하나이다.**

### 1.1 셸이란?

OS의 중심인 커널을 둘러싼 '껍질'이라고 보면 됨. 커널을 감싸듯이 존재하고 있으며 사용자의 입력을 받아서 다음과 같은 처리를 수행한다.

* 애플리케이션의 구동
* 프로그램 실행 순서의 변경
* 변수 저장
* 실행 커맨드 이력 저장
* 결과 출력

셸은 사전에 커맨드를 기술해 둔 텍스트 파일, 셸 스크립트\(shell script\)를 읽어들이면서 이를 차례대로 실행한다.

![](/assets/Shell-Kernel Interaction.jpg)

### 1.2 셸의 종류

![](/assets/Types of Shells.jpg)

##### bash

Bourne Again SHell.

현재 거의 모든 리눅스 배포판에서 표준으로 채택된 셸. 그전의 Bourne Shell을 대체하며 등장.

##### csh/tcsh

C SHell이라는 계열의 셸. 오래전부터 사용해온 오리지널 csh가 아닌 C 셸의 계통을 잇는 셸로, 현재 tcsh가 주류로 사용되고 있다. FreeBSD 등 많은 BSD 계열 UNIX에서 표준 셸로 채택되어 있다.

##### dash

Debian Almquist SHell.

Debian GNU/Linux나 Ubuntu에서 표준 셸로 채택되고 있으며 bash보다 가볍다는 특징이 있다. NetBSD에서 이용되던 ash를 계승하고 있다.

##### zsh

Z SHell.

이것도 Bourne Shell에 많은 수정을 가해 개량한 셸이다. ksh를 기반으로 하며, bash나 tcsh의 편리한 기능도 많이 채택하고 있다.

![](/assets/Linux Shell Path.png)

CENTOS는 기본적으로 현재 거의 모든 리눅스 배포판에서 표준으로 채택된 셸인 bash를 쓴다.\(하지만 yum install ~로 다른 셸 설치 가능\)

참고하기. 5 Most Frequently Used Open Source Shells for Linux\([https://www.tecmint.com/different-types-of-linux-shells/](https://www.tecmint.com/different-types-of-linux-shells/)\)

### 1.3 셸의 기능

#### 커맨드 프롬프트

![](/assets/Shell prompt.png)

#### 프로그램 실행 동작

![](/assets/sudo su -.png)

리눅스의 커맨드 라인에서 프롬프트는 권리자 권한이 '_\#_', 일반 사용자 등 관리자 이외는 '_$_'\(zsh의 경우 _%_\)가 사용된다.

다음은 `sudo su -` 명령어를 통해 일반 사용자를 관리자 권한으로 변경하는 모습이다. sangyeon에서 root로, '$'에서 '\#'로 바뀌었음을 확인할 수 있다.

#### 자동완성 기능\(tab\)과 히스토리 기능

`tab`을 두 번 누르면, 다음과 같이 입력한 알파벳으로 시작하는 명령어를 볼 수 있다.

![](/assets/tab twice.png)

그리고 p.66 표2-1에 나와있는 방법으로 히스토리를 검색해 실행할 수 있다. 그 중 흥미로웠던 단축키는 `Ctrl`+`r`키로, 과거 사용한 커맨드를 검색할 수 있다. 그리고 그 상태에서 `↑`나 `↓`를 누르면 그 커맨드를 썼을 당시에 썼던 여러 커맨드들을 볼 수 있다.

![](/assets/ctrl+r%282%29.png)

![](/assets/ctrl+r%281%29.png)

#### 작업\(job\) 제어 기능

**foreground job, background job, suspend job**으로 구분되며 실행한 특정 프로그램을 중지하거나, 포어그라운드로 복귀, 백그라운드에서 실행 등으로 변경 할 수 있다.

책에 나와있는 예제의 `emax -nw`, `w3m` 둘 다 command not found인 관계로.. 다른 예제로 해보았다.

다음은 `Ctrl`+`z`키로 중지한 후 `jobs` 명령어 실행 후 스크린샷이다.

![](/assets/jobs.png)

`+` symbol : most recent process, `-` symbol : next most recent process

이후에 `fg 1`, `fg 2` 명령어를 입력했더니 두 프로세스 모두 재개하였다.

여기서 중지한다는 것은 실행하던 프로세스를 중지시키고, 셸로 돌아온다는 의미이다.

`jobs`는 셸의 내장 커맨드이며 작업 상태를 리스트로 보여준다. `fg`도 셸의 내장 커맨드로, `jobs`에 표시되는 job 번호를 지정하면 포어그라운드에 해당 프로세스를 가져와 구동시킨다. `bg`라는 내장 커맨드도 있다. 이 커맨드는 지정한 작업 번호\(\[1\]이나 \[2\]\)의 프로세스를 백그라운드에서 실행시킨다. 처음부터 백그라운드 작업으로 실행한다면 커맨드와 옵션의 마지막에 `&`를 붙여 실행한다.

![](/assets/bg 1%281%29.png)

![](/assets/bg 1%282%29.png)

`bg` 명령어로도 중지했던 프로세스를 재개할 수 있다.

포어그라운드와 백그라운드의 차이에 대해 잘 설명해놓은 URL : [http://thesoul214.tistory.com/67](http://thesoul214.tistory.com/67)

추가적으로, `kill` 명령어를 사용하면 작업 상태가 Stopped에서 Terminated로 바뀐 걸 볼 수 있다. 그리고 `Ctrl`+`z`키를 이용해서 중지하면 작업상태에 남지만, `Ctrl`+`c`키를 이용하면 바로 cancel되어 작업 상태에도 남지 않는다.

![](/assets/kill.png)

#### 리다이렉션과 파이프

프로그램의 출력을 파일로 기록하거나 또는 별도의 프로그램 입력에 사용할 수 있다. 전자를 리다이렉션, 후자를 파이프라고 한다.

##### 리다이렉션

리다이렉션을 이용하기 전에 우선 파일 디스크립터가 뭔지 알아야 한다.

File Descriptor\(파일 디스크립터\)

![](/assets/File Descriptor.png)

리다이렉션은 `>`, `<`, `>>`로 파일 디스크립터를 지정하여 프로그램의 입출력할 곳을 변경한다. 프로그램의 처리 결과를 파일로 기록하여 파일에 기술되어 있는 내용을 프로그램에 전달하기 위해 사용한다. `>`는 이미 파일이 있다면 한 번 비워낸 후에 기록하지만, `>>`를 사용하면 결과를 이어서 추가로 기록한다. `>`와 `<`는 파일 디스크립터를 생략한 것으로, 원래는 `1>`, `0<`라고 쓴다.

간단한 예를 들자면,

```
1) 명령어A > 파일A
2) 명령어A >> 파일A
3) 명령어B < 파일B
4) 명령어C < 파일C > 파일D
```

1\) 명령어A의 결과를 파일A에 저장한다.\(파일A가 없다면 새로 생성하고, 있다면 새로 작성한다.\)

2\) 명령어A의 결과를 파일A에 저장한다.\(파일A가 없다면 새로 생성하고, 있다면 이어서 작성한다.\)

3\) 파일B의 내용을 불러와 명령어B를 실행한 후, 그 결과를 터미널에 출력한다.\(명령어B의 간단한 예를 들자면, `head`나 `tail`.\)

4\) 파일C의 내용을 불러와 명령어C를 실행한 후, 그 결과를 파일D에 저장한다.\(파일D가 없다면 새로 생성하고, 있다면 새로 작성한다.\)

이를 이용해 표준 에러 출력\(2\)을 무시하고 표준 출력만을 화면에 표시하고 싶은 경우 다음과 같이 입력하면 된다.

```bash
2> /dev/null
```

`&`을 붙여 출력과 에러 출력을 하나로 묶을 수도 있다. ex\) `2>&1`

처음에는 에러 출력은 실습을 어떻게 해봐야하나 했는데 좋은 예제를 찾았다.

nop.txt라는 파일이 없는 상태에서 다음과 같이 입력하면

```bash
cat nop.txt > test.txt
```

터미널에 cat: nop.txt: No such file or directory와 함께 test.txt에는 아무런 내용이 없게된다.

이제 다음과 같이 에러 출력\(2\)를 같이 입력하면

```bash
cat nop.txt 2> test.txt
```

test.txt에 cat: nop.txt: No such file or directory 가 출력된다.

다음과 관련된 여러 예제는 [http://blog.gaerae.com/2014/10/linux-file-descriptor.html](http://blog.gaerae.com/2014/10/linux-file-descriptor.html) 에서 확인할 수 있다.

##### 파이프

파이프는 `|`를 이용해 프로그램의 출력을 다른 프로그램의 입력에 쓰도록 하는 것이다.

자주 볼 수 있는 것은 `grep` 명령어를 이용한 예제이다. \(gLOBAL rEGULAR eXPRESSION pRINT\)

```bash
mount | grep '/dev'
```

![](/assets/grep.png)

다음과 같이 `mount` 명령어를 실행했을 때 출력되는 결과에서 문자열 '/dev'가 들어가있는 부분을 출력한다.

해당 블로그에서 친절한 설명과 함께 다양한 예제들을 엿볼 수 있다.\([http://jdm.kr/blog/74](http://jdm.kr/blog/74)\)


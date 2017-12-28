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

##### 커맨드 프롬프트

##### 프로그램 실행 동작

##### 자동완성 기능과 히스토리 기능

##### 작업\(job\) 제어 기능

##### 리다이렉션과 파이프






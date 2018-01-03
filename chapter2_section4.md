# Section 4. 셸의 설정 파일

UNIX 시스템에서는 같은 셸이라도 **부팅 방법에 따라** 로그인 셸과 대화형 셸로 나뉘게 된다. GUI 환경에서는 이 구별이 특별한 의미가 없지만, CUI에서는 읽어들이는 설정파일이 달라진다.

다음은 start-up 파일들과 그에 대한 설명이다.\(출처 : [https://www.slideshare.net/AccioOliveira/lico-02-shell-basics-bash-intro](https://www.slideshare.net/AccioOliveira/lico-02-shell-basics-bash-intro)\)

![](/assets/start-up files.jpg)

### 4.1 로그인 셸\(Login Shell\)

bash의 메뉴얼에 따르면 로그인 셸이란 0번째 인수의 첫 글자가 `-`이거나 `--login`으로 지정되어 있는 경우에 구동하는 셸이다.

로그인 셸과 그렇지 않은 경우는 읽어오는 파일이 다르다. 로그인 셸로 bash가 구동되면 우선 /etc/profile을 불러온다. 그 후에 읽어오는 파일의 순서는 다음과 같다.

![](/assets/Login Shell.jpg)

/etc/profile에서는 프롬프트에 표시되는 항목의 설정 /etc/bash.bashrc가 존재하면 그 파일을 읽어들이고, `id -u` 커맨드 출력이 0이면 사용자는 root이므로 프롬프트를 `#`로, 그 외에는 `$`로 설정한다. 다음으로 /etc/profile.d가 존재하면 이 디렉터리 이하의 .sh 확장자를 갖는 스크립트를 순서대로 읽어들인다. 마지막으로 이 스크립트에서 사용한 변수 i를 `unset` 커맨드로 무효화한다.

/etc/profile의 내용이 보고싶다면 책을 참고하도록.

로그인 셸로 구동된 bash는 사용자가 로그아웃할 때 홈 디렉터리의 .bash\_logout을 읽어들인다.

### 4.2 대화형 셸\(Non-Login Shell\)

대화형 셸이란 로그인 셸이 아닌 상태의 셸을 말한다. 로그인 셸이 아닌 경우 홈 디렉터리의 .bashrc를 불러온다.

책에 .bashrc 파일을 분석한 내용이 나온다. 이건 책 참고하기.

이 내용의 흐름을 파악하면 bash의 추가 기능을 어떻게 설정해야 좋을지 감이 잡힌다고 한다..

개인적으로 Login Shell과 Non-Login Shell에 대한 설명은 어째 [http://webdir.tistory.com/126](http://webdir.tistory.com/126) 여기가 간단명료한 것 같다.


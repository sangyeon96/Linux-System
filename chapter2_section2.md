# Section 2. 셸 커맨드

### 2.1 내장 커맨드란?

셸 자체에 포함된 커맨드를 **내장 커맨드\(내부 명령어\)**라고 부른다. 반대로 셸이 가진 기능이 아닌 별도의 프로그램을 **외부 명령어**라고 한다. 외부 명령어를 실행하려면 경로를 설정해 두거나 전체 경로\(full PATH\)를 입력하여 해당 프로그램을 실행해야만 한다.

내장 커맨드는 `type`으로 확인할 수 있다. 또한, bash 메뉴얼 `man bash`에서는 'SHELL BUILTIN COMMANDS'라는 항목에서 내장 커맨드를 확인할 수 있다.

참고로 bash 메뉴얼은 매우 길기 때문에.. `/`을 누른 후 shell builtin commands를 입력하면 해당 문자열이 포함된 부분을 찾을 수 있다.

이전 문자열 찾기는 `N`, 다음 문자열 찾기는 `n`을 입력하면 된다. 찾아보면 많은 내장 커맨드들을 확인할 수 있다.

![](/assets/type internal command%281%29.png)![](/assets/type internal command%282%29.png)

추가적으로 궁금한 명령어 몇 개를 더 해보았다.

![](/assets/type internal command%283%29.png)

밑의 이미지에서는 Chapter 1 - Section 2에서 다룬 리눅스의 디렉터리 중 시스템 관리자와 사용자가 사용하는 커맨드가 배치되어 있는 /bin 디렉터리에 있는 커맨드들을 `type` 명령어로 확인할 수 있었다.

![](/assets/type external command.png)

`ls`와 `grep`의 경우 is aliased to ~가 무슨 뜻인지는 모르겠다만 `type -a ls`, `type -a grep`으로 입력하면 /bin 디렉터리에 있음을 확인할 수 있다. 해당 Chapter의 Section 4에 의하면 alias는 셸 상의 별명을 뜻한다. ~/.bashrc 파일에 의해 `ls`라고 입력하면 `ls --color=auto`가, `grep`라고 입력하면 `grep --color=auto`가 실행되도록 설정되어 있다.

![](/assets/type -a.png)

### 2.2 주요 내장 커맨드

#### 쌍점\(:\)

아무것도 하지 않고 종료 코드 '0'을 반환한다.

test.sh라는 파일을 만들어 예제 2-1을 실행해보았다.

```bash
count=0
while :
do
    echo $count
    count=`expr $count + 1`
done
```

첫번째 줄에서 주의해야 할 점은 변수를 대입할 때 공백이 없어야한다는 점이다. 대입 시 공백을 넣으면 공백을 데이터로 처리해 오류가 난다.

두번째 줄에서는 `:`를 제대로 인식시키기 위해 while 뒤에 꼭 공백을 넣어줘야한다.

`echo`는 리눅스에서 출력을 하기 위해 사용하는 명령어이다. 셸 스크립트에서 변수에 대입된 값은 모두 문자열로 취급된다. 그래서 변수를 출력하는 경우 `$`를 이용하고, 문자열을 출력하는 경우 그냥 입력하거나 `" "`를 이용한다. 특히 `-e` 옵션으로 escaped characters를 이용하는 경우 `" "`를 이용한다.

`expr`는 연산자와 피연산자 사이에 공백을 주어야 제대로 계산된다. 공백을 주지 않으면 하나의 문자열로 인식한다. 그리고 수식을 묶어 준 것은 따옴표\('\)가 아니라 역따옴표\(\`\)다..그것도 모르고 실행했다가 expr $count + 1만 무한으로 출력되었다..count 변수에 문자열로 들어갔나보다..

여기서 살짝 의문인 점은 C언어에서 보면 while\(1\)에서 1이 true를 반환하여 무한반복하는데, 여기서는 0이 true를 반환하여 무한 루프를 돈다는 것이다. 이건 차차 알아가기로 하고..\(이 해답은 해당 Chapter의 Section 3에서 알 수 있다.\)

#### source 또는 온점\(.\)

현행 셸 환경에서 다른 셸 스크립트를 불러와 실행한다.

test.sh라는 파일에 다음을 입력한 후 `source`와 `.`을 실행해보았다.

```bash
year=2018
echo $year
```

현재 퍼미션이 user에게 rw-임에도 불구하고, `source` 또는 `.`명령어를 이용하면 파일을 실행할 수 있다.

![](/assets/source.png)

#### cd

change directory. 지정된 디렉터리로 이동한다.

콘솔 2-14에 있는 `export`는 환경변수를 설정할 때 사용하는 명령어이다.

`env | grep 변수`로 환경변수를 확인한다.

환경변수를 해제할 시에는 `unset` 명령어를 사용한다.

CENTOS 7에서는 dpkg directory가 존재하지 않아 ubuntu에서 실행해보았다.

![](/assets/cd.png)

#### eval

evaluation. 파라미터를 간접 참조한다.

![](/assets/eval echo.png)![](/assets/echo eval.png)

`\$$human` 또는 `${!human}` 또는 `${$human}`이 가능하다.

참고 URL : [https://wiki.kldp.org/HOWTO/html/Adv-Bash-Scr-HOWTO/ivr.htm](https://wiki.kldp.org/HOWTO/html/Adv-Bash-Scr-HOWTO/ivr.html)

#### exec

execution.

fork 시스템 콜을 사용하지 않고 지정된 커맨드를 스스로 실행한다. fork 시스템 콜을 사용하지 않기 때문에 프로세스가 생성되지 않고 셸 자신이 지정된 커맨드로 치환되어 실행된다. `exec`를 사용하지 않는 경우 커맨드가 실행된 후에는 셸로 실행이 돌아가지만 `exec`를 사용하면 돌아가지 않는다.

셸에서 `exec ls`를 실행하면, `ls` 명령어를 실행한 후 로그아웃된다.\(그 속도가 굉장히 빨라서 `ls` 명령어의 출력 결과를 보기 힘들다\)

![](/assets/exec ls.png)

다음은 `exec ls` 실행 후 종료되는 이유를 설명한 글이다.

If you run `ls`, your shell process will start up another process to run the  `ls` program, then it will wait for it to finish. When it finishes, control is returned to the shell.

With `exec ls`, you actually replace your shell program in the current process with the `ls` program so that, when it finishes, **there's no shell waiting for it**.

Most likely, you will have either a terminal program or `init` as the parent which is what will take over when your process exits. That's why your shell disappears, because you explicitly told it to.

#### read

표준 입력에서 1행 읽어들인다.

![](/assets/read.png)

다음은 키보드 입력이 아닌 텍스트 파일을 불러와서 `read` 명령어를 실행한 예제이다.

![](/assets/read text.png)

`while read line`은 그 자체로 많이 쓰이는 듯하다. 여기서 n은 0으로 초기화 되지도 않았는데 어떻게 1부터 시작하는지는 모르겠다0\_0

#### 그 외의 내장 커맨드

`exit`, `export`, `return`, `unset`, `pwd`, `logout`, `kill`

`pwd`\(pRINT wORKING dIRECTORY\)


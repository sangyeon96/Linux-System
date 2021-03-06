# Section 3. 셸 스크립트의 문법

### 3.1 셸 변수 선언

**셸 스크립트**란 실행하려는 커맨드를 나열해놓은 텍스트를 가리킨다. 확장자는 필수는 아니지만, 셸 스크립트인 것을 알 수 있도록 .sh를 붙이는 것이 일반적이다.

```bash
#!/bin/sh
echo "hello"
```

첫번째 줄 '\#!/bin/sh'를 **샤방\(shebang\)**이라고 부른다. shebang의 어원은 **Sh**arp **Bang**_ 혹은_ Ha**sh Bang**이다.

shebang에 관하여 자세한 내용은 다음 링크 참조 : [https://en.wikipedia.org/wiki/Shebang\_%28Unix%29\#Etymology\_and\_name\_history](https://en.wikipedia.org/wiki/Shebang_%28Unix%29#Etymology_and_name_history)

샤방을 '\#!/usr/bin/ruby'로 변경하여 실행해보면

![](/assets/:usr:bin:ruby.png)

ruby 인터프리터에 echo라는 메서드가 없기 때문에 실행 에러가 난다.

`./`는 현재 디렉터리를 나타내는 표현이다. 환경 변수 PATH를 표시했을 때,

![](/assets/$PATH.png)

현재 주소인

![](/assets/pwd.png)

/home/sangyeon은 PATH에 지정되어 있지 않기 때문에 `./`를 붙이지 않으면 실행되지 않는다. 그래서 한 번 PATH로 지정되어 있는 위치에서 `./`없이 실행해보았다.

![](/assets/execute without .:.png)

`./`없이 실행이 아주 잘된다:\)

### 3.2 셸의 변수

셸 변수의 종류에는 셸 변수, 환경 변수, 위치 파라미터, 특수 파라미터가 있다.

#### 셸 변수 : 구동 중인 셸, 셸 스크립트 내에서 이용할 수 있는 변수

변수 대입 시에는 `=`를 구분자로 쓰며 좌변의 변수명에 우변의 값을 대입한다. `=`의 앞뒤에 띄어쓰기가 들어가지 않도록 주의해야 한다.

```bash
VAL=16
```

셸 변수는 변수명 앞에 `$`를 붙임으로써 값을 참조할 수 있다. 그러므로 `echo VAL`이라 입력하면 문자열로 인식해 VAL이 출력되고, `echo $VAL`이라 입력하면 VAL의 값인 16이 출력된다.

사용 중인 셸에서 유효한 셸 변수는 `set` 명령어를 사용해 리스트로 표시할 수 있다. 매우 기므로 `set | grep '찾으려는 셸 변수'`로 입력하는게 좋다.

셸 변수는 `unset` 명령어를 사용해 삭제할 수 있다.

#### 환경 변수 : 여러 커맨드가 공유할 수 있는 변수, bash의 내장 커맨드 export로 변수를 생성, 지정하면 환경 변수로 참조할 수 있게 됨

**정의한 셸에서 구동된 커맨드에도 적용되는 변수**를 **환경 변수**라고 한다. `export`로 셸 변수를 환경 변수로 바꿀 수 있다.

셸 변수는 사용 중인 셸에서만 유효한 반면, 환경 변수는 해당 프로세스의 자식 프로세스까지 적용된다.

`bash` 명령어를 통해 자식 프로세스에 bash 구동을 할 수 있다.\(bash에 관해 더 자세한 건 아직 모르겠다\)

`env` 커맨드로 설정되어 있는 환경 변수들을 확인할 수 있다. 매우 기므로 `env | grep '찾으려는 환경변수'`로 입력하는게 좋다.

환경 변수도 셸 변수와 같이 `unset` 명령어를 사용해 삭제할 수 있다.

bash에서 이용되는 주요 환경 변수이다. 다음은 `man bash`에서 가져온 내용이다.

![](/assets/bash_PPID.png)

![](/assets/bash_PWD.png)

![](/assets/bash_OLDPWD.png)

![](/assets/bash_UID.png)

![](/assets/bash_HISTFILE.png)

![](/assets/bash_IFS.png)

![](/assets/bash_PATH.png)

![](/assets/bash_HOME.png)

#### 위치 파라미터 : 함수 등의 인수\(argument\) 참조에 사용하는 변수

셸 스크립트를 실행할 때 인수 값을 부여하지 않는 경우에는 그 값을 셸 스크립트 내에서 취득하고자 위치 파라미터를 이용한다. 위치 파라미터는 1 이상의 정수 앞에 `$`를 붙인다.

다음은 count.sh 파일이다.

```bash
#!/bin/bash
echo $1
echo $2
echo $3
```

`bash ./count.sh a b c d`를 실행했을 때, 파일에서 첫번째 인수\(`$1`\)인 a, 두번째 인수\(`$2`\)인 b, 세번째 인수\(`$3`\)인 c를 출력한다.

`$0`부터 시작하지 않는 것은 `$0` 위치에 ./count.sh가 들어가기 때문이다.

셸 스크립트 내에서 셸 함수를 작성한 경우, 다음과 같이 위치 파라미터를 이용해 함수에서 취득할 수 있다.

```bash
#!/bin/bash

count() {
    echo $1
    echo $2
    echo $3
}

count a b c d
```

이 경우 이미 셸 스크립트 내에서 인수를 포함한 함수 실행문을 넣었으므로 `bash ./count.sh`를 실행하면 위와 같은 결과를 얻을 수 있다.

위치 파라미터를 이동하고 싶을 때는 인수 없이 shift만으로 하나씩 이동이 가능하다.정수를 인수로 주면 그 값만큼 이동할 수도 있다.

```
#!/bin/bash

count() {
    echo $1
    echo $2
    shift 3
    echo $3
}

count a b c d e f
```

실행하면 첫번째 인수\(`$1`\)인 a, 두번째 인수\(`$2`\)인 b, shift 3번해서 c, d, e를 제껴서 세번째 인수\(`$3`\)인 f를 출력한다.

#### 특수 파라미터 : 인수 전체나 인수의 전체 개수 등을 나타내는 변수

특수 파라미터는 스크립트 이름이나 인수, 프로세스 ID 등을 참조하기 위한 변수로 이용한다. 주요 파라미터는 다음과 같다. 이들 변수의 사용법은 5장에서 해설할 시스템 부팅 스크립트를 읽고 나면 자연스럽게 익혀질 것이라고 한다..

`$0`, `$@`, `$*`, `$#`, `$?`, `$!`, `$$`, `$-`, `$_`

예제들을 찾아보니 자주 쓰는 특수 파라미터는 `$@`\(모든 인수 참조\), `$#`\(인수 개수를 참조\)인 듯 하다.

`$0`은 '셸 스크립트 이름 참조'라고 설명에 씌어있지만, 따로 설명을 외우지 않아도 파일을 실행할 때 `bash ./count.sh arg1 arg2 ~` 이렇게 입력하기 때문에 위치상 `$0`은 셸 스크립트 이름임을 알 수 있다. 그나저나 `bash` 명령어 넣으면 `./` 안 쓰고 `bash count.sh arg1 arg2 ~` 이렇게 써도 되더라.

![](/assets/positional parameters.png)

`$*`와 `$@`이 실행 결과상 똑같아 보일 수 있는데, 둘의 차이는 모든 인수를 하나의 문자열로 보느냐\(`$*`\) 각각 개개인의 인수로 보느냐\(`$@`\)에 있다. 그건 여길 참조하도록. [http://jybaek.tistory.com/477](http://jybaek.tistory.com/477)

### 3.3 if를 이용한 조건문

```bash
if 리스트1; then 리스트2;
elif 리스트3; then 리스트4;
else 리스트5; fi
```

elif\(else if\), fi\(finish\)

```bash
if [-f /etc/hostname ]; then
    cat /etc/hostname
elif [-z /etc/hostname ]; then
    echo "localhost.localdomain" > /etc/hostname
else
    echo "/etc/hostname not found"
fi
```

`-f`는 /etc/hostname 파일이 존재하면 참\(0\)을 반환한다. `-z`는 /etc/hostname의 크기가 0 byte이면 참\(0\)을 반환한다.

![](/assets/if.png)

첫번째 조건의 실행문이 실행된 걸로 보아 /etc/hostname이 존재함을 알 수 있었다.\(실제로 /etc에서 `ls | grep 'hostname'` 해보니 hostname이 있었다\)

여기서 조건식에 대해 더 자세히 알아보자면 조건식에는 셸 내부 커맨드 `test`나 `[ ]`를 사용한다. 기술 방법은 다음과 같다.

```bash
기술법(1) :test 조건식
기술법(2) :[조건식]
```

`[ ]`는 test의 별명일 뿐 동작은 같다. 조건식은 파일이나 변수의 상태, 두 개의 변수 및 문자열의 비교 평가 결과가 참이면 '0', '거짓이면 '1'로 종료 상태를 반환한다.\(참이면 0, 거짓이면 1을 반환한다는게 그동안 배워왔던 거랑 반대다..이유는 [https://stackoverflow.com/questions/2933843/why-0-is-true-but-false-is-1-in-the-shell ](https://stackoverflow.com/questions/2933843/why-0-is-true-but-false-is-1-in-the-shell)참고\)

`test`의 조건 연산자는 다음과 같다.

![](/assets/test command_file.jpg)

![](/assets/test command_string.jpg)

![](/assets/test command_relational.jpg)

![](/assets/test command_boolean.jpg)

출처 : [http://slideplayer.com/slide/5291601/](http://slideplayer.com/slide/5291601/)

산술연산에는 `(( ))`와 함께 `+`, `-`, `<`, `>`, `&&`, `||` 등을, 논리연산에는 `[[ ]]`와 함께 `&&`, `||`, `==`, `!=`을 연산자로 사용할 수 있지만, 이들은 bash용 표기이므로 이식성\(portability\)을 고려한다면 `test`와 `[ ]`를 사용하는 것이 무난하다고 한다.

### 3.4 while/for를 사용한 반복문\(루프\)

#### while문을 이용한 커맨드의 반복\(루프\)

```bash
while 리스트1; do 리스트2; done
```

다음은 while문을 이용한 예제이다.

```bash
NUM=1
while test "$NUM" -le "10"
do
    echo "Num in $Num"
    Num=$[Num+1]
done
```

참고로 `-lt`는 less than이고 `-le`는 less than or equal to이므로 NUM이 10일 때에도 참\(0\)이 반환되어 "Num in 10"까지 출력된다.

실행하는 각 커맨드의 행 끝에 쌍반점`;`을 붙이면 다음과 같은 방법으로 여러 개의 커맨드를 실행시킬 수 있다.

```bash
NUM=1; while test "$NUM" -le "10"; do echo "Num in $Num"; Num=$[Num+1]; done
```

#### for문을 이용한 커맨드의 반복\(루프\)

```bash
1) for 변수 [in 값 [값...]]; do 리스트; done
2) for (( 식1; 식2; 식3 )) do 리스트; done
```

다음은 1\)을 이용한 예제이다. 이건 사진을 이용해야해서 GUI환경인 우분투에서 실행해보았다.

우분투에서 `man convert`로 확인해보니 회전 옵션명이 -rotate degrees이었다. Downloads 폴더에 이미지 2개 준비해놓고 예제를 실행해보니 for문으로 이미지를 하나씩 불러와 180도 회전하고 이미지명에 '180rotated-'를 붙힌 이미지를 얻을 수 있었다.![](/assets/for%281%29.png)다음은 2\)을 이용한 예제이다.

![](/assets/for%282%29.png)

이건 뭐 표현하는 방식이 약간 다를 뿐이지 많이 봐왔던 for문이라 설명은 패스.

#### break로 루프 해제,  continue로 계속 실행

break는 for, while, select의 루프에서 빠져나올 때 쓰는 구문이다. break는 인수로 숫자를 쓸 수 있으며 빠져나올 루프의 수를 지정한다.

continue는 for, while, select에서 현재 루프를 건너뛰고 다음 지점부터 계속 루프를 돌리는 것이다. 비슷한 커맨드로는 return이나 exit이 있다. 다만, return은 셸 함수에서 빠져나오는 것이고, exit은 셸을 종료하는 것이다.

```bash
for i in 1 2 3; do
    echo "[i=$i] Start"
    if test $i -eq 2; then
        echo "No j when i is 2"
        continue
    fi
    for j in 1 2 3 4; do
        if test $j -eq 3; then
            echo "[j=$j] It is 3. Break."
            break
        else
            echo "[j=$j] It is $j."
        fi
    done
    echo "[i=$i] End"
done
```

![](/assets/break, continue.png)

실행 결과를 통해 `for j in 1 2 3 4`임에도 불구하고 j의 값이 3일 때 break를 해서 j의 for문에서 빠져나와\(done으로 어디서 끝나는지 알 수 있다\) 그 다음 실행문을 실행하는 걸 볼 수 있다. 또한 i의 값이 2일 때, continue 밑의 실행문들을 실행하지 않고 i=3일 때의 for문으로 넘어갔음을 알 수 있다.

#### 와일드카드를 이용한 반복

셸에서는 와일드카드 `*`를 사용해 여러 개의 파일을 다룰 수 있다.

```
for i in *.txt; do
    ls -lh $i
done
```

현재 위치에 a.txt, b.txt, c.txt라는 파일이 있다고할 때, 실행결과는 다음과 같다.

![](/assets/wild card *.png)

### 3.5 조건 분기

다음은 특정 조건에 맞아떨어질 때에 처리를 실행하는 조건 분기이다.

#### case문

```bash
case 문자열 in
    패턴1)
        리스트1
        ;;
    패턴2|패턴3)
        리스트2
        ;;
    *)
        default
        ;;
esac
```

esac이 뭐의 줄임말인가 궁금해서 찾아봤더니 case spelled backward란다..아..그렇네..

문자열이 패턴1에 해당하면 리스트1이 처리된다. 패턴2나 패턴3에 해당하면 리스트2가 처리된다. `|`를 사이에 넣음으로써 패턴을 여러 개 설정할 수 있다. 예제는 다음과 같다.

```bash
case $1 in
    --help|-h)
        print_help()
        ;;
    --version|-v)
        print_version()
        ;;
    *.txt)
        for i in $1; do
        sed -i -e "s/Perl/Ruby/g" $1
        done
        ;;
    *)
        echo "Usage: $0 [--version|-v|--help|-h|FILE.txt]"
```

`sed`는 특정 문자열을 변경할 때 쓰는 커맨드이다.

### 3.6 셸 함수

#### function문으로 함수를 이용한다.

함수는 `{ }`로 둘러싼다. 행 첫머리의 function에서 'function'은 생략할 수 있지만 `{ }`를 생략하면 함수가 되지 않는다.

셸 함수도 종료 상태를 반환한다. return으로 반환값을 명시적으로 지정할 수 없는 경우에는 가장 마지막에 실행된 커맨드의 반환값을 반환한다.

`$?`변수로 함수의 반환값을 얻을 수 있다.

#### shift를 이용해 커맨드 라인 인수를 처리한다.

여러 개의 인수를 처리하고 싶을 때 유용하다. 순서를 고정하는 것도 좋지만, 자신 이외의 사용자가 쓸 때에도 이러한 방법이 통용될지 알 수 없다. 이때 shift를 이용하면 위치 파라미터를 하나씩 이동할 수 있다. 이 경우 -h, -v, \*.txt가 어느 시점에 들어오더라도 처리할 수 있다.

#### 작은따옴표\('\)와 큰따옴표\("\)의 차이

$1000을 예로 들자면, 작은따옴표\('\)는 텍스트를 셸이 해석하지 않게 만들어 특수 문자를 $1000 그대로 표시, 이용할 수 있다. 반면 큰따옴표\("\)로 감싼 텍스트는 $1을 특수 문자로 취급하기 때문에 $1이 빠진 000밖에 표시되지 않는다.

![](/assets/escape.png)

작은따옴표는 큰따옴표보다 강하기 때문에 큰따옴표로 감쌀 때는 작은따옴표를 밖으로 빼야한다.

작은따옴표 안에서 작은따옴표를 쓰고 싶을 때에는 한 번 작은따옴표를 닫고 `\`로 빠져나온 다음 작은따옴표를 표시하고 그 후 다시 작은따옴표로 텍스트를 지정해야 한다. 셸 내에서 `\`를 사용하면 바로 뒤에 오는 문자의 특별한 의미를 무력화시킨다. 이처럼 특수 문자를 무력화하는 것을 이스케이프\(escape\)한다고 부른다.

#### 역따옴표\(\`\)

역따옴표\(\`\)는 작은따옴표와는 달리 셸 내에서 특별한 의미가 있다. 역따옴표로 감싼 텍스트는 커맨드로 실행되어 출력된다. 해당 Chapter의 Section 2에서 예시로 `count = 'expr $count + 1'`가 있었다.

    1) TARGET=`ls -l | grep *.txt`
    2) TARGET=$(ls -l | grep *.txt)

변수 TARGET에는 `ls -l | grep *.txt`라는 결과가 들어있다. 셸 스크립트 안에서 커맨드를 실행하고 그 결과를 이용하고 싶을 때에 사용한다. 하지만, 역따옴표는 작은따옴표와 비슷해서 혼동할 가능성이 크므로 대신 2\)처럼 `$( )`으로 커맨드를 감싸는 방법도 있다.


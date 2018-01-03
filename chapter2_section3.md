# Section 3. 셸 스크립트의 문법

### 3.1 셸 변수 선언

**셸 스크립트**란 실행하려는 커맨드를 나열해놓은 텍스트를 가리킨다. 확장자는 필수는 아니지만, 셸 스크립트인 것을 알 수 있도록 .sh를 붙이는 것이 일반적이다.

```
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

##### 셸 변수 : 구동 중인 셸, 셸 스크립트 내에서 이용할 수 있는 변수

변수 대입 시에는 `=`를 구분자로 쓰며 좌변의 변수명에 우변의 값을 대입한다. `=`의 앞뒤에 띄어쓰기가 들어가지 않도록 주의해야 한다.

```
VAL=16
```

셸 변수는 변수명 앞에 `$`를 붙임으로써 값을 참조할 수 있다. 그러므로 `echo VAL`이라 입력하면 문자열로 인식해 VAL이 출력되고, `echo $VAL`이라 입력하면 VAL의 값인 16이 출력된다.

사용 중인 셸에서 유효한 셸 변수는 `set` 명령어를 사용해 리스트로 표시할 수 있다. 매우 기므로 `set | grep '찾으려는 셸 변수'`로 입력하는게 좋다.

셸 변수는 `unset` 명령어를 사용해 삭제할 수 있다.

##### 환경 변수 : 여러 커맨드가 공유할 수 있는 변수, bash의 내장 커맨드 export로 변수를 생성, 지정하면 환경 변수로 참조할 수 있게 됨

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

##### 위치 파라미터 : 함수 등의 인수\(argument\) 참조에 사용하는 변수

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

```
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

##### 특수 파라미터 : 인수 전체나 인수의 전체 개수 등을 나타내는 변수

특수 파라미터는 스크립트 이름이나 인수, 프로세스 ID 등을 참조하기 위한 변수로 이용한다. 주요 파라미터는 다음과 같다. 이들 변수의 사용법은 5장에서 해설할 시스템 부팅 스크립트를 읽고 나면 자연스럽게 익혀질 것이라고 한다..

`$0`, `$@`, `$*`, `$#`, `$?`, `$!`, `$$`, `$-`, `$_`

예제들을 찾아보니 자주 쓰는 특수 파라미터는 `$@`\(모든 인수 참조\), `$#`\(인수 개수를 참조\)인 듯 하다.

`$0`은 '셸 스크립트 이름 참조'라고 설명에 씌어있지만, 따로 설명을 외우지 않아도 파일을 실행할 때 `bash ./count.sh arg1 arg2 ~` 이렇게 입력하기 때문에 위치상 `$0`은 셸 스크립트 이름임을 알 수 있다. 그나저나 `bash` 명령어 넣으면 `./` 안 쓰고 `bash count.sh arg1 arg2 ~` 이렇게 써도 되더라.

![](/assets/positional parameters.png)

`$*`와 `$@`이 실행 결과상 똑같아 보일 수 있는데, 둘의 차이는 모든 인수를 하나의 문자열로 보느냐\(`$*`\) 각각 개개인의 인수로 보느냐\(`$@`\)에 있다. 그건 여길 참조하도록. [http://jybaek.tistory.com/477](http://jybaek.tistory.com/477)

### 3.3 if를 이용한 조건문

```
if 리스트1; then 리스트2;
elif 리스트3; then 리스트4;
else 리스트5; fi
```

elif\(else if\), fi\(finish\)

```
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

```
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

##### while문을 이용한 커맨드의 반복\(루프\)

```
while 리스트1; do 리스트2; done
```

다음은 while문을 이용한 예제이다.

```
NUM=1
while test "$NUM" -le "10"
do
    echo "Num in $Num"
    Num=$[Num+1]
done
```

참고로 `-lt`는 less than이고 `-le`는 less than or equal to이므로 NUM이 10일 때에도 참\(0\)이 반환되어 "Num in 10"까지 출력된다.

실행하는 각 커맨드의 행 끝에 쌍반점`;`을 붙이면 다음과 같은 방법으로 여러 개의 커맨드를 실행시킬 수 있다.

```
NUM=1; while test "$NUM" -le "10"; do echo "Num in $Num"; Num=$[Num+1]; done
```

##### for문을 이용한 커맨드의 반복\(루프\)

```
1) for 변수 [in 값 [값...]]; do 리스트; done
2) for (( 식1; 식2; 식3 )) do 리스트; done
```

다음은 1\)을 이용한 예제이다.

![](/assets/for%281%29.png)

CENTOS 7에서는 convert 명령어가 없어서 실행이 되지 않는다. ubuntu에서도 -rotate가 없는 옵션이라고 한다. 그런데 책에서는 그냥 for문에 대한 설명만 있고 do 뒤의 커맨드를 실행한다. 라고 써있어서 뭔지 모르겠다..패스.

다음은 2\)을 이용한 예제이다.

![](/assets/for%282%29.png)

이건 뭐 표현하는 방식이 약간 다를 뿐이지 많이 봐왔던 for문이라 설명은 패스.

##### break로 루프 해제,  continue로 계속 실행

break는 for, while, select의 루프에서 빠져나올 때 쓰는 구문이다. break는 인수로 숫자를 쓸 수 있으며 빠져나올 루프의 수를 지정한다.

예제 2-8은 왜 계속 4번째줄 fi에서 syntax error가 날까..

예제 2-9도..

continue는 for, while, select에서 현재 루프를 건너뛰고 다음 지점부터 계속 루프를 돌리는 것이다. 비슷한 커맨드로는 return이나 exit이 있다. 다만, return은 셸 함수에서 빠져나오는 것이고, exit은 셸을 종료하는 것이다.

##### 와일드카드를 이용한 반복

셸에서는 와일드카드 \*를 사용해 여러 개의 파일을 다룰 수 있다.

```
for i in *.txt; do
    ls -lh $i
done
```

현재 위치에 a.txt, b.txt, c.txt라는 파일이 있다고할 때, 실행결과는 다음과 같다.

![](/assets/wild card *.png)

### 3.5 조건 분기

### 3.6 셸 함수




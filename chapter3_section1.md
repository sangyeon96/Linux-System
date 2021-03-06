# Section 1. 리눅스 커널 개요

### 1.1 리눅스 커널이란?

리눅스 커널은 OS의 중추 역할을 하며 주로 디바이스 관리, 프로세스 관리, 메모리 관리, 시스템 콜 제공과 같은 기능을 가지고 있다.

#### 디바이스 관리

리눅스 커널은 디바이스 드라이버라는 하드웨어 입출력을 제어하는 소프트웨어를 이용하여 장치를 관리한다.

#### 프로세스 관리

리눅스에서는 프로그램을 실행할 때 파일 시스템 내 특정 디렉터리에 있는 프로그램의 파일을 읽어와 메모리에 적재한다. 이 프로그램은 메모리상에서 실행되는 **프로세스**가 된다. 프로그램 실행이 종료되면 메모리에 복사되었던 프로세스 파일은 삭제된다.

사용자가 시스템에 로그인하게 되면 보통 약 100여 개의 프로세스가 동시에 실행된다. 하지만, 프로세스가 이용할 수 있는 CPU는 하나이므로 여러 프로세스를 동시에 이용할 수는 없다. 따라서 동시에 실행되는 프로세스 간에 CPU를 이용하는 시간을 분배하게 된다. 각 프로세스에는 프로세스 ID\(PID\)가 부여되어 **시스템은 이 PID를 통해 프로세스를 관리**한다.

프로세스는 파일과 마찬가지로 항상 **소유자**가 있다. 프로세스는 특정 사용자의 권한으로 실행되어 **퍼미션**을 확인한 후 실행, 처리한다. 리눅스 커널은 이러한 프로세스 관리도 담당한다.

터미널에서 커맨드를 실행할 때에는 `Ctrl`+`C`을 눌러 프로세스를 중지하거나 `Ctrl`+`Z`로 일시 중지를 하게 되는데, 이것은 사용자가 **셸에서 프로세스로 신호를 보내는 것**이다.

이벤트를 받은 프로세스는 받은 이벤트의 종류에 따라 처리를 실행한다. 프로세스 측의 타이밍에 따라 신호를 무시하거나 수신한 신호로 지정된 것과는 다른 처리를 하는 신호 핸들러가 등록된 때도 있다. **신호**의 종류는 signal\(7\)로 확인할 수 있다. \([http://man7.org/linux/man-pages/man7/signal.7.html](http://man7.org/linux/man-pages/man7/signal.7.html)\)

`kill -l`로 CENTOS 7 시스템이 지원하는 신호를 확인해보니 다음과 같다.

![](/assets/signal.png)

#### 메모리 관리

프로그램 실행 시에 메모리에는 프로그램만 있는 것이 아니다. 프로그램이 이용하는 데이터 영역도 동시에 메모리에 할당된다. 사용자 프로그램의 요구에 따라 메모리 영역을 분배하거나 이용이 끝난 메모리 영역 회수 등을 담당하는 것이 메모리 관리 기능이다.

메모리 관리에서는 가상 메모리가 지원되고 있다. **가상 메모리**라는 것은 실제 메모리로의 접근에 대하여 리눅스 커널을 덧씌운 가상 기억 영역을 프로그램에 제공하는 것을 말한다. 이 가상 기억 영역은 실제 메모리가 아닌 HDD와 같은 보조 기억 장치의 일부도 프로세스가 볼 때에는 같은 메모리인 것처럼 보이게 하기 때문에 실제 내장된 메모리보다 더 큰 용량의 메모리를 쓸 수 있게 한다. 이 HDD에 마련된 가상 메모리 영역을 '**스왑\(swap\)**'이라고 한다. 메모리에 있는 프로그램 전체 영역을 보조 기억 장치로 내보내는 것을 **스왑 아웃\(swap out\)**, 반대로 내보냈던 데이터를 메모리에 다시 가져오는 것을 **스왑 인\(swap in\)**이라고 한다.

![](/assets/swapping.jpg)

#### 시스템 콜 제공

시스템 콜이란 표준 출력이나 파일을 쓰는 write, 읽어들이는 read, 프로세스를 포크\(fork\)하는 기능 등을 가지고 있어 사용자 프로그램에서 액세스할 수 있도록 돕는다. 리눅스 커널에서는 약 300개의 시스템 콜을 제공하고 있다.

시스템 콜을 이용하려면 인터럽트\(interrupt\)를 통해 CPU에 대한 명령을 직접 어셈블리로 작성해야 한다. 사용자의 프로그램에 이것까지 모두 담기에는 이식성이나 유지관리 면을 고려할 때 어렵기 때문에 시스템 콜을 간단히 이용할 수 있도록 C 라이브러리 함수가 시스템에 준비되어 있다.

리눅스 환경에서 C 라이브러리 함수라는 것은 **glibc**\(GNU C Library\)를 가리킨다. C언어 입문에서 누구나 접하게 되는 함수 printf\(\)도 실행 시에는 glibc를 통해 시스템 콜 write를 이용하여 표준 출력에 문자열로 출력된다. p.112 그림 참조.

GNU C Library is a wrapper around the system calls of the Linux kernel.

### 1.2 시스템 부팅 절차

부팅 과정은 BIOS -&gt; MBR 읽기 -&gt; 부트 로더 실행 -&gt; 커널 구동 순이다.

\(참고 : [http://duartes.org/gustavo/blog/post/how-computers-boot-up/](http://duartes.org/gustavo/blog/post/how-computers-boot-up/)\)

#### BIOS

PC의 전원을 켜면 BIOS\(Basic Input Output System\)가 머더보드 상의 ROM에서 메모리로 복사되어 실행된다. BIOS는 입력과 출력을 실행하는 소프트웨어로, 다음과 같은 기능을 갖고있다.

* 머더보드에 접속된 장치의 초기화나 PC 부팅 설정 
* 부팅 순서 변경 ex\) HDD 우선 부팅을 CD-ROM 우선 부팅으로
* SATA RAID의 유효/무효화 전환
* 접속 장치를 무효화
* POST\(Power-On Self Test\)라는 각 하드웨어의 자가 진단 시행

#### MBR

BIOS에서 POST가 실행된 뒤 부트 디바이스\(HDD나 CD-ROM\)의 첫 섹터인 MBR\(Master Boot Record\)에 부팅 프로그램이 있는지 검색한다.

MBR은 부팅 장치의 첫 512byte의 영역에 해당한다. 첫 446byte\(0~445\)까지가 **부트 로더**, 그 이후 64byte\(446~509\)가 **파티션 테이블**, 남은 2byte\(510,511\)가 **부트 시그니처**이다.

![](/assets/structure of MBR.jpg)

파티션 테이블에는 스토리지의 파티션 구성 정보가 들어있다. 이 영역에는 파티션 정보를 4개까지 쓸 수 있다\(논리 파티션\). 이 때문에 원칙적으로 파티션은 최대 4개까지만 분할 가능하다. 4개 이상의 파티션을 생성하는 경우에는 하나의 파티션을 확장 파티션으로 만들면 된다.

부트 시그니처에는 '0xAA55'가 들어가며, 이것이 없으면 MBR로서 인식되지 않는다.\(순서로 봤을 때 LSB first인 Little Endian방식인가 보다. 그러고보니 리눅스는 Little Endian방식이었다! 갑자기 생각났다.\)

무사히 MBR에서 부트 로더가 발견되면 이 프로그램을 메모리의 '0x7c00'에 쓴 다음 실행한다.

\(왜 0x7c00인지 찾아봤더니 컴퓨터의 역사\(?\)와 관련이 있다. 궁금하면 읽어보도록. [https://www.glamenv-septzen.net/en/view/6](https://www.glamenv-septzen.net/en/view/6)\)

#### 부트 로더

부트 로더는 **커널을 실행하기 위한 프로그램**이다. 부트 로더는 저장 공간 크기가 446byte밖에 되지 않아서 커널을 수용할 수 없다. 커널 본체는 HDD나 USB 메모리, CD-ROM과 같은 대용량 장치에 들어가는 것이 일반적이다. 부트 로더는 대용량 장치에서 부팅되는 커널을 찾아 메모리에 복사하여 커널의 셋업 루틴을 호출하는 역할을 한다. 정확하게는 MBR의 부트 로더\(1차 부트 로더\)는 대용량 장치에 있는 2차 부트 로더를 부팅시키고 여기에서 리눅스 커널을 부팅하는 것이다.

#### 리눅스 커널 파일의 구성

부트 로더에서 불러온 커널은 우선 자신의 **셋업 루틴**을 호출하게 된다. 커널의 셋업 루틴은 구동되면 하드웨어 초기화를 실시한다. 메모리의 크기, 입력 장치, VGA, HDD 등의 정보를 얻고 커널의 **압축 해제 루틴**을 호출한다. 다음으로는 압축되어 있는 커널 이미지의 압축을 풀어 커널을 실행한다. 그 후 부트 로더의 리눅스 커널 옵션 `init=`에 지정된 프로그램을 커널이 실행한다. 특별한 지정이 없으면 /sbin/init을 실행한다.


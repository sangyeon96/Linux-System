# Section 2. 리눅스에서 이용하는 부트 로더

### 2.1 GRUB

GRUB\(GRand Unified Bootloader\)는 GNU Project의 하나로 개발된 부트 로더이다. 부트 로더에 작은 로드 프로그램을 적재하여 부팅 시에 메모리가 이를 읽어들이게 함으로써 커널의 위치를 파악하여 커널을 구동시킨다.

GRUB는 두 개의 버전이 유통되고 있다. version 1.0계열의 GRUB 1\(GRUB Legacy\)과 GRUB version 2.0계열의 GRUB 2이다.

CENTOS 7은 GRUB 2인거 같다.

GRUB 2에서는 다음과 같이 변경되었다.

* 설정 파일 menu.lst -&gt; grub.cfg

* 파티션 번호 시작 0 -&gt; 1

GRUB는 MBR의 512byte에 **Stage 1 로더**를 먼저 구동한다. Stage 1부터 다음으로 Stage 1.5 또는 Stage 2 로더를 구동한다. **Stage 1.5 로더**는 파일 시스템을 해석하는 드라이버를 가지고 있으며 Stage 1에서 직접 Stage 2에 액세스 할 수 없을 때 이용한다. **Stage 2 로더**는 파일 시스템에 있는 GRUB의 설정 파일을 읽어들여 사용자에게 부팅 후보를 표시한다.

**GRUB는 셸을 포함하고 있다**는 것도 사용자에게는 중요한 점이다. 셸 모드로 진입함으로써 커널 파라미터의 추가나 변경, 초기 RAM 이미지 지정 변경 등의 작업을 더욱 쉽게 수행할 수 있다. GRUB 셸은 자주 사용하지는 않지만, 시스템 관리나 배포판 표준 커널에서는 지원하지 않는 부품을 사용하는 PC 등의 문제 해결에 활용할 수 있으니 기억해 두라고 책에 씌어있다.

### 2.2 GRUB Shell

부팅 시 부팅 목록이 표시될 때 `C`를 누르면 셸 모드로 진입한다.

![](/assets/GRUB Shell.png)

주요 커맨드는 set root, kernel/linux, initrd, boot 다음 4개이다.

#### set root

![](/assets/set root.png)

GRUB 2는 파티션을 1부터 카운트한다던데 밑에 possible partitions를 통해 알 수 있다.

#### kernel\(GRUB 1\)/linux\(GRUB 2\)

구동할 커널을 지정한다. GRUB 1에서는 kernel, GRUB 2에서는 linux라는 커맨드를 사용한다.

#### initrd\(init ram disk\)

초기 RAM 디스크 이미지를 지정한다. 초기 RAM 이미지는 메모리상에 일시적으로 적재되는 루트 파일 시스템을 지칭한다. 이 이미지를 사용해 실제 루트 파일 시스템을 마운트한다.\(Chapter 1의 Section 2에서 2.3 /boot 참고\)

#### boot

지금까지 입력한 커널 패스와 옵션 정보를 바탕으로 실제 커널을 부팅하는 커맨드이다.


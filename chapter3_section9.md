# Section 9. UNIX 디바이스

### 9.1 디바이스 파일의 종류

하드웨어 디바이스에는 HDD처럼 랜덤 액세스가 가능한 블록 디바이스와 마우스나 단말기처럼 직선적으로 액세스하는 캐릭터 디바이스가 있다.

#### 블록 디바이스

블록 디바이스란 **거대한 데이터 블록을 주고받는 디바이스**를 뜻한다\(Chapter 3의 Section 4에서 이미 언급했었음\). **버퍼**라고 하는 임시 저장소를 매개로 읽기, 쓰기 작업이 이루어진다. 버퍼에 어느 정도의 블록 데이터가 쌓이면 이 버퍼의 내용이 디바이스에 데이터로 전송되고 버퍼 안의 데이터는 삭제된다.

데이터에 액세스하는 경우에는 **임의 접근\(Random access\) 방식**으로 대상 데이터가 들어 있는 블록에 접근하게 되는데, 임의라는 것이 '아무렇게나'라는 뜻은 아니다. 순차접근\(Sequential access\)이라고 하는, 즉 디바이스의 처음부터 끝까지 차례대로 검색하여 대상 데이터가 들어 있는 블록이 나올 때까지 기다리는 방식이 아니라, 직접 데이터 위치를 찾아 대상 블록을 쓰거나 읽는 것을 의미한다.

![](/assets/block device.png)

하드디스크\(/dev/sda\*\)나 CD드라이브\(/dev/cdrom\), DVD\(/dev/dvd\), 메모리 영역\(/dev/ram\*\) 등이 블록 디바이스에 해당한다.

#### 캐릭터 디바이스

캐릭터 디바이스란 **byte 단위로 액세스하는 디바이스**를 말한다. 캐릭터 디바이스는 `ls`로 확인해보면 앞글자가 c로 시작한다. 캐릭터 디바이스는 블록 디바이스처럼 **버퍼를 거치지 않고** 1byte씩 디바이스에 전송한다.

![](/assets/character device.png)

캐릭터 디바이스에는 PS/2나 USB 마우스\(/dev/psaux\), 가상 터미널\(/dev/pty/\*\), 사운드 카드 등이 해당한다. 또한, 입력한 것을 모두 버리는 /dev/null, 0\(ZERO\)를 만드는 /dev/zero, 블록 가상 난수 발생기\(Random number generator\)인 /dev/random, 블록이 아닌 가상 난수 발생기 /dev/urandom도 캐릭터 디바이스이다.

### 9.2 메이저 번호와 마이너 번호

디바이스는 메이저 번호와 마이너 번호로 각각 구별된다. 리눅스 커널 소스에 들어 있는 **Documentation/device.txt**를 보면 메이저 번호 '8'은 SCSI 디바이스에 할당되어 있으며 '5'는 캐릭터 디바이스의 tty에 할당되어 있고 HP의 서버에 탑재된 SmartArray에서 인식할 수 있는 디바이스 /dev/cciss/는 '104'로 할당되는 등 자세한 내용이 설명되어 있다. 사용자로서는 그다지 접할 일이 없는 번호지만, 이렇게나 많은 디바이스 정보를 리눅스 커널과 드라이버 개발자가 정리하여 공유하고 있다는 것을 알 수 있다.

메이저 번호는 디바이스를 구별하는 번호이다. 예를 들어 SCSI 디스크의 메이저 번호는 8번이다. 마이너 번호는 해당 디바이스를 기기 별로 구별하는 번호이다. SCSI 디스크의 첫 번째 파티션 디바이스는 1, 두 번째는 2, 이와 같은 식으로 번호가 붙여져 있다.

![](/assets/device major numbers.png)


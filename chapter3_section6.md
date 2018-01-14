# Section 6. 리눅스 커널 구축

리눅스 커널은 kernel.org 웹 사이트에서 소스 코드를 배포하고 있다. 이 소스 코드를 이용하여 사용자가 이용하려는 기능, PC나 사용 부품에 필요한 모듈을 내장한 커널을 빌드\(구축\)해볼 것이다. 커널은 kernel.org에서 배포된 후에 발견된 security hole\(보안 취약점\)에 대한 대응 패치나 배포 후 지원했던 하드웨어 드라이버의 backport\(이식\), 배포판 자체의 기능 확장 패치 등을 패키지로 제공하고 있다. 여기에서는 kernel.org에서 소스 코드를 내려받아 커널에 빌드하는 과정, 배포자가 만든 커널 패키지를 재설정하여 빌드하는 과정을 살펴볼 것이다.

커널 빌드 과정은 크게 다음과 같은 순서로 이루어진다.

1. 커널 소스 코드 준비
2. 빌드용 설정
3. 빌드, 설치
4. 부트 로더에 등록
5. 재부팅

### 6.1 소스 코드 준비

알아서 준비.

### 6.2 빌드 설정 방법

빌드용 설정에서는 이용하는 커널의 기능이나 파라미터, 사용할 하드웨어 드라이버, 모듈 준비 등을 지정한다.

커널 빌드 설정\(menuconfig\) 항목은 다음과 같다.

* General setup
* Enable loadable module support
* Enable the block layer
* Processor type and features
* Power management and ACPI options
* Bus options\(PCI etc.\)
* Executable file formats/Emulations
* Networking support
* Device Drivers
* Firmware Drivers
* File systems
* Kernel hacking
* Security options
* Crytographic API
* Virtualization
* Library routines

커널 소스 디렉터리에서 빌드 설정을 할 경우 .config라는 텍스트 파일이 생성된다. 여기에서는 각 기능이나 모듈 취급에 대해 'y', 'm', 'is not set'등으로 표현되어 있다. 'y'는 커널에 적재, 'm'은 모듈로 빌드, 'is not set'은 앞에 있는 파라미터는 이용하지 않는다는 뜻이다.

### 6.3 빌드 방법

빌드는 다음과 같은 순서로 진행된다.

1. make O=/home/user/build/kernel : 커널, 모듈을 빌드하여 압축된 커널 이미지를 생성
2. make O=/home/user/build/kernel\_modules\_install : 모듈을 시스템 디렉터리에 설치
3. make O=/home/user/build/kernel\_firmware\_install : 일부 드라이버에서 이용하는 펌웨어를 시스템 디렉터리에 설치
4. make O=/home/user/build/kernel install : install kernel 커맨드를 사용해 새로운 커널을 시스템에 설치




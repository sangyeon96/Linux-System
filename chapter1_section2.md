# Section 2. 리눅스의 디렉터리 구성

![](/assets/Linux Filesystem Hierarchy.jpg)

### 2.1 기본 지식

* **FHS\(Filesystem Hierarchy Standard\)** : 리눅스 디렉터리 계층 구조 표준
* **마운트\(mount\)** : HDD 등의 block device 상의 파일 시스템을 시스템 디렉터리 트리의 일부에 설치하는 것
* **파티션\(partition\)** : 어떤 HDD를 여러 개의 영역으로 분할했을 때 생긴 한 영역. 하나의 HDD에는 4개까지 파티션을 만들 수 있다. 이것은 MBR\(Master Boot Record\)이라는 하드디스크의 첫 번째 섹터에 마련된 부트 로더를 저장하는 공간상의 제약 때문에 4개의 파티션까지밖에 기록할 수 없기 때문이다. 또한, 기록할 수 있는 파티션은 '기본\(primary\) 파티션'과 확장\(extended\) 파티션'을 포함해서 4개가 된다.

### 2.2 /\(루트\)

디렉터리 구성의 최상위

### 2.3 /boot

리눅스 커널, init ram fs파일, 부트 로더 GRUB의 설정 파일 등이 들어 있는 디렉터리.

리눅스 커널 본체 **vmlinuz**.

![](/assets/:boot.png)

### 2.4 /etc

### 2.5 /bin

### 2.6 /sbin

### 2.7 /usr

### 2.8 /home

### 2.9 /var

### 2.10 /proc

### 2.11 /sys

### 2.12 /dev

### 2.13 /tmp




# Section 7. Debian GNU/Linux의 커널 구축 방법

### 7.1 빌드 준비와 소스 패키지 구하기

Debian 계열에서는 build-essential이 패키지 빌드에 필요한 패키지, kernel-package가 커널 빌드용 바이너리 패키지를 만드는 데 필요한 패키지명으로 사용되고 있다. 먼저 이들을 빌드하는데 필요한 패키지를 설치한다.

`sudo dpkg --configure -a`, `sudo apt install aptitude로 aptitude` 설치.

`sudo aptitude install build-essential kernel-package`로 빌드하는 데 필요한 패키지 설치.

`aptitude search linux-source`로 커널 소스 패키지 검색.

`sudo aptitude install linux-source`로 설치.

설치 후 /usr/src에 있는 tar.bz2파일에 따라서 `sudo tar xfj linux-source-4.4.0.tar.bz2`로 압축 풀기.

### 7.2 빌드 설정

여기서부터 버젼이 달라서 책으로 따라가니까 잘 안된다..그리고 permission denied..

/usr/src/linux-source-4.4.0에서  `sudo make oldconfig`로 빌드 설정.

`make-kpkg cean`, `fakeroot make-kpkg --initrd --stem extraversion`로 빌드 시작.

/usr/src에서 vanila 아카이브를 내려받아 deb 패키지를 생성. 방법은 위랑 비슷함.

`fakeroot make deb-pkg`로 빌드 시작.

### 7.3 패키지의 빌드

`make deb-pkg`

에러 없이 빌드가 성공하면 화면에 패키지를 구축했다는 메시지가 표시된다. 패키지 파일은 하나 위의 디렉터리에 배치된다.

`make-kpkg`를 이용하면 세세한 옵션을 설정할 수 있다.

### 7.4 패키지 설치

make deb-pkg와 make-kpkg로 만든 패키지는 설치 방법이 같다. 권리자 권한으로 dpkg에 -i 옵션을 붙여 설치할 패키지를 상대 경로나 절대 경로로 지정한다. Debian 패키지로 설치된 커널은 가장 마지막에 부트 로더 갱신을 실시한다. 실행문에서 가장 마지막 10행 정도는 grub.cfg의 자동 생성과 커널을 리스트로 표시하고 있다. 이제 시스템을 재부팅하면 부트 로더 메뉴에서 새로운 커널 선택지가 표시될 것이다.


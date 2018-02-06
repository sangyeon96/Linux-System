# Section 4. X 구동

### 4.1 X 구동 흐름

X 구동에는 일반적으로 xinit, startx, XDM\(X Display Manager\)을 이용한다.

1. X 프로그램\(/usr/bin/X\)이 실행된다. 또는 백그라운드에서 동작하고 있다. X는 설정 파일\(/etc/X11/xorg.conf\)을 찾아서 그래픽 보드 등의 설정을 읽어들이고 초기화한다.
2. 표시 명령어 등의 요구 사항이 들어가는 TCP 포트\(보통 6000\)의 소켓을 **대기 가능 상태**로 오픈한다.
3. 해당 단말기 또는 원격 단말기에서 애플리케이션\(X 클라이언트\)을 실행한다. 모든 X 애플리케이션은 표시할 X 서버의 IP주소를 지시하여 표시할 단말기를 지정한다.
4. 애플리케이션은 네트워크 경유로 X 서버의 **대기 소켓**을 연다. 접속이 안 된다면 X 서버가 실행되어 있지 않거나 또는 애플리케이션을 구동한 원격 단말기가 허가되어 있지 않은 호스트에 속해있는 것이 원인일 수 있다.
5. 애플리케이션은 X 프로토콜을 주고받고 디스플레이에 그 결과를 표시한다.

### 4.2 xinit과 startx

`xinit`은 수동으로 X를 구동하는 명령어이며 `startx`는 `xinit` 실행 후에 환경 변수를 설정하고 프로그램을 실행하는 셸 스크립트이다.

`startx`의 실행\(CentOS\)

1. startx를 실행한다.
2. ~/.xinitrc와 ~/.xserverrc를 탐색한다.
3. /etc/X11/xinit/xinitrc와 etc/X11/xinit/xserverrc를 탐색한다.
4. xinitrc에서 /etc/X11/xinit/xinitrc-common을 실행한다.
5. xinitrc-common에서 /etc/profile.d/lang.sh를 실행하여 언어 관련 환경 변수를 지정한다.
6. xinitrc-common에서 리소스 파일\(~/.Xresources와 /etc/X11/Xresources\)을 읽어들인다.
7. xinitrc-common에서 키보드 설정 파일\(~/.Xmodmap, ~/.Xkbmap과 /etc/X11/Xmodmap, /etc/X11/Xxbmap\)을 읽어들인다.
8. xinitrc-common에서 /etc/X11/xinit/xinit.d 아래의 스크립트를 차례로 실행한다.
9. xinitrc에서 ~/.Xclients 혹은 /etc/X11/xinit/Xclients의 파일 유무를 확인하여 실행한다.
10. Xclients에서 /etc/X11/xinit/Xclients.d 아래의 스크립트를 차례로 실행한다.




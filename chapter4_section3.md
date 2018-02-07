# Section 3. X 설정

X 서버 설정은 이용할 디스플레이나 키보드, 마우스 등의 디바이스를 지정하고 어떻게 움직일 것인가를 지시하는 것이다. X.org의 설정 파일명은 xorg.conf이거나 지정된 디렉터리에 있는 .conf 확장자를 가진 텍스트 파일이 된다. 많은 리눅스 배포판에서는 /etc/X11/xorg.conf와 /usr/share/X11/xorg.conf.d/가 설정 파일 경로로 이용된다.

### 3.1 xorg.conf 생성

X.org의 설정 파일을 자동 생성하는 예

```
Xorg -configure
```

### 3.2 xorg.conf의 내용

#### 기본 포맷

```
Section "섹션명"
    설정항목[설정값]
    ...
EndSection
```

위에 나오는 섹션명과 그 용도의 의미는 다음과 같다.

* Files : X 서버가 이용할 파일을 지정. FontPath, ModulePath, XkbDir 등을 지정한다.
* ServerFlags : X 서버 전체에 적용되는 특별한 설정. 이 섹션은 필수는 아니며 ServerLayout 섹션에서 정의된 내용이 있을 때에는 덮어쓰기 됨.
* Module : 구동 시 읽어 들일 모듈을 지정.
* Extensions : X 프로토콜 확장을 이용하는 경우에 지정.
* InputDevice : 입력 디바이스를 지정. 복수 설정 가능.
* InputClass : InputDevice별 드라이브 읽기, 옵션 지정 등을 기술.
* Device : 비디오 카드 설정. 적어도 하나 이상 기술해야 함.
* Monitor : 모니터 관련 설정.
* Modes : Monitor섹션에 기술되는 비디오 모드에 대해 기술.
* Screen : 표시 설정.
* ServerLayout : X 서버가 이용하는 입력, 출력 디바이스를 지정함. Screen섹션\(출력장치\), InputDevice섹션\(입력장치\), Options에서 ServerFlags의 변수를 덮어씀.
* DRI : Direct Rendering Infrastructure의 변수 지정.
* Vendor : 디바이스를 제공하는 벤더 정보를 기술.

책에 나와있는 xorg.conf 예제를 보면 기본 포맷 형태의 섹션이 총 8개 존재한다.

Module, InputDevice, InputDevice, Device, Monitor, Screen, ServerLayout, DRI

여기서 InputDevice섹션이 2개인 이유는 하나는 키보드, 하나는 마우스 관련 설정이기 때문이다.

ServerLayout섹션을 보면 Screen섹션과 InputDevice섹션과 관련이 있음을 알 수 있다. 반면 Module섹션과 DRI섹션은 독립되어 있다.

### 3.3 xorg.conf.d

기본적인 키보드와 마우스 이외의 입출력 장치는 /usr/share/X11/xorg.conf.d 또는 /etc/X11/xorg.conf.d 아래에서 설정한다. InputClass섹션을 사용하여 제품명이나 경로로 검색된 장치의 세밀한 설정을 할 수 있다. 예시는 책 참고.


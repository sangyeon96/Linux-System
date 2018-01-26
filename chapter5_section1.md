# Section 1. 부팅 스크립트와 설정 파일

리눅스 커널을 부팅할 때에는 루트 파일 시스템상의 `init`을 실행하게 된다.  `init`은 부팅 시 설정 파일을 읽어들이고 이에 따라 자식 프로세스를 생성한다. `init`의 자식 프로세스가 모두 종료되더라도 `init` 자체는 시스템이 종료될 때까지 프로세스로서 남아 있게 된다. 먼저 `init`을 제공하는 패키지를 살펴보자.

### 1.1 sysvinit

init은 커널의 하드웨어 초기화 후 커널이 구동한다. /etc/inittab을 설정 파일로 읽어들이고 파일에 있는 엔트리 ID를 참조하여 런레벨을 결정한다.

```
id:runlevel:action:process
```

엔트리 ID는 4문자까지 지정할 수 있는 엔트리 식별 문자이다. 행 첫머리에 오는 'id', 'si', '10' 등이 이 ID에 해당한다. **runlevel**은 지정할 액션을 실행할 런레벨을 복수 지정할 수 있다. 런레벨을 생략하는 경우 기본 런레벨이 설정된다. action은 동작을 정의한다. action에 정의할 수 있는 항목에 대해서는 뒤에서 설명하겠다고한다. process는 실행하는 명령어를 지정한다.

![](/assets/:etc:inittab.png)

이걸 통해 CentOS에서는 systemd를 부팅 도구 패키지로 이용함을 알 수 있다.

### 1.2 upstart

upstart는 sysvinit을 대체하기 위해 Ubuntu의 프로젝트가 중심이 되어 개발한 패키지이다.

sysvinit은 런레벨 별로 init 스크립트를 나누어 프로세스 구동이나 정지를 관리한다. 반면 upstart는 커널이나 동작 중인 프로세스에서 받는 실행, 중지 이벤트를 통해 다른 프로세스의 서비스 실행이나 중지, 또는 예기치 못한 프로세스의 중지가 발생했을 시 재부팅을 지원하는 등 시스템이 제공하는 서비스와 데몬의 구동을 담당하는 sysvinit보다 한층 더 발전된 구동 시스템이라고 할 수 있다.

upstart는 sysvinit과의 호환성을 유지하기 위해 런레벨이라는 개념을 그대로 가져오고있다. 단, upstart는 기본적으로 'event'와 'job' 개념을 바탕으로 동작한다. 구동시 /etc/init/아래의 .conf 확장자를 가진 파일을 'job'으로 사용하게 된다.

#### job의 기술 방법

job 파일은 /etc/init/ 아래에 'job 이름.conf'로 생성한다. 또한, job 파일 내에서 지정하는 각 항목은 **스탠자\(stanza\)**라고 부른다. start on, stop on 스탠자는 이벤트를 받는 쪽이지만, job은 emit 스탠자를 사용하여 이벤트를 발생시킬 수도 있다. job 파일끼리 서로 이벤트를 주고받음으로써 실행, 중지 처리를 수행한다.

upstart는 task와 service라는 두 가지 job 타입으로 나뉜다. task는 짧은 프로세스로 처리가 종료되면 job도 종료된다. service는 Apache와 같은 데몬 job으로, 'task'가 명시적으로 쓰여 있지 않은 job은 'service'로 취급된다.

또한, job 파일은 bash 베이스의 스크립트를 처리할 수 있다. 프로세스 실행, 종료 처리 관련 스탠자의 종류는 다음과 같다.

* pre-start
* post-start
* pre-stop
* post-stop

그 외 자주 사용하는 스탠자는 다음과 같다.

* env
* exec
* respawn
* umask
* kill timeout
* expect
* normal exit
* console

#### initctl 사용

upstart는 job 관리에 initctl을 사용한다. 이 커맨드로 이용할 수 있는 주요 기능은 다음과 같다.

* start
* stop
* restart
* reload
* status
* list
* emit
* reload-configuration
* log-priority
* show-config
* check-config

#### upstart의 동작\(CentOS\)

upstart의 init은 커널에 의해 실행된 후 startup 이벤트를 발생시킨다. 각 job 파일 중 start on startup 이라는 기술이 들어가 있는 job이 이때 실행된다. CentOS에서는 가장 첫 이벤트로 etc/init/rcS.conf job을 실행한다.

재실행\(respawn\)하는 job은 해당 프로세스를 kill하여 정상적으로 프로세스가 다시 부활하는지 확인해볼 수 있다.

#### 사용자가 직접 job 파일을 작성

여기에서는 swap을 마운트하는 간단한 job을 예로 들어보겠다.

```
# mount-swap - Mount swap
#
description "Mount swap"

start on all-swap
stop on starting rcS

task

# temporary, until we have progress indication
# and output capture (next week :p)
console output

script
    swapon /dev/sda5
end script
```

다음으로 `sudo initctl emit all-swap`을 실행하면 swap이 마운트될 것이다. job이 문제없이 반응하면 swap 영역이 마운트 되고 로그에 기록이 남는다.

### 1.3 systemd

systemd는 sysvinit을 대체하기 위한 init 데몬이다. 특징으로는 처리 병렬화, UNIX 소켓과 D-Bus\(애플리케이션의 프로세스 간 통신\)를 이용한 서비스 실행, Linux 프로세스 컨트롤 그룹\(cgroups\) 기능을 이용한 프로세스 추적, 시스템 상태 스냅샷 생성/복원 등을 들 수 있다.

systemd에서는 **유닛**과 **타깃**이 기본 개념이다. 지금까지의 runlevel은 '타깃'으로 바꾸어 생각하면 된다. **타깃은 유닛의 집합**이다. 유닛은 systemd가 다루는 조작 대상을 가리킨다.

#### systemd의 구성

![](/assets/systemd.jpg)

sysvinit에서는 init이 구동되고 /etc/inittab으로부터 런레벨을 읽어와 해당 런레벨의 디렉터리\(/etc/rc5.d\) 아래의 데몬 구동 스크립트를 실행한다. 이 안에는 syslog도 포함되어 있어서 시스템의 로그 등이 syslogd 경유로 로그에 기록되거나 httpd와 같이 독자적으로 로그를 기록한다. 데몬의 구동, 중지에는 service 등을 사용한다.

systemd는 **journald**라는 독자적인 로깅 시스템을 가지고 있으며 systemd와 동시에 구동된다. journald만으로도 로그는 수집되지만, syslogd가 존재하는 환경과의 호환성을 고려하여 rsyslogd와 소켓을 경유하여 기존의 시스템 로그가 기록되어 있다. sshd 등은 journald에 기록하며, journald가 소켓을 경유하여 rsyslog에서 로그를 남긴다. httpd 등은 sysvinit과 마찬가지로 독자적으로 로그를 기록한다.

#### systemd의 유닛

* systemd.service
* systemd.socket
* systemd.device
* systemd.mount
* systemd.automount
* systemd.swap
* systemd.target
* systemd.path
* systemd.timer
* systemd.snapshot

#### systemd 이용 커맨드

* /usr/bin/hostnamectl
* /usr/bin/journalctl
* /usr/bin/localctl
* /usr/bin/loginctl
* /usr/bin/systemctl
* /usr/bin/systemd
* /usr/bin/timedatectl
* /usr/bin/udevadm
* /usr/sbin/halt
* /usr/sbin/init
* /usr/sbin/poweroff
* /usr/sbin/reboot
* /usr/sbin/runlevel
* /usr/sbin/shutdown
* /usr/sbin/telinit
* /usr/sbin/udevadm

여기서 ctl은 control이라고 보면 된다.

#### systemd의 설정 파일

systemd는 /etc/inittab을 읽어들이지 않고 /etc/systemd/system/default.target을 먼저 읽어 온다.

##### multi-user.target

![](/assets/multi-user.target.png)

##### graphical.target

![](/assets/graphical.target.png)

#### 서비스의 등록/실행/중지/재실행

##### 서비스 실행

```
sudo systemctl start httpd
```

##### 서비스 상세 정보 확인

```
systemctl status httpd
```

##### 서비스 중지

```
sudo systemctl stop httpd
```

##### 서비스 자동 시작

```
sudo systemctl enable httpd
```

systemd는 리눅스의 고유 기능을 이용하고 있으며 타 시스템에 대한 이식성이 낮은 init시스템이지만, 리눅스의 거인이라고 할 수 있는 Red Hat사의 Fedora에서 채택하고 있는 만큼 향후 존재감은 더 커질 가능성이 높다고 한다.

### 1.4 파티션과 마운트 포인트 설정\(/etc/fstab\)



### 1.5 커맨드라인 입력 지원 라이브러리\(/etc/inputrc\)




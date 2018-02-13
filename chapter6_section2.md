# Section 2. 네트워크 파일 시스템

### 2.1 NFS

NFS\(Network File System\)는 서버 자신이 가진 스토리지의 파일 시스템을 네트워크 경유로 원격 호스트에 제공하기 위한 프로토콜이다. 파일 시스템을 제공하는 **NFS 서버**와 이 파일 시스템을 이용하는 **NFS 클라이언트**로 구성되어 있다.

NFS 서버는 원격 호스트에 제공하는 스토리지와 접근 제어에 사용하는 설정 파일의 조정이 필요하다. 그리고 NFS 클라이언트는 수동으로 마운트할 때에는 mount, 구동 시 자동으로 마운트하는 경우에는 /etc/fstab 설정이 필요하다.

NFS 서버는 Debian의 경우 nfs-server 패키지, Red Hat 계열은 nfs-utils 패키지를 설치하여 서버 환경을 구축한다.

/var/nfs 디렉터리를 NFS로 서비스하는 스토리지 영역으로 설정하고 /etc/exports를 편집하여 클라이언트에서 마운트해보자.



### 2.2 Samba

Samba 웹사이트에는 "Samba는 Windows와의 상호 운용을 위한 Linux/UNIX용 프로그램의 집합이다."라는 설명이 나와있다. Samba는 **SMB/CIFS** 프로토콜을 사용하여 Windows와 Linux 갑의 파일 공유, 프린터 공유, 인증, 이름 분석 등의 기능을 제공한다.

**SMB\(Server Message Block\)**은 LAN 상에서 파일 공유나 프린터 공유를 구현하는 프로토콜이다.

**CIFS\(Common Internet File System\)**는 SMB의 사용을 공개, 확장한 프로토콜이다.

SMB는 Microsoft의 독자 프로토콜이며 CIFS는 SMB를 오픈화한 프로토콜이다.

이 프로토콜을 사용함으로써 Windows 단말기가 서비스하는 파일 공유나 프린터 공유를 리눅스 단말기가 클라이언트로서 이를 이용할 수 있게 되고, 반대로 리눅스 단말기에서 구축된 Samba 서버에서의 파일 공유와 프린터 공유 서비스를 Windows 단말기가 이용할 수 있게 된다.

#### smb.conf의 내용

#### smbclient를 통한 접속

#### 파일 매니저 노틸러스\(Nautilus\)

### 2.3 WebDAV

WebDAV\(Web-based Distributed Authoring and Versioning\)는 여러 명이 원격 웹 서버의 파일 편집이나 관리를 수행할 수 있도록 확장한 HTTP 프로토콜이다.

리눅스에서 이용하는 웹 서버 Apache에 mod\_dav 모듈이 표준 탑재되어 있으므로 이를 설정하면 파일을 공유할 수 있다.

#### httpd-dav.conf의 내용

#### WebDAV의 설정 예

#### WebDAV에 접근




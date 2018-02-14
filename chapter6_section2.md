# Section 2. 네트워크 파일 시스템

### 2.1 NFS

NFS\(Network File System\)는 서버 자신이 가진 스토리지의 파일 시스템을 네트워크 경유로 원격 호스트에 제공하기 위한 프로토콜이다. 파일 시스템을 제공하는 **NFS 서버**와 이 파일 시스템을 이용하는 **NFS 클라이언트**로 구성되어 있다.

NFS 서버는 원격 호스트에 제공하는 스토리지와 접근 제어에 사용하는 설정 파일의 조정이 필요하다. 그리고 NFS 클라이언트는 수동으로 마운트할 때에는 mount, 구동 시 자동으로 마운트하는 경우에는 /etc/fstab 설정이 필요하다.

NFS 서버는 Debian의 경우 nfs-server 패키지, Red Hat 계열은 nfs-utils 패키지를 설치하여 서버 환경을 구축한다.

/var/nfs 디렉터리를 NFS로 서비스하는 스토리지 영역으로 설정하고 /etc/exports를 편집하여 클라이언트에서 마운트해보자.

1. /etc/exports 파일을 만든 뒤 NFS 서버를 구동한다.
2. 다음으로 클라이언트에서 공유 디렉터리를 마운트한다.
3. 마운트되면 NFS 서버에 NFS 클라이언트가 표시된다.

옵션에는 일반 옵션과 사용자 맵 옵션이 있다. 기본은 ro\(읽기 전용 파일 시스템\), wdelay\(NFS 서버에 대한 쓰기를 지연시킴으로써 퍼포먼스가 향상되는 것처럼 보임\), root\_squash\(root사용자로 접속했을 때 NFS 서버에서 해당 사용자를 일반 사용자로 취급함으로써 악성 파일이 갱신되는 것을 방지함\)가 설정되어 있다. 그 외 설정할 수 있는 주요 옵션은 p.321을 참고하기. /etc/exports를 변경한 뒤에는 NFS 데몬을 재시작하거나 exportfs로 다시 읽어들이면 적용된다.

### 2.2 Samba

Samba 웹사이트에는 "Samba는 Windows와의 상호 운용을 위한 Linux/UNIX용 프로그램의 집합이다."라는 설명이 나와있다. Samba는 **SMB/CIFS** 프로토콜을 사용하여 Windows와 Linux 갑의 파일 공유, 프린터 공유, 인증, 이름 분석 등의 기능을 제공한다.

**SMB\(Server Message Block\)**은 LAN 상에서 파일 공유나 프린터 공유를 구현하는 프로토콜이다.

**CIFS\(Common Internet File System\)**는 SMB의 사용을 공개, 확장한 프로토콜이다.

SMB는 Microsoft의 독자 프로토콜이며 CIFS는 SMB를 오픈화한 프로토콜이다.

이 프로토콜을 사용함으로써 Windows 단말기가 서비스하는 파일 공유나 프린터 공유를 리눅스 단말기가 클라이언트로서 이를 이용할 수 있게 되고, 반대로 리눅스 단말기에서 구축된 Samba 서버에서의 파일 공유와 프린터 공유 서비스를 Windows 단말기가 이용할 수 있게 된다.

#### smb.conf의 내용

설정 파일에서는 섹션과 섹션에 속하는 파라미터 그리고 해당 파라미터의 값을 `=`로 대입하여 설정한다. 섹션에는 \[global\], \[homes\], \[printers\] 등이 있으며 다음 섹션의 정의까지가 그 섹션에 속하는 파라미터가 된다.

#### smbclient를 통한 접속

여기서는 smbclient와 mount.cifs를 사용하여 Samba를 이용하는 예를 소개한다.

`smbclient`는 `//Samba 서버명/공유 디렉터리명`을 옵션으로 주어 Samba 서버에 접속한다.

```
$ smbclient //endeaver/mp3
```

#### 파일 매니저 노틸러스\(Nautilus\)

파일 매니저 노틸러스를 사용하여 Samba 파일 공유에 접근할 수 있다.

1. 메뉴의 파일-서버에접속 선택
2. 서버에접속-종류-Windows 공유 선택
3. 서버명, 공유할 위치, 도메인명 등을 설정한 후 연결

### 2.3 WebDAV

WebDAV\(Web-based Distributed Authoring and Versioning\)는 여러 명이 원격 웹 서버의 파일 편집이나 관리를 수행할 수 있도록 확장한 HTTP 프로토콜이다.

리눅스에서 이용하는 웹 서버 Apache에 mod\_dav 모듈이 표준 탑재되어 있으므로 이를 설정하면 파일을 공유할 수 있다.

Debian GNU/Linux의 WebDAV는 다음 순서대로 유효화한다.

1. /etc/apache2/mods-available/dav\_fs.conf를 편집
2. a2enmod dav\_fs를 관리자 권한으로 실행
3. service apache2 restart를 관리자 권한으로 실행

#### httpd-dav.conf의 내용

책 참고.

#### WebDAV의 설정 예

1. /home/dav를 WebDAV 디렉터리로 설정
2. /dav/project와 /dav/manager 디렉터리를 만들어 각 사용자별로 접근을 제어
3. /dav/user 아래는 LDAP 서버로 인증

Digest 인증 파일은 `htdigest`로 생성한다. 또한 그룹 파일은 '그룹명:사용자명...'과 같은 형식으로 작성하면 복수 사용자를 그룹으로 취급하여 접근을 제어할 수 있다.

#### WebDAV에 접근

Nautilus에서 접근하려면 위에도 언급했다시피 \[파일\]메뉴에서 \[서버에 접속\]을 선택하고 WebDAV 서버의 호스트 이름이나 IP주소, 디렉터리명과 사용자명, 비밀번호를 입력한다. 인증을 통과하면 새 창에서 WebDAV 디렉터리의 리스트가 표시된다. 또한, 커맨드 라인으로 접근할 수 있게 해주는 `cadaver`이라는 도구도 있다.

WebDAV에서는 디렉터리를 가리켜 'Collection'이라고 부른다. 그래서 해당 내용에 있는 Coll:은 디렉터리를 의미한다.

기타 cadaver에서 이용할 수 있는 주요 커맨드는 책 참고하기. dir 대신 col이 들어간거\(ex. mkdir -&gt; mkcol, rmdir -&gt; rmcol\) 빼고는 리눅스 기본 커맨드랑 비슷하다.


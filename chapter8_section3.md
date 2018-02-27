# Section 3. LDAP 인증

### 3.1 LDAP 인증

LDAP\(Lightweight Directory Access Protocol\)는 디렉터리 서비스를 제공하는 프로토콜이다. 디렉터리 서비스라는 것은 검색 문구를 바탕으로 데렉터리 데이터베이스를 검색, 참조하여 데이터를 추출하는 서비스이다.

### 3.2 OpenLDAP의 설정

LDAP를 오픈소스로 구현한 것이 **OpenLDAP**이다.

OpenLDAP의 실행에 필요한 것은 크게 두 가지로 나눌 수 있다.

1. schema라고 하는 객체의 구성과 속성을 결정하는 데이터 구조이다.
2. slapd.conf 설정 파일이다. OpenLDAP 2.3 이후부터는 **cn=config**라는 전용 DIT에 설정 항목을 저장함으로써 데몬 재구동 없이도 설정 변경을 적용하는 **OLC**\(On-Line Configuration\)을 채택하고 있다. OpenLDAP 2.3에서는 slapd.conf 운용도 가능하지만 OLC 이용을 권장하고 있다.

**LDIF**\(LDAP data interchange format\)라는 디렉터리 정보를 추가, 갱신, 삭제하기 위한 텍스트 파일이 있다.

![](/assets/LDAP DIT.png)출처 : [https://ensiwiki.ensimag.fr/index.php?title=About\_LDAP](https://ensiwiki.ensimag.fr/index.php?title=About_LDAP)

LDAP DIT 구조를 보면, dc, ou, cn등이 있는데 그건 다음과 같다.

dc \(Domain Component\)

ou \(Organizational Unit\)

cn \(Common Name\)


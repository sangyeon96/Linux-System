# Section 4. Postfix \(SMTP 서버\)

### 4.1 메일 시스템의 개요

M**U**A\(Mail **User** Agent\) : 메일 클라이언트. ex\) Outlook, Becky!, Sylpheed, Thunderbird

M**T**A\(Mail **Transfer** Agent\) : 메일 서버, SMTP 서버라고도 불린다.

M**D**A\(Mail **Deliver** Agent\)

사용자 A가 사용자 B에게 메일을 보내고 이를 사용자 B가 수신하는 과정은 다음과 같다.

유저 A\(MUA\) -&gt; MTA -&gt; MDA -&gt; Mailbox -&gt; 유저 B\(MUA\)

**목적 MTA에 도착하려면 DNS가 반드시 필요하다.** MUA에서 메일 전송을 의뢰받은 MTA는 우선 DNS에 목적 도메인의 메일 서버를 묻는다. DNS는 MX 레코드라는 리소스를 MTA에 반환하고 MTA는 MX 레코드에 지시된 MTA로 메일 전송을 위한 통신을 시행한다. 목적 도메인 안에 메일이 전송되고 나서도 사용자의 메일함에 도달하기까지 다시 MTA를 통과해야 하는 경우가 있을지도 모른다.

### 4.2 SMTP 서버 Postfix

#### Postfix란?

Postfix는 과거 대표적 오픈소스 MTA였던 sendmail과의 이용 호환성을 유지하면서도 간편한 설정, 관리와 대용량 메일의 고속 전송을 구현한 MTA이다. postfix는 sendmail에 비해 메일 전송 시에 여러 개의 데몬이 협조하여 동작하기 때문에 동작을 파악하기 어려울 것으로 생각하기 쉬우나 설정 파일이 보기 슆고 관리하기도 편하다는 장점을 갖고 있다.

#### Postfix와 협조 동작하는 데몬

p.361 참조. Debian GNU/Linux의 Postfix 패키지 파일에서 협조 동작하는 데몬을 발췌해 정리한 내용이다.

#### 설정 파일 내용

Postfix의 설정 파일은 /etc/postfix에서 볼 수 있다. 책에서는 main.cf의 내용을 다루고 있다. 다음은 주석 분량을 제외한 내용이다.

![](/assets/:etc:postfix:main.cf%281%29.png)

![](/assets/:etc:postfix:main.cf%282%29.png)

#### telnet을 사용하여 SMTP 서버와 대화




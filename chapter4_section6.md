# Section 6. X 서버 로그

### 6.1 X 서버 로그를 보는 법

X.org의 로그는 /var/log/Xorg.&lt;X세션번호&gt;.log에 기록되어 있다. 복수의 X 세션이 실행되는 경우에는 각 세션의 번호가 로그 파일명에 들어가는데, 로컬에서 X 서버와 X 클라이언트를 모두 실행할 때에는 보통 /var/log/Xorg.0.log에 기록된다.

#### X.org 로그 기호와 그 의미

| 기호 | 영문 표기 | 의미 |
| :--- | :--- | :--- |
| \(--\) | probed | 하드웨어 인식 |
| \(\*\*\) | from config file | 설정 파일 읽기 |
| \(==\) | default setting | 기본 설정 읽기 |
| \(++\) | from command file | 커맨드 라인 읽기 |
| \(!!\) | notice | 주의 |
| \(\|\|\) | informational | 정보 |
| \(WW\) | warning | 경고 |
| \(EE\) | error | 오류 |
| \(NI\) | not implemented | 구현되어 있지 않음 |
| \(??\) | unknown | 확인되지 않음 |




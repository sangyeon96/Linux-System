# Section 2. X 액세스 제어

X는 클라이언트/서버 아키텍처를 채택하고 있어 원격 호스트에서 구동하는 X 애플리케이션을 로컬 X 서버에 표시할 수 있다고 앞서 설명했다. **X 서버 자체의 설정과 패킷 필터링을 통해 액세스를 제어할 수 있다**.

### 2.1 xhost

X는 기본적으로는 X 서버와 같은 단말기에서 동작하는 X 애플리케이션 표시만을 허가한다. 다른 단말기에서의 접속을 허가하려면 `xhost`를 이용한다. 다음 예시는 192.168.11.12의 X 애플리케이션을 자신이 이용하는 X 서버에 표시한다.

```
xhost +192.168.11.12
```

`xeyes`로 제대로 실행이 되는지 확인할 수 있다.

X 서버를 지정하는 형식은 다음과 같다.

```
-display X 서버명: 디스플레이 번호.스크린 번호
```

'X 서버명'은 IP주소나 이름 분석\(name resolution\)이 가능한 호스트 이름이다. '디스플레이 번호'는 X 세션의 번호로, `-display`에서는 필수 지정 항목이다. '스크린 번호'는 X 서버가 이용하는 디스플레이 확인을 위한 것이다. 이를 생략할 경우 기본값은 0이다.

`xhost`를 사용해 접속을 허가할 X 클라이언트 실행 호스트만을 추가하는 것이라면 +는 생략하고 IP주소만 지정해도 된다. 단, +만 적고 대상 IP주소를 지정하지 않는 경우 TCP/IP로 접속 가능한 모든 호스트가 X 서버에 액세스할 수 있게 되므로 주의해야한다.

그리고 사용자명으로도 지정할 수 있다. 다음은 localhost의 사용자 rino의 접속을 허가하는 예제이다.

```
xhost +si:localuser:rino
```

현재 액세스 제어 상태를 확인하고 싶을 때에는 옵션 없이 `xhost`을 실행한다.

### 2.2 xauth

`xhost`는 네트워크상의 호스트 이름 등을 이용해 액세스를 제어하지만, 더욱 엄격히 액세스를 제어하고 싶을 때에는 `xauth`를 사용한다. `xauth`에서는 매직 쿠키라고 하는 인증용 문자열을 사용한다. 이것은 X 서버의 쿠키를 X 애플리케이션을 실행하는 단말기에 설정함으로써 액세스를 제어하는 키\(key\)역할을 하게 된다. `xauth list`로 X 서버의 매직 쿠키를 표시할 수 있다.

X서버의 쿠키는 클라이언트 실행 단말기로 전송한다. `xauth extract`를 사용해 xauth 지시 파일을 만들고 파이프 `|`를 사용해 `ssh`로 대상 호스트, `xauth merge`의 실행을 지시하면 대상 호스트에 등록할 수 있다. 참고로 ssh는 secure shell을 의미한다. 다음은 xauth 매직 쿠키 전송 예시이다.

```
$ xauth extract - $DISPLAY | ssh -l 사용자명 실행단말기 xauth merge -
```




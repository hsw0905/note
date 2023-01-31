# HTTP Cookie 와 HTTP Session

## 왜 등장하게 되었는가?
- HTTP 요청은 stateless (요청 -> 응답 후 연결이 끊어진다.)
- 이전 요청을 기억하지 않는다.
- 클라이언트는 자기가 누구인지 계속 알려줘야 한다.
- 그렇다고 매 요청마다 사용자 정보를 직접 넘기기엔 너무 복잡하다.
- 대안 : HTTP 통신시 Header에 쿠키를 주고 받자 !!

## 주로 사용되는 용도
- 사용자의 로그인 세션 관리
- 광고 정보 추적

## 관련 Header
- Set-Cookie : 서버 -> 클라이언트로 쿠키를 전달한다.
- Cookie : 클라이언트 -> 서버로부터 받은 쿠키를 저장하며, HTTP 요청시 서버로 다시 전달한다.


## 어떻게 주고 받는가?
- 클라이언트(브라우저)에서 사용자가 로그인(POST)을 한다.
- 사용자에 대한 세션을 쿠키로 관리하는 경우
- 서버는 이에 대한 응답으로 Header에 Set-Cookie: sessionId=xxx...(Key:Value 형태) 쿠키를 전달한다.
- 브라우저에는 쿠키 저장소가 있어 받은 쿠키를 저장한다.
- 이후 브라우저는 요청 할때마다 쿠키 저장소를 뒤져서 쿠키 정보를 Header에 Cookie: sessionId=xxx... 값으로 전달한다.
- 서버는 요청시 Header에 담겨 온 쿠키값을 통해 유저가 누구인지 알 수 있다.

## 브라우저에서 쿠키 정보를 항상 서버로 보낸다면 문제가 있지 않을까?
- 네트워크 트래픽이 추가적으로 유발된다.
- 그러므로 최소한의 정보만 사용해야 한다. (세션 id, access token 등)
- 개인정보에 민감한 데이터는 주고 받으면 안된다.

## 쿠키를 서버에 전송하지 않고 사용할 수도 있다
- 브라우저 내부에 데이터를 저장하고 클라이언트에서 사용할 수 있다.(local storage, session storage)

## 쿠키는 언제까지 존재하는가?
- expire: 만료 날짜
	- 만료일이 지나면 쿠키는 삭제된다.
- 세션 쿠키 : 만료 날짜 생략시 브라우저가 종료될 때까지만 유지된다.
- 영속 쿠키 : 만료 날짜를 입력하면 해당 날짜까지 유지된다.

## 기타 쿠키 설정
- domain=xxx.com
	- 도메인을 지정하면 하위 도메인까지 접근 가능
- path=/
	- 이 경로를 포함한 하위 페이지만 쿠키 접근 가능하다.
- Secure
	- 적용시 HTTPS인 경우에만 전송된다.
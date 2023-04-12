# Spring Security

## Authentication(인증)
- Application이 이를 이용하려는 사람을 식별하는 프로세스
	-	 Server : 당신은 누구입니까?
	- Client : 나는 XXX 입니다!

## Authorization(인가)
- 인증된 유저가 특정 기능과 데이터에 대한 이용 권리가 있는지 확인하는 프로세스
	- Client : (허락되지 않은) 리소스에 접근
	- Server : 당신은 사용할 권한이 없습니다 !


### HTTP는 Stateless 하다
- HTTP는 Stateless 하다
	- HTTP Request마다 독립적이다.
	- Client는 항상 자기가 누구인지 Server에게 인증 받아야 한다.
	- Server는 내부적으로 인증과 인가 모두 검증한다.

## Cookie vs LocalStorage
- Cookie
	- 웹사이트에 의해 사용자 컴퓨터에 저장되는 작은 텍스트 파일(최대 4KB)
	- 용도
		- 사용자가 방문한 페이지 저장
		- 사용자의 로그인 정보 저장 등
		- 문자열만 저장 가능
	- 유형
		- Persistent Cookies
			- 만료일을 포함하지 않는다.
			- 브라우저가 닫히면 영구적으로 해당 쿠키는 삭제된다.
		- Session Cookies
			- 만료일을 가진다.
			- 만료일까지 유저의 디스크에 저장되고 만료일이 지나면 삭제된다.
			- 특정 웹사이트에서의 행동을 기록

- LocalStorage
	- HTML5가 등장하며 새로운 데이터 저장 옵션으로 도입
	- 새로운 JavaScript 객체 (5MB 이상)
		- SessionStorage
			- 현재 페이지가 브라우징되고 있는 브라우저 컨텍스트 내에서만 데이터가 유지된다.
			- 브라우저가 종료되면 데이터도 지워진다.
			- 브라우저를 하나 더 실행해서 같은 페이지를 보고 있다 하더라도 브라우저 컨텍스트가 다르므로 두 SessionStorage는 별개 영역이다.
		- LocalStorage
			- 브라우저를 종료해도 데이터는 보관된다.
			- 다음번 접속시 저장되어 있던 데이터를 사용 가능하다.
			- 도메인만 같다면 전역적으로 공유 가능하다.
	- 사이트의 도메인 단위로 접근이 제한된다.
		- A 도메인에서 데이터 저장 -> B 도메인에서 조회 불가
	- JavaScript 코드를 통해 삭제되지 않는한 삭제되지 않는다.
	- 문자열 외에 JavaScript 객체, 원시타입 자료형도 저장할 수 있다.
	- 장점
		- Cookie와 다르게 모든 HTTP 요청에서 데이터를 주고 받을 필요가 없다.
			- 서버간 트래픽, 낭비되는 대역폭의 양을 줄일 수 있다.
			- 인터넷이 끊어져도 데이터가 삭제되지 않는다.
	- 주의
		- 저장된 데이터는 해킹에 대비해 암호화 하는편이 좋다.
		- 인터넷 연결이 끊어졌다가 다시 연결이 되었을 때는 로컬에 있던 버전을 삭제하는 것이 좋다.

## Interceptor vs Filter (vs AOP)
![출처 : https://velog.io/@_koiil/Filter-Interceptor-AOP](/spring/security/images/filter.png)
- [Dispatcher Servlet?](/spring/webmvc/dispatcher.md)

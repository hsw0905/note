# Dispatcher Servlet
![출처 : https://mangkyu.tistory.com/18](/spring/webmvc/images/dispatcher.png)
- Request -> Servlet Container
	- 필터들을 지나
		- Spring Container로 오면
			- Dispatcher Servlet이 가장 먼저 요청을 받는다.
- 역할 : 요청을 처리할 적절한 컨트롤러를 찾고, Request를 위임한다. -> 위임한 결과를 받아온다.

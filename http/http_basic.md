# HTTP의 이해

## HTTP (HyperText Transfer Protocol)
- 애플리케이션 사이에 리소스(HTML 문서 등)를 주고 받기 위한 약속(규약)
- L7(Layer7) 계층
	- L5 계층 이상의 통신은 Socket을 사용하며, Stream 데이터이다.
	- 즉, 데이터가 Stream 형태로 오며 시작은 있는데(Header) 끝이 어디인지 해석을 해봐야 한다.
	- 이 해석하는 규정이 HTTP 약속이다.
- 기본 원리
	- Client: 요청 <-> Server : 응답


## Web의 영역
![출처 : MDN](/http/images/web_range.png)


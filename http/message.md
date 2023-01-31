# HTTP Message 구조

## Request, Response type
- Request: 클라이언트 -> 서버로 요청시 일어나는 메시지
- Response: 요청에 대한 서버의 응답
- ASCII 문자열

## 메시지 구조
![출처 : https://developer.mozilla.org/ko/docs/Web/HTTP/Messages](/http/images/http_message.png)
- Start Line
	-	 Request / Response 별 형태가 다르다
- Header
	- 다양한 헤더...
	- 표현 헤더 (Representation Header)
		- 표현 데이터를 해석할 정보를 제공(유형 : html, json..., 길이, 압축 정보 등)
		- Content-xxx...
		- (html 로 전송하겠다 / json으로 전송하겠다)
- 빈줄
- 표현 데이터 : message body (==payload)



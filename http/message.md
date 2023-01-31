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


## MIME(Multipurpose Internet Mail Extension) type
- 서버가 응답을 보낼때 컨텐츠 타입이 무엇인지 알려주는 것이 중요하다.
	- Web상에서 파일의 확장자는 별 의미가 없기 때문이다.
- 브라우저 대부분은 리소스를 받을 때 MIME 타입을 사용한다.
- 형태 : type/subtype (예시 : text/html, application/json...)

### 멀티파트 타입
- multipart/form-data ... 등의 형태
- 이미지 등의 파일을 주고 받을 때
- 파일을 어떻게 주고 받을 것인가를 생각해보자.
- HTTP request Body에 글과 이미지 파일이 들어간다면
	- Content-Type은 ?
		- application/x-www-form-urlencoded ? image/png?
		- 서로 다른 종류의 데이터를 구분해서 넣어줄 방법이 필요하다.
		- 이때 multipart 타입이 등장한다.

### enctype
- form 요소를 보면 enctype이 있는데 여기에 인코딩 정보가 담긴다.
- 예시
	- application/x-www-form-urlencoded
		-	default, 모든 문자들은 서버로 보내기 전 인코딩된다.
	- text/plain
		-	공백문자->"+"로 변환되며, 나머지 문자는 인코딩 되지 않는다.
	- multipart/form-data
		- 모든 문자를 인코딩하지 않는다.
		- 주로 파일이나 이미지를 서버로 전송할때 사용한다.

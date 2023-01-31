# HTTP vs HTTPS (TLS)

## TLS(Transport Layer Security)
- 예전엔 SSL(Secure Socket Layer)이라 불렀다.
- L5(Layer5) 계층의 암호화 프로토콜
- 하위 L4(Layer4) - Transport 계층을 암호화한다.
- TLS로 암호화된 HTTP -> HTTPS이다. (주로 443 port)

## SSL/TLS 위치 참고 이미지
![출처 : https://www.lesstif.com/ws/ssl-tls-https-43843962.html](/http/images/tls.png)

## 왜 굳이 암호화를 하는가?
- 클라이언트와 서버간에 서로 신뢰할 수 있는지 확인할 수 있다.
	- 전자 서명이 포함된 인증서를 사용한다.
- 클라이언트와 서버간의 통신에 사용할 암호를 교환한다.
	- 제3자가 도청할 수 없는 암호화된 통신을 할 수 있다.

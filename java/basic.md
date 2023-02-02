# Java text blocks
- Java 15부터 가능
- Python 텍스트 사용과 동일
- ```java
	String str = """
	이 안에 문장을 남긴다.
	줄바꿈도 해준다.
	""";
	```

# InputStream, OutputStream
- 스트림의 형태를 바이트단위로 읽기 혹은 쓰기 할 때 사용된다.
- 스트림 : 데이터가 들어오고 나가는 통로 역할

# try-with-resources
- 스트림 형태의 데이터 사용시 열고 닫는 과정을 try-with-resources 형태의 문법으로 간결하게 사용 가능하다.
- try-with-resources 형태로 사용하면서 여전히 catch도 사용 가능하다.

# ServerSocket
- 서버 역할을 하는 소켓의 구현체
- port <- 0 입력시 포트번호 자동 할당
- binding 후 클라이언트를 기다리고 있으므로 보통 listener라 한다.

# Blocking vs NonBlocking
- IO에 관련된 리소스 접근에 대한 이야기
- 어떤 값이 되었던간에, 값이 return 될 때까지 기다리면 Blocking, 아니면 NonBlocking
- 코드만 보고 구분하긴 어렵다.
- NIO를 사용한다면 NonBlocking이겠구나 예상만 가능

# Java HttpServer
- Java에서 제공해주는 고수준 Http Server 구현체
- HTTPS도 지원
- HTTP와 Message 형태에서 보았던 Header, Request, Statuscode 등등 컴포넌트를 지원한다.

# NIO

## 왜 등장하게 되었는가?
- 기존 IO 패키지를 개선하기 위해 나왔다.
	- 커널 버퍼 -> JVM의 버퍼로 데이터가 전달되는 과정 중 스레드 블록 발생
	- -> 커널 시스템 메모리를 직접 사용하도록 변경

## IO에 비해 무엇이 달라졌는가?
- IO는 스트림 형태로 입출력을 사용한다.
- NIO는 버퍼를 사용하여 채널 형태로 사용한다.
- 하나의 스레드로 여러 채널을 관리하고 처리한다.
	-	적은 스레드를 바쁘게 사용!!

# 람다식
- Java 인터페이스에 있는 함수의 구현부만 정의한다.
- 익명 함수와 매우 비슷하다.

## 함수형 인터페이스
- 추상 메소드가 단 한개만 존재하는 인터페이스

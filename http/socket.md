# Socket과 Socket API

## Socket
![출처 : https://12bme.tistory.com/297](/http/images/socket.png)
- 네트워크 상에서 돌아가는 서로 다른 두 프로세스 간 양방향 통신의 엔드포인드
- 프로토콜 + IP주소 + Port번호
	- 같은 호스트 내에서는 서로 다른 프로세스가 같은 Port번호를 가질 수 없다.
- 프로세스가 데이터를 주고 받기 위해 소켓을 사용 (열고 데이터를 쓰거나, 읽는다)
	- 파일 다루는 것과 비슷하다.
- 멀리 떨어져 있는 두 호스트를 연결해준다. (데이터 통로)
- 역할에 따라 client socket, server socket으로 구분한다.

## HTTP 통신과 무엇이 다른가?
- HTTP의 경우 클라이언트가 요청을 보내는 경우에만 서버가 응답한다. (단방향 통신)
- 서버로부터 응답을 받은 후에 연결이 종료된다.
- 소켓 통신의 경우 클라이언트와 서버가 특정 Port를 통해 실시간으로 데이터를 주고 받는다. (양방향 통신)
- 소켓 통신의 경우 클라이언트와 서버가 서로 연결을 유지한다.

## Socket API
![출처 : https://12bme.tistory.com/297](/http/images/socket_api.png)
- Java에서는 소켓을 한번 래핑해서 Stream으로 다룰 수 있다.

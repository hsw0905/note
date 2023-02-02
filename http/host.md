# Host

## Host?
- 네트워크가 연결된 컴퓨터
- Switch : 네트워크 그 자체를 이루는 호스트(인프라)
	- 예시 : Router, IPS...
- End-Point : 이용 주체
	-	예시 : Client, Server, Peer...

## DNS(Domain Name System)
- 브라우저 : www.naver.com -> 접속하려면?
	- TCP/IP 연결을 해야 한다 -> IP를 알아야 한다.
	- 이런 IP들을 알고 있는 DB가 있다.
	- DNS 서버 : 도메인의 이름 -> IP 주소로 알려주는 서비스
- 예시
	- Google DNS : 8.8.8.8
	- KT ISP: 168.126.63.1~2

## Domain
- 트리 구조의 체계
- www.naver.com
	- 도메인 이름 : naver.com
	- www : naver.com에 속한 호스트 이름
	- com 영역에 속한 naver에 속한 www
- 생성된 도메인 이름은 언제나 유일(Unique)하도록 구성

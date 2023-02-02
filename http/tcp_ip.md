# TCP/IP

## Internet Protocol Suite
- 인터넷이 연결된 컴퓨터끼리 정보를 주고 받는데 쓰는 프로토콜의 모음
- 이들 중 TCP와 IP가 가낭 많이 쓰여 흔히 TCP/IP 라고 부른다.

## IP(Internet Protocol)
![ip](/http/images/ip.png)
- L3(Layer3) 계층
- Host에 대한 식별자
- IPv4, IPv6 두 가지 타입
- 32 bit (8bit * 4)
- 처음 24bit : 네트워크 주소
- 마지막 8bit : 호스트 ID
---
## TCP vs UDP

### TCP(Transmission Control Protocol)
- L4(Layer4) 계층
- Port: 프로세스에 대한 식별자
- 연결이라는 개념이 있다.
- 상대방이 데이터를 잘 받았는지 확인하기 때문에 데이터가 정확히 전달했음을 보장한다. (신뢰성)
- 데이터 단위 : Segment
- 3-way-handshaking
	- 상대방과 사전에 세션을 연결하는 과정
	- 데이터 전달이 시작되기 전 양쪽 모두 서로가 준비되었다는 것을 알 수 있도록 한다.
- 4-way-handshaking
	- 상대방과 세션을 종료하기 위해 진행되는 과정

### UDP(User Datagram Protocol)
- L4(Layer4) 계층
- Port: 프로세스에 대한 식별자
- 연결이라는 개념이 없다.
- TCP와 달리 신뢰성을 보장하지 않는다. (손실되는 데이터가 발생할 우려가 있다.)
- TCP와 달리 스스로 속도제어를 하는 기능이 없다.
- 데이터 단위 : Datagram

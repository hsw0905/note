# 서버 - 클라이언트

## 구조와 역할
- 클라이언트는 HTTP를 사용하여 서버에 메시지를 요청한다.
	- (서버는 어떤 요청이 올지 모르니 꼼꼼하게 검사(validation)하는 로직이 들어갈 수 밖에 없다.)
	- 클라이언트는 서버의 응답이 올때까지 대기
	- (클라이언트는 서버의 응답이 언제 올지 모르니 응답 여부를 기다리지 않고 일단 UI는 그려야 한다.)
- 클라이언트는 서버의 응답이 오면 응답 결과에 맞춰 동작한다.

## 상대적 역할
- 서버는 동작에 따라 클라이언트 역할을 할수도, 서버 역할을 할 수도 있다.

## 개념적 분리
- 클라이언트 개념과 서버 개념을 분리한다.
	- 클라이언트는 UI, 사용자 경험에, 서버는 비즈니스 로직에 집중할 수 있다.



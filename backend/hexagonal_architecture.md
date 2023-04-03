# Hexagonal Architecture

## 왜 나오게 되었는가?
- UI와 비즈니스 로직이 뒤섞이는 문제를 해결하기 위해

## Layered Architecture를 사용하면 되지 않나?
- Layered Architecture 에서의 접근 방식
	- User-side
	- Database-side
	- 두 가지를 구분
- Hexagonal Architecture 에서의 접근 방식
	- 위 두가지를 모두 Application의 외부로 동일하게 취급한다.
	- 위 둘은 모두 동일한 구조를 갖게 된다.
		- Application과 Adapter는 Port로 연결된다. (Port는 interface)
			- Application -> Port
			-                  ^
			-                  |
			-               Adapter
	- Application이 다른 외부 요소와 분리되어 테스트하기 쉽고 재사용하기 쉬워진다.
		- Application 입장에서는 UI 입력이나, MockMvc 입력이나 동일한 외부
		- Application 입장에서는 Real DB나 Mock DB나 동일한 외부
---
![출처: https://alistair.cockburn.us/hexagonal-architecture/](/backend/images/barn_door.png)

### Port
- User-side -> Application : Incoming Port
- Application -> Database : Outgoing Port

### POJO
- Plain Old Java Object
- 특정 자바 모델이나 기능, 프레임워크 등을 따르지 않은 자바 오브젝트를 지칭하는 말

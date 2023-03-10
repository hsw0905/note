# API(Application Programming Interface)

- 컴퓨터 프로그램 사이의 연결점
- 인터페이스 : 명세 또는 시그네처(signature)
- API를 사용 : API를 호출(Call)한다.
	- 엔드포인트
	- 서브 루틴, 메소드
- 목적 : 동작하는 실제 내부 구현은 숨겨져 있다.
	- 일부 공개된 통로를 통해서만 외부에서 사용할 수 있다.

## Web API
- Web 상에서 다른 서비스에 요청을 보내고 응답을 받기 위해 정의된 명세 (엔드포인트)

## 정보은닉 (Information Hiding)
- 프로그램의 구체적인 세부 구현을 노출시키지 않는 원칙 (뒤로 숨긴다!)
	- 구현을 직접 의존하지 않도록 함으로써 기능(디자인)의 변경에 대한 유연함을 제공
	- 하나의 인터페이스에 다양한 구현체를 제공하여 동적으로 기능 변경이 가능
	- 코드를 모듈화하여 주어진 코드를 사용하기 위해 이해해야할 내용의 양을 줄여준다.
- 정보은닉의 여러 방법 중 한가지로 캡슐화를 한다.
	- 캡슐화가 되어 있다고 반드시 정보은닉이 보장되진 않는다.

## 캡슐화 (Encapsulation)
- 객체의 속성(맴버변수)과 행위(메서드)를 하나로 묶는다.
	- 특정 속성이나 메소드를 외부에서 사용할 수 없도록 직접적인 접근을 막는다.
	- 외부에서는 객체가 제공하는 필드와 메서드만 이용 가능하다.

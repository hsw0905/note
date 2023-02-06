# IoC(Inversion of Control)

- 목적 : 컴포넌트의 의존성 제공 + 관리
- 종류 : DL, DI

## DI(Dependency Injection) vs DL(Dependency Lookup)
- DL
	- 의존성 풀 존재
		- 예시 : xxx.xml
	- 컴포넌트 스스로 의존성 참조를 가져와야 한다.
		- 컴포넌트들을 다시 결합하는 추가 코드가 필요하다.

- DI
	- IoC 컨테이너가 컴포넌트에 의존성을 주입한다. (초기 컴포넌트 접근을 위해 의존성 룩업 필요)
	- 생성자 주입
		- Bean 객체를 불변 객체로 사용할 수 있다.
	- Setter 주입
		- 컴포넌트가 의존성을 컨테이너로 노출

- Main()
	- IoC 컨테이너 생성
	- ApplicationContext -> 의존성 가져오기
- 스프링 Web MVC 개발
	- DI 사용

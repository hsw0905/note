# REST(Representational State Transfer)
- 네트워크 기반 소프트웨어에서 필요한 아키텍처 스타일
- 범위 : 네트워크 기반 애플리케이션에 한정 (논문 2장)
	- 관심 사항
		- 성능
		- 규모확장성
		- 단순성
		- 수정용이성
		- 가시성
		- 이식성
		- 신뢰성

## REST를 이끌어내려면
1. Null Style에서 시작 (www)
2. 클라이언트 - 서버
	- 목적 : 클라이언트의 이식성 + 서버의 규모 확장성을 개선하기 위함
	- 내용 : 괌심 사항을 분리한다. (사용자 인터페이스 - 데이터 저장)
3. Stateless
	- 모든 요청은 필요한 모든 정보를 담고 있어야 한다.
	- 상태가 유지되지 않는다.
4. Cache
	- 목적 : 효율성, 규모 확장성, 사용자 입장에서 성능을 개선하기 위함
	- 내용 : 캐시가 가능해야 한다.
5. Uniform Interface (중요)
	- 다른 네트워크 기반 아키텍처 스타일과 구분되는 핵심
	- 목적 : 전체 시스템 아키텍처가 단순해지고
		- 가시성 개선
		- 구현과 서비스가 분리되어 독립적인 진화(고도화)가 가능해지기 위함
	- 내용 : 구성 요소 사이의 인터페이스는 균일해야 한다. Uniform Interface를 얻기 위해서는 4가지 제약 조건이 필요하다.
	- 필딩 제약 조건
		1. URI로 리소스를 식별할 수 있다.
			- (하이퍼텍스트 레퍼런스가 될 수 있는 것이라면 어느 개념이든 가능)
		2. 표현으로 리소스를 조작한다.
			- 표현?
				- 어떤 리소스의 특정 시점의 상태를 반영하고 있는 정보
					- representation data
					- representation metadata (Content-Type, Content-Language...)
				- 서버에 저장된 무언가가 아니다.
			- 리소스를 조작하다.
				- 사실상 HTTP Message를 어떻게 조작하느냐와 같은 이야기이다.
				- HTTP Message는 HTTP Method로 표현한다.
				- 리소스는 Content-Type과 Message Body로 표현한다.
		3. 자기서술적 메시지
		4. HATEOAS(Hypermedia as the Engine of Application State)
			- 엔진 : 하나의 상태를 다른 상태로 바꾸는 엔진
			- 하이퍼미디어 링크
			- 하지만 현업에서는 API 명세 문서 (잘 지켜지지 않음)
6. 계층 구성
	- 계층된 구조로 구성이 가능해야 한다.
7. Code-On-Demend(옵션)
	- 클라이언트에서 서버로부터 온 프로그램을 실행될 수 있어야 한다.
---
![출처 : https://martinfowler.com/articles/richardsonMaturityModel.html](/rest_api/images/rest_step.png)
- HTTP Message만 만들어도 레벨 2단계 가능
- 특정 링크를 담으면 3단계
- 현업에서는 2단계 레벨만 되어도 REST API로 만족한다.



## 도메인 주도 설계 핵심 - 반 버논

### 서서히 발생하는 문제
- DB 위주의 개발
	- 비즈니스 로직보다 데이터 모델에 집중하게 되는 현상
- 비즈니스 목적에 맞지 않는 클래스, 오퍼레이션 이름 짓기
- 프로젝트에 대한 지나친 예측
	- 오히려 개발이 지연되는 결과
	- 큰 진흙덩어리
- UI 코드에 비즈니스 로직 코드가 섞이게 됨
- 과도한 일반화로 인한 잘못된 추상화
- 서비스와 서비스 사이의 직접 호출(강한 결합)

### 효과적인 설계
- 전략적 설계 후 전술적 설계
	- 바운디드 컨텍스트 -> 도메인 모델을 분리 -> 보편언어 개발
		- 서브도메인
	- 여러 개의 바운디드 컨텍스트를 통합
		- 어떻게 통합하느냐? -> 컨텍스트 맵핑
	- Entity + Value Object -> Aggregate로 묶는다.
	- 도메인 이벤트

### 바운디드 컨텍스트
- 의미적으로 동일한 컨텍스트의 범위
- 문제영역 -> 개발자들이 똑똑해지고, 비즈니스 로직이 자세하고 명확해지면 -> 해결영역으로 전환
- 바운디드 컨텍스트 내에서 메인 소스, 테스트 코드로 해결 방안을 개발한다.
- 반드시 엄격하게 핵심만 걸러내어 보편언어로 정의한다.
- 팀에 할당한다면
	- 바운디드 컨텍스트 1개당 한 팀
		- 팀마다 코드 Repository 하나, DB 스키마도 명확하게 분리
		- 인수 테스트와 단위 테스트 유지
		- 공식 인터페이스 정의 -> 다른 팀에서 우리 바운디드 컨텍스트를 사용할 수 있도록
	- 도메인 전문가가 필요하며, 개발자와 같이 협업해야 한다.

### 보편언어
- 도메인 모델에 나타난 개념
- 구체적인 시나리오를 나타낼 수 있어야 한다.
	- 도메인 모델이 어떻게 동작해야 하는가?
- 시나리오를 계속해서 개선하라
	- 현재 모델에 "누가? 또는 무엇일까?" 라는 질문을 던진다.

### 서브도메인
- 전체 비즈니스 도메인의 하위 부분
- 바운디드 컨텍스트 1개 당 1개의 서브도메인
- 타입
	- 핵심도메인
		- 주요 자원을 할당하는 명시적인 바운디드 컨텍스트
		- 비즈니스에서 다른 기업보다 차별화 되는 포인트
		- 가장 많은 리소스 투자 필요
	- 지원 서브도메인
		- 이미 존재하는 제품으로도 문제를 해결할 수 없어서 맞춤 제작이 필요한 도메인
	- 일반 서브도메인
		- 이미 존재하는 제품으로 해결 가능한 도메인

### 컨텍스트 매핑
- 핵심 도메인을 다른 바운디드 컨텍스트와 통합하는 것
- 종류
	- 파트너십
		- 두 팀이 함께 성공하거나 다같이 실패하거나
	- 공유 커널
		- 작지만 공통인 모델을 공유하는 관계
	- 고객-공급자 (하류-상류)
		- 공급자가 관계를 주도(고객이 원하는 것을 제공한다.)
		- 현실적인 팀 관계
	- 준수자
		- 매우 거대한 모델(AWS 등)과의 통합이 필요할 때
	- 반부패 계층
		- 상류 모델과 하류 모델 사이의 번역 계층
- 어떤 인터페이스로 제공할 것인가?
	- RPC
	- RESTful HTTP
	- Messaging
		- 멱등 수신자 : 오퍼레이션이 여러 번 수행되도 같은 결과가 되어야 한다.

### Aggregate
- Aggregate 경계 내에서 비즈니스 불변사항을 보호하라
- 작은 Aggregate을 설계하라
	- 빠르게 로드
	- 작은 메모리 차지
	- 빠른 GC
	- 단일 책임 원칙(SRP)
- 오직 ID를 통해 다른 Aggregate를 참조하라
	- 동일한 트랜잭션 내에 다른 Aggregate를 수정하지 않는 규칙
- 결과적 일관성을 사용해 다른 Aggregate를 갱신하라
- 모델링
	- 가장 먼저 Aggregate Root Entity 클래스 생성
		- 고유한 식별성을 가지도록
	- Aggregate를 찾는 데 필요한 속성(필드)을 찾는다.
	- 행위와 관련된 메서드를 사용하여 속성 값 변경
	- 추상화는 조심스럽게 할 것
	- 일관성 경계 목표
		- 오직 1개의 Entity를 갖는 Aggregate Root 생성
			- Root Entity와 가장 관련이 깊은 속성으로 채운다.
		- 변경될 때 함께 갱신돼야 하는 것이 있는지 파악한다. (도메인 전문가와 상의!)
			- 갱신에 걸리는 시간 파악
		- 갱신이 일어나는 시간 파악
		- 각각의 Aggregate들이 즉시 처리 되어야 한다면
			- 동일 Aggregate 경계 안에 2개 Entity 구성을 고려
		- 그렇지 않다면(주어진 시간에 따라 Aggregate마다 각각 반응)
			- 결과적 일관성을 사용하여 다른 Aggregate를 갱신하라
- 주의사항
	- 빈약한 도메인 모델이 되지 않도록 할 것
	- 비즈니스 로직이 Application 계층까지 새어 나가지 않도록 주의할 것
	- 비즈니스 로직을 Helper, Util 클래스에 위임시키지 말 것

### 도메인 이벤트
- 바운디드 컨텍스트 내의 비즈니스 관점에서 중요한 사항들에 대한 기록
- 인과관계 일관성
	- 특정 오퍼레이션이 다른 Aggregate에서 명확하게 발생하기 전에는 다른 Aggregate이 생성되거나 수정될 수 없다.
- 도메인 이벤트에 어떻게 이름을 붙일 것인가?
	- 보편언어 반영
	- 과거형 동사로 표현
- 명령은 거부할 수 있지만 도메인 이벤트는 실제 발생하는 것이고, 논리적으로 부정할 수 없다.
- 이벤트 소싱
	- Aggregate 인스턴스에 대해 변경된 것에 대한 기록
	- 발생했던 모든 도메인 이벤트를 저장하는 것
	- 어떤 사유로 인해 메모리에서 삭제되었던 Aggregate들을 이벤트 스트림을 통해 온전히 환원시킬 수 있다.
- 이벤트 Repository
	- 도메인 이벤트를 추가하는 순차적인 repository 컬렉션 또는 테이블

# ORM(Object-Relation Mapping)
- Application Class 와 SQL Table 사이의 맵핑 정보를 정의한 메타데이터를 사용해서
- 객체를 SQL의 테이블에 자동으로 영속화 해주는 기술
	- 맵핑 정보를 정의한 메타데이터 : 어떤 클래스가 어떤 테이블에 맵핑이 되는가에 대한 정보
- 목적 : 도메인 모델 기반으로 코딩을 하기 위함
	- OOP 기반
	- 코드 재사용
	- 유지보수 용이

## 왜 등장하게 되었는가?
- 패러다임 불일치
	- 객체를 Relation에 맵핑하려다보니 생기는 문제
	- 객체와 관계형 DB는 지향하는 목적이 서로 다르다.
	- 개발자가 이 불일치 문제를 중간에서 해결해야 하는데, 너무 많은 비용이 든다.

## JPA(Jakarta Persistence API)
- Java 진영에서 사용하는 ORM 표준 인터페이스의 모음
	- 구현체 : 주로 Hibernate를 사용한다.
- Application과 JDBC 사이에서 동작한다.
- 제공하는 기능
	- 엔티티와 테이블을 매핑하는 설계
	- 매핑한 엔티티를 실제 사용
- JPA가 제공하는 다양한 쿼리 방법
	- JPQL
	- QueryDSL
	- JPA Criteria
	- Native SQL
	- JDBC API 직접 사용, MyBatis, Spring JdbcTemplate 함께 사용

### Jakarta EE
- Jakarta Enterprise Edition
- 자바를 이용한 서버측 개발을 위한 플랫폼
- Java : 오라클이 만들었다가 최근 오픈소스화 되었다.
- 이때, JavaEE (J2EE)로 불리던 Enterprise 기술이 상표권 문제로 Jakarta EE로 부르게 되었다.

## Entity
- 관게형 DB에 저장된 테이블을 나타낸다.
- 엔티티 객체는 해당 테이블의 row에 해당된다.

## Entity Manager
![출처 : 김영한님 - 자바 ORM 표준 JPA 프로그래밍](/spring/orm/images/entitymanagerfactory.png)
- EntityManagerFactory로부터 생성 (팩토리는 App 전체 1개)
- 각 Request에 대하여 EntityManager가 Connection Pool의 스레드를 사용하여 대응
- 동시에 여러 스레드가 접근하게 되면 동시성 문제가 발생하므로 스레드간 공유하면 안된다.
- Entity Manager를 사용하여 엔티티를 영속성 컨텍스트에 저장한다.

### Persistence Context
- 엔티티를 영구 저장하는 환경
- 엔티티를 식별자 값으로 구분한다.
	- 반드시 식별자 값이 있어야 한다.
- 생명주기
	- 비영속(new/transient) : 순수 객체 상태
	- 영속(managed) : 영속성 컨텍스트에 의해 관리되고 있는 상태
	- 준영속(detached) : 영속 상태에서 분리된 상태
		- 이미 영속화된 적이 있으므로 반드시 식별자를 가지고 있다.
		- 1차캐시, 쓰기 지연 SQL 저장소까지 해당 엔티티 관련 모든 정보가 제거된다.
		- clear() : 영속성 컨텍스트 내 모든 엔티티를 준영속 상태로 만든다.
	- 삭제(removed) : 엔티티가 영속성 컨텍스트와 DB에서 삭제된 상태

### 영속성 컨텍스트에서 관리하면 뭐가 좋은가?
![출처 : 김영한님 - 자바 ORM 표준 JPA 프로그래밍](/spring/orm/images/persistence_context.png)
- 1차 캐시
	- 컨텍스트 내부 Map에 저장됨
- 동일성 보장
	- 1차 캐시에 저장된 객체를 사용
- 트랜잭션을 지원하는 쓰기 지연
	- 커밋 할때 모아둔 쿼리를 DB에 반영
- 변경 감지
	- 컨텍스트 내부 Map에 저장할 때 최초 상태(스냅샷) 저장
	- 스냅샷과 비교하여 엔티티 변경사항 감지하여 쓰기 지연 SQL 저장소에 보낸다.
- 지연 로딩
	- 실제 객체 대신 프록시 객체를 로딩해두고, 해당 객체를 실제 사용할 때 영속성 컨텍스트를 통해 데이터를 불러오는 방법

## 트랜잭션
- [정리 링크](https://hsw0905.gitbook.io/note/database/transaction)

## JPQL
- [정리 링크](https://hsw0905.gitbook.io/note/spring/orm/jpql)


# ORM(Object-Relation Mapping)
- Application Class 와 SQL Table 사이의 맵핑 정보를 정의한 메타데이터를 사용해서
- 객체를 SQL의 테이블에 자동으로 영속화 해주는 기술
	- 맵핑 정보를 정의한 메타데이터 : 어떤 클래스가 어떤 테이블에 맵핑이 되는가에 대한 정보
- 목적 : 도메인 모델 기반으로 코딩을 하기 위함
	- OOP 기반
	- 코드 재사용
	- 유지보수 용이

## 왜 등장하게 되었는가?
- 데이터 모델 vs 객체 모델
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

---
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

---
## JPA 데이터 타입
- 엔티티 타입
	- @Entity로 정의하는 객체
	- 데이터가 변하더라도 식별자를 사용하여 변경사항 추적 가능하다.
- 값 타입
	- 단순히 값으로 사용하는 자바 타입
	- 식별자가 없기 때문에 데이터가 변경되면 추적이 불가능하다.

### 값타입 분류
- 기본값 타입 : 생명주기를 엔티티에 의존한다.
	- 자바 원시타입
	- 래퍼클래스(Integer, Double..)
	- String
	- 사이드 이펙트(공유 값 변경)가 없다.
- 임베디드 타입
	- 커스텀 값 타입
	- 기본값 타입을 모아서 만든 복합 값타입
	- 해당 값 타입만의 의미 있는 메소드를 만들 수 있다.
	- 생명주기를 엔티티에 의존한다.
		- 사용방법
			- @Embeddable : 값 타입 정의하는 부분에 표시
			- @Embedded : 값 타입 사용하는 부분에 표시
			- 이때 기본 생성자 필수로 필요하다.
	- 하나의 값타입을 여러 엔티티에서 참조하게 되면?
		- 사이드 이펙트 발생
		- 그러므로 값타입을 복사해서 사용해야한다.
		- 또는 불변객체로 사용해야한다.
- 컬렉션 값 타입
	- 값 타입을 하나 이상 저장할 때 사용한다.
	- 컬렉션을 저장하기 위한 별도의 테이블이 필요하다.
	- 속해있는 @Entity 객체를 영속화하게 되면 알아서 같이 저장된다.
	- 즉, 영속성 전이(Cascade) + 고아 객체 제거 기능을 필수로 가진다.
	- 컬렉션에 변경사항이 생기면 해당 엔티티와 연관된 모든 데이터를 삭제하고, 컬렉션에 있는 현재 값을 다시 저장한다.
		- 사용방법
			- @ElementCollection 필요
			- @CollectionTable(name = "xxx...", joinColumns = @JoinColumn(name = "xxx..")) 테이블 매핑
			- 실무에서는 컬렉션 값타입 대신 일대다 관계를 고려할 것

## 엔티티 매핑
- 객체와 테이블 매핑
	- @Entity
		- @Entity가 붙은 클래스 : JPA가 관리한다.
		- 기본 생성자가 꼭 필요하다.
		- final 클래스, 혹은 필드에 final 사용하면 안된다.
	- @Table
		- 엔티티와 매핑할 테이블을 작성한다.
- 필드와 컬럼 매핑
	- @Column
- 기본키 매핑
	- @Id

## 스키마 자동 생성
- App 로딩 시점에 DDL을 사용해 테이블 자동 생성 (개발에서만 사용)
- 운영서버에서는 사용하지 않거나, 다듬어서 사용한다.
- ddl-auto 속성
	- create: App 시작시 매번 테이블 지우고 새로 생성 (개발에서만 사용)
	- update: 변경사항만 반영 (개발, 테스트 서버에서만 사용)
	- validate: 엔티티와 테이블이 정상 매핑되었는지만 확인한다. (스테이징, 운영서버)
	- none: (스테이징, 운영서버)

## 연관관계 맵핑
- 객체와 테이블의 연관관계 차이
	- 객체는 참조
	- 테이블은 외래키
- 방향
	- 객체는 단방향 2개 (양방향)
	- 테이블은 외래키 1개
- 예시
	- Member <-> Team = N : 1 관계
	- ```java
		@Entity
		public class Member {
			@Id
			@GeneratedValue
			private Long id;
			...
			@ManyToOne // 다대일 관계
			@JoinColumn(name = "team_id") // 참조할 외래키 설정
			private Team team; // 객체를 필드에 지정
			...
		}
		...
		@Entity
		public class Team {
			@Id
			@GeneratedValue
			private Long id;
			...

			@OneToMany(mappedBy = "team")
			private List<Member> members = new ArrayList<>();
		}
		```
- 1:1 관계
	- 주 테이블에 외래키 지정
	- 주 테이블만 조회해도 대상 테이블에 데이터가 있는지 확인 가능하다.
	- 값이 없으면 외래 키에 Null을 허용하게 된다.
---

## 연관관계의 주인
- 양방향 매핑
	- mappedBy : 나는 xxx에 의해 관리받고 있다
	- 연관관계의 주인은 mappedBy 없다.
	- 객체는 양방향 매핑이라 둘 중 한 객체에서 외래키 관리를 해야한다.
	- 연관관계의 주인은 두 관계 중 하나의 객체를 지정한다.
		- 누구를 주인으로 하지?
			- 테이블에서 외래키를 가진 엔티티 !
	- 주인이 아닌 쪽은 읽기만 가능하다.
- JPA는 <수정, 삭제>시 연관관계의 주인인 엔티티만 참고한다.
	- 무한루프 주의!
		- to_string
		- lombok에서 to_string 금지
		- JSON - 컨트롤러에서 Entity 반환하지 말 것
			- 반드시 Dto로 변환해서 반환하기

### @JoinColumn(name = "...") 주의사항
- @OneToMany에서 이거 안넣으면 Join Table로 작동 -> 못 보던 테이블이 하나 생성됨
	- 양쪽 엔티티에 모두 @JoinColumn(...) -> 양쪽 엔티티 모두 연관관계 주인설정됨 -> 하이버네이트에서 에러 안나고 실행됨(이러면 안된다!!)
		- 해결 : 양쪽 엔티티 중 연관관계의 주인이 아닌 쪽에다 @JoinColumn(..., insertable = false, updatable = false) <-- 읽기 전용 결과
			- 위와 같이 넣어주면 결과적으로 @ManyToOne 설정 + 양방향 설정과 같은 결과가 나온다.

### @OneToMany 주의사항
- 실무에선 @OneToMany 보다 @ManyToOne으로 한 후 반대편에 양방향 설정을 추천함
	- 왜냐하면 반대편 테이블의 외래 키를 관리하는 특이한 구조가 되기 때문 (어? 나는 Team 엔티티를 save 했는데 왜 Member에서 Update 쿼리가 발생하지?)

### @OneToOne 주의사항
- 주 테이블과 대상 테이블이 있을 때 (서로 1:1관계)
	- 주 테이블에 외래키를 설정하는 경우(실무에서 이 방법을 추천함!!!)
		- 장점 : 주 테이블만 조회해도 대상 테이블의 데이터까지 확인할 수 있다.
		- 단점 : 값이 없다면 외래키에 null값을 허용하게 된다.
	- 대상 테이블에 외래키를 설정하는 경우
		- 장점 : 나중에 일대일 -> 일대다로 관계가 변경되는 경우 테이블 유지가 가능하다.
		- 단점 : 프록시 기능의 한계로 지연로딩으로 설정해도 즉시로딩되버린다.

### @ManyToMany 주의사항
- 실무에서 안쓴다.
- 연결 테이블을 중간에 넣고 [@OneToMany, 중간테이블, @ManyToOne] 관계로 풀어낸다.

### Member : Team (ManyToOne) 관계에서 고민
- 비즈니스 로직에 따라 Member 로딩 시 Team도 같이 가져오면 좋을텐데 !!
- 아니, 특정 로직엔 Member만 가져오는게 더 이득인걸?
- 이럴땐 어떻게 해결할까? --> 프록시!
---
## 상속 관계 매핑
- 객체의 상속관계 구조와 DB의 슈퍼타입-서브타입 관계를 매핑
- 슈퍼타입-서브타입 논리 모델을 실제 물리 모델로 구현하는 방법
---
![조인 전략](/spring/orm/images/join_stratagy.png)
- 조인 전략 : 각각 테이블로 변환 (중요한 테이블, 복잡한 로직일 때)
```Java
@Entity
@Inheritance(strategy = InheritanceType.JOINED)
@DiscriminatorColumn // DType 생성을 위해 필요, Default 로 서브타입 엔티티 이름이 들어간다.
public abstract class Item {
	@Id
	@GeneratedValue
	private Long id;
	private String name;
	...
}

...
@Entity
@DiscriminatorValue("A") // <- 커스텀 구분 이름
public class Album extends Item {
	...
}
```
- 장점
	- 테이블이 정규화 되어있음
	- 외래 키 참조 무결성 제약조건 활용 가능
	- 저장공간 효율화
- 단점
	- 조회시 조인 사용, 성능 저하
	- 조회 쿼리 복잡
	- Insert 쿼리 2번 호출
---
![단일 테이블 전략](/spring/orm/images/single_stratagy.png)
- 단일 테이블 전략 : 통합 테이블 하나로 변환 (확장될 일이 없을 때)
```Java
@Entity
@Inheritance(strategy = InheritanceType.SINGLE_TABLE) //<- default 전략
// @DiscriminatorColumn 어노테이션 없어도 테이블에 DType 생긴다.
public abstract class Item {
	@Id
	@GeneratedValue
	private Long id;
	private String name;
	...
}
```
- 장점
	- 조인이 없으므로 조회 성능 빠름
	- 조회 쿼리 단순
- 단점
	- 자식 엔티티가 매핑한 컬럼은 null값 허용
	- 단일 테이블에 모든 것을 저장 -> 테이블이 커질 수 있다. (성능이 오히려 저하 우려)
---
- 구현 클래스마다 테이블 만들기 : 서브타입 테이블로 변환 (추천 X)

### @MappedSuperClass
- 엔티티간 공통 속성을 상속받아 엔티티를 만드는 경우 사용
- 매핑 정보만 받는 슈퍼클래스, entityManager -> 조회, 검색 안된다.
- 직접 객체를 생성해서 사용할 일이 없다 -> 추상클래스 권장
```Java
@MappedSuperClass
public abstract class BaseEntity {
	...
}
```
---
## 프록시
![프록시 동작방식](/spring/orm/images/proxy.png)
- 프록시를 알려면 entityManager.find() vs entityManager.getReference()의 차이를 이해해야 한다.
	- entityManager.find() : DB를 통해 실제 엔티티를 조회한다.
	- entityManager.getReference() : DB 조회를 미루는 가짜(프록시) 엔티티 객체를 조회한다.
		- getReference() 호출 시점엔 DB의 쿼리가 안 나간다.
		- getReference() 호출 결과 객체는 프록시 객체이다. (예: class ...Member$HibernateProxy...)
		- 결과값을 직접 조회하는 시점에 쿼리가 나간다.
- 프록시 객체는 하이버네이트가 내부적으로 진짜 Entity를 상속받아 만들어진다. 그래서 진짜 Entity 객체와 겉모습이 동일하다.
	- 사용하는 입장에서, 가짜인지 진짜 객체인지 구분하지 않고 사용하면 된다.
- 그렇다면 프록시 객체는 무슨 일을 하냐?
	- 프록시 객체는 실제 객체의 참조(target)를 가지고 있다.
	- 이 객체를 호출하면 프록시 객체는 실제 엔티티 객체의 메소드를 호출한다. (실제 객체로 교체되는게 아니다!!)
		- 타입 체크할때 주의해야할 사항 : == 비교가 아니라 instance of 를 사용해야 한다.
			- JPA 엔티티를 비교할 땐 프록시를 사용하는지 안하는지 모르기 때문에 왠만하면 instance of를 사용할 것
- 프록시 객체는 처음 사용할 때 한번만 초기화 하며, 초기화 방식은 위 이미지 참조
	- 내부적으로 EntityManager에게 초기화 요청한다는 점이 포인트
	- 두 번째 호출할 때부터는 EntityManager에 이미 실제 엔티티가 있으므로, getReference()를 호출하면 실제 객체가 나온다.(프록시X)
	- 준영속 상태일때 프록시를 초기화하면 Exception 발생하므로 주의

### 지연 로딩
```Java
@Entity
public class Member {
    @Id
    @GeneratedValue
    private Long id;
    ...
    @ManyToOne(fetch = FetchType.LAZY)  // <-- 지연로딩 : entityManager.find().. 조회 시 프록시로 조회한다.
		@JoinColumn(name = "teams_id")
		private Team team;
}
...
team.getName() // <-- 실제 객체 사용 시 그때서야 쿼리 나가고 초기화된다.
```

### 즉시 로딩
```Java
@Entity
public class Member {
    @Id
    @GeneratedValue
    private Long id;
    ...
    @ManyToOne(fetch = FetchType.EAGER) // <-- 즉시로딩 : 프록시가 아니라 진짜 객체를 한 번에(left outer join) 모두 조회
		@JoinColumn(name = "teams_id")
		private Team team;
}
...
```
### 프록시와 즉시로딩 주의
- 가급적 지연 로딩만 사용할 것 (즉시 로딩은 JPQL에서 N+1 문제를 일으킨다.)
- @ManyToOne, @OneToOne은 기본이 즉시 로딩이다.
	-> Lazy로 설정할 것
- @OneToMany, @ManyToMany는 기본이 지연 로딩이다.

### 영속성 전이 : CASCADE
- 특정 엔티티를 영속 상태로 만들면서, 연관된 엔티티도 함께 영속 상태로 만들고 싶을 때 사용
- 연관관계 매핑과는 관련 없다.
- 종류
	- ALL : 모두 적용
	- PERSIST : 영속
	- REMOVE : 삭제
	- 기타
		- MERGE, REFRESH, DETACH
- 예시
	- 게시판과 해당 게시물의 첨부 파일 관계(소유자가 1개)

### 고아 객체 제거: orphanRemoval = True
- 부모 엔티티와 연관관계가 끊어진 자식 엔티티를 자동으로 삭제
- 참조하는 곳이 하나일때 사용해야 한다.
- 특정 엔티티가 개인 소유할 때 사용한다.

## N+1 문제
- 연관관계가 설정한 엔티티를 조회할 때 조회된 데이터 갯수(N)만큼 연관관계의 조회 쿼리가 추가로 발생하여 데이터를 읽는 문제
- 왜 발생하는가?
	- JpaRepository에 정의된 인터페이스 메소드를 실행하면
		- JPA는 해당 JPQL을 생성해서 실행한다.
		- 즉, 엔티티 객체와 필드 이름으로 쿼리를 만든다(테이블 X)
		- 만약 연관된 엔티티 데이터가 필요하다면
		- FetchType으로 지정한시점에 조회를 별도로 호출한다.
- 해결 방안?
	- Fetch join을 사용한다.
		- JpaRepository에서 제공해주진 않는다. JPQL로 작성해야 한다.
		- 실제 로그를 확인하면 INNER_JOIN으로 호출된다.
	- 단점
		- 페치 조인 대상에는 별칭을 줄 수 없다.
		- 둘 이상의 컬렉션은 페치 조인 할 수 없다.
		- 컬렉션을 페치 조인하면 페이징 API(setFirstResult, setMaxResult)를 사용할 수 없다.
		- 카테지언 곱이 발생하게 되므로 중복 데이터가 존재할 수 있다.
			- 중복된 데이터를 제거하려면?
				- 컬렉션 Set 사용
				- distinct
	- EntityGraph
		- @EntityGraph(attibutePath = "xxx") <- 쿼리 수행시 바로 가져올 필드명 지정
		- Eager 조회로 가져오게 된다.
		- 실제 로그를 확인하면 OUTER_JOIN으로 실행된다.
	- 단점
		- 카테지언 곱이 발생하게 되므로 중복 데이터가 존재할 수 있다.
		- 중복된 데이터를 제거하려면?
				- 컬렉션 Set 사용
				- distinct
---
## Spring Data JPA
![출처: https://hackmd.io/@ddubson/rkn-sR4wU](/spring/orm/images/spring_data_jpa.png)
- Spring Data Common : Spring Data 프로젝트들의 공통 기능 제공
- Spring Data JPA : Spring Data Common 기능 + JPA 관련 기능

### JPA Repository vs DDD Repository
- JPA Repository: Spring Data에서 사용되는 인터페이스
- DDD Repository: 도메인 모델 계층과 구현 기술을 분리시켜 구현 기술에 종속적이지 않고 도메인에 집중할 수 있는 패턴에서 나온 용어
	- 도메인 모델 계층 : 저장하는 방법에 대한 관심사
	- 인프라스트럭처 계층 : 실제로 어떻게 저장하는지에 대한 관심사
	- 인프라스트럭처 계층이 도메인 계층을 의존하도록 설계

### 기타 DDD 관련 정리
- [정리 링크](https://hsw0905.gitbook.io/note/backend/ddd)

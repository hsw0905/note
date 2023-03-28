# JPQL(Java Persistence Query Language)
- 객체지향 쿼리 언어
- 엔티티 객체를 중심으로 개발(테이블이 아닌 객체 대상으로 쿼리)
- 실제 SQL로 번역되어 실행된다.
- 특정 SQL에 의존되지 않는다.

## 문법
- 엔티티와 속성은 대소문자를 구분한다.
- JPQL 키워드는 대소문자 구분 안한다. (select, from, WHERE)
- 테이블 이름이 아니라 엔티티 이름을 사용한다.
- 별칭은 필수 (as는 생략 가능하다.)
- Insert 문은 없다.


## TypedQuery vs Query
- 반환 타입이 명확하다 ! -> TypedQuery
- 명확하지 않다 ! -> Query
- projection
	- ```java
		TypedQuery<Member> query = entityManager.createQuery("select m from Member as m where m.age > 20", Member.class);

		Query query = entityManager.createQuery("select m.age, m.name from Member");
		```
	- 결과 조회
	- 결과가 하나 이상
		- query.getResultList()
		- 없으면 빈 리스트
	- 결과가 단 하나
		- query.getSingleResult()
		- 없으면 Exception 발생(주의)

### 파라미터 바인딩
- 이름 기준
- 위치 기준
- 예시
	- ```java
		Member result = entityManager.createQuery("select m from Member as m where m.username = :username", Member.class)
		                             .setParameter("username", usernameParam)
		                             .getSingleResult();


		//위치 기준은 숫자 위치가 바뀌면 순서가 꼬이므로 잘 쓰지 않는다.
		TypedQuery<Member> query = entityManager.createQuery("select m from Member as m where m.username = ?1", Member.class")
		                                        .setParameter(1, usernameParam);
		```

### 프로젝션(SELECT)
- 엔티티 조회 쿼리 -> 결과 엔티티는 모두 영속성 컨텍스트에서 관리된다.
- ```Java
	List<Member> result = entityManager.createQuery("select m from Member m", Member.class)
	                                   .getResultList();
	```
- 참조 엔티티를 조회할 때
	- ```Java
		// Member가 Team 엔티티를 참조할 때
		// Team을 조회하고 싶다면?
		List<Team> result = entityManager.createQuery("select m.team from Member m", Team.class)
		                                 .getResultList();
		// 결과는 Team이 반환되지만, 내부적으로 Inner Join 쿼리가 실행된다.
		// 이럴땐 위의 JPQL 쿼리 보단 다음과 같이 Join을 명시적으로 보여주는 것이 좋다.
		List<Team> result = entityManager.createQuery("select t from Member m join m.team t", Team.class)
		                                 .getResultList();
		```
- 스칼라 엔티티이고, 타입이 여러가지일 때(Dto로 바로 빼기)
	- ```Java
		List<MemberDto> result = entityManager.createQuery("select new com.example.jpql.MemberDto(m.username, m.age) from Member m")
	                                        .getResultList();
		```

### 페이징 API
- 몇 번째부터 : setFirstResult(n)
- 몇 개까지 : setMaxResults(m)
	- ```Java
		List<Member> result = entityManager.createQuery("select m from Member order by m.age desc", Member.class)
		                                   .setFirstResult(1)
		                                   .setMaxResults(10)
		                                   .getResultList();
		```

### 조인
- 내부조인
	- ```SQL
		select m from Member m (inner) join m.team
		```
- 외부조인
	- ```SQL
		select m from Member m left(outer) join m.team
		```
- 세타조인
	- ```SQL
		select count(m) from Member m, Team t where m.username = t.name
		```
- 조인 대상 필터링
	- ```SQL
		select m,t from Member m left join m.team t on t.name = 'abc'
		```

### 서브쿼리
- (not) exists (subquery)
- {all | any | some} (subquery)
- (not) in (subquery)

### JPA 서브쿼리 한계
- JPA는 where, having 절 뒤에만 서브쿼리 사용 가능
- select절 뒤에 서브쿼리는 하이버네이트 사용하면 가능 (사실 대부분 하이버네이트 쓰니까)
- from 절 뒤에 서브쿼리는 현재 JPQL에서 불가능
	- 조인으로 해결해보고 안되면 쿼리를 분해해서 날리거나 네이티브SQL 쓰기

### 경로표현식
- 상태 필드
- 단일 값 연관 경로
	- 묵시적 내부조인(Inner Join) 발생 <- 실무에서는 사용하지 않는 것을 권장
	- 객체 탐색 가능
- 컬렉션 값 연관 경로
	- 묵시적 내부조인(Inner Join) 발생 <- 실무에서는 사용하지 않는 것을 권장
	- 컬렉션에서 객체 탐색 불가

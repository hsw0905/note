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
		TypedQuery<Member> query =
		entityManager.createQuery("select m from Member as m where m.age > 20", Member.class);

		Query query =
		entityManager.createQuery("select m.age, m.name from Member");
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
		TypedQuery<Member> query =
		entityManager.createQuery("select m from Member as m where m.username = :username", Member.class);
		query.setParameter("username", usernameParam);

		TypedQuery<Member> query =
		entityManager.createQuery("select m from Member as m where m.username = ?1", Member.class");
		query.setParameter(1, usernameParam);
		```

# QueryDSL

## JPQL 대신 QueryDSL를 사용하면 뭐가 좋은가?
- 컴파일 시점에 오류를 잡아준다. (JPQL은 문자열로 쿼리를 작성하기 떄문에 런타임 오류가 생길 수 있다.)
- 파라미터 바인딩을 자동으로 해결해준다.

### Q-Type
- static import 사용하면 깔끔
	- ```Java
		import static ...QMember.*;
		...
		@Test
		void querydslTest() {
		  Member findMember = queryFactory
		          .select(member)
		          .from(member)
		          .where(member.username.eq("member1"))
		          .fetchOne();

		assertThat(findMember.getUsername()).isEqualTo("member1");
		}
		```

### 결과 조회
- fetch() : 리스트 조회, 없으면 빈 리스트 리턴
- fetchOne() : 단 건 조회
	- 결과가 없으면 null
	- 둘 이상이면 NonUniqueResultException 발생
- fetchFirst() : limit(1).fetchOne()
- fetchResults() : 페이징 정보가 포함되며, total count 쿼리가 추가로 발생한다.
	- 컨텐츠가 복잡하여 count 쿼리가 별도로 만들어져 있을 경우엔 사용 안함
- fetchCount() : count 수 조회

### 정렬, 페이징
- 예시
	- ```Java
		List<Member> result = queryFactory
		        .selectFrom(member)
		        .orderBy(member.username.desc())
		        .offset(1)  // <- 앞에서부터 몇 개를 스킵한다는 뜻
		        .limit(2)
		        .fetch();
		```


### 집합
- 예시
	- ```Java
		// 팀의 이름과 각 팀의 평균 연령을 구해보기
		List<Tuple> result = queryFactory
		        .select(team.name, member.age.avg())
		        .from(Member)
		        .join(member.team, team)
		        .groupBy(team.name)
		        .fetch();

		// 튜플 결과는 리스트처럼 받는다.
		    Tuple teamA = result.get(0);
		    Tuple teamB = result.get(1);
		...
		```

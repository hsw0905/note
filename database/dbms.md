# Database
- 구조화 되어있는 데이터

## DBMS(DataBase Management System)
- 다수의 사용자가 데이터에 접근할 수 있게 해주는 소프트웨어 도구의 집합
- 사용자의 요구를 처리하여 적절한 데이터를 응답한다.

## RDBMS(Relational Database Management System)
- 우리가 일반적으로 말하는 관계형 DB
- NoSQL은 RDBMS가 아니다

## Database 언어
- DDL(Data Definition Language) - Schema
- DML(Data Manipulation Language) - Query, Command
- DCL(Data Control Language) - Grant, Revoke, Commit, Rollback
	-	위 3가지는 모두 SQL로 표현된다.

## Data Model
- 개념적인 모델
	- Entity-Relationship Model : 가장 인기 있는 개념적 데이터 모델
		- ER 모델이라고도 함
		- Entity : 개별적으로 다룰 수 있는 데이터
			- 주의 : 객체지향의 Entity와 다른 맥락의 의미이다.
				- 객체지향의 Entity : 연속성과 식별성이 있는 객체를 의미
		- Entity Type : 같은 Attributes를 가진 Entity의 집합
			- ERD를 그릴 때 쓰인다.
- 논리적인 모델
	- Relational Model : 가장 인기 있는 논리적 데이터 모델
	- Hierarchical Model
	- Network Model
	- Object-based Model
- 물리적인 모델

### 관계형 모델에서 사용되는 개념
![출처: 위키피디아](/database/images/Relational_database_terms.svg)
- Relation
	- 동일한 구조로 이루어진 Tuple의 집합
	- 즉, (속성 집합, 튜플 집합)의 쌍
	- 속성 집합: Schema로 표현
	- Table로 구현
- Tuple
	- (속성, 값) 쌍의 집합
		- 속성의 이름이 유일해야 한다.
		- 중복, Null 허용
	- Row, Record로 구현
- Attribute
	- 속성
	- 이름/타입
		-	타입: 문자열, 문자, 숫자
	- Column으로 구현
		- 가로 : Row
		- 세로 : Column
---
## 관계 대수
- Relation을 처리하는 연산의 집합
- 하나 이상의 Relation으로 새로운 Relation을 만들 수 있다.

### 관계대수 연산자 종류
- 일반 집합 연산자
	- Union(합집합)
	- Intersect(교집합)
	- Difference(차집합)
	- Cartesian Product(곱집합)
		- Tuple을 합치다.
		- From 뒤에 관계 이름을 나열한다.
		- ```SQL
			SELECT *
			FROM r1, r2;
			```

- 순수 관계 연산자
	- Selection
		- 주어진 술어(조건)을 만족하는 Tuple만 선택하다.
		- Where절을 이용해 술어를 표현한다.
		- ```SQL
			SELECT name, age, gender
			FROM people
			WHERE age < 13;
			```
	- Project : 원하는 속성만 포함하다.
		- ```SQL
			SELECT name, age, gender
			FROM people;
			```
	- Join
		- Selection + Cartesian Product
		- 즉, 본래 Select와 Where 구문으로 표현한다.
			- ```SQL
				SELECT people.name, age, gender, items.name AS item_name, usage
				FROM people, items
				WHERE people.name = items.person_name;
				```
		- 이것을 편리하게 JOIN으로 표현한다.
			- ```SQL
				SELECT people.name, age, gender, items.name AS item_name, usage
				FROM people
				JOIN items ON people.name = items.person_name;
				```
		- Outer join: Null인 사람도 포함하고 싶을 때
			- ```SQL
				SELECT people.name, age, gender, items.name AS item_name, usage
				FROM people
				LEFT OUTER JOIN items ON people.name = items.person_name;
				```
	- Division

---
## ERD(Entity-Relationship Diagram)
- ER 모델을 시각화하는 방법
- 관계 표현을 현업에서는 사용하지 않을 수도 있지만 일단 말이 되도록 사용해보자.
- 예시
	- 하나의 계정이 여러개의 캐릭터를 갖는다.
	- 하나의 지역이 여러개의 캐릭터를 포함한다.

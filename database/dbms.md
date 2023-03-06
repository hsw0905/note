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
- 논리적인 모델
	- Relational Model : 가장 인기 있는 논리적 데이터 모델
	- Hierarchical Model
	- Network Model
	- Object-based Model
- 물리적인 모델

### 관계형 모델에서 사용되는 개념
- Relation
	- (속성 집합, 튜플 집합)의 쌍
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

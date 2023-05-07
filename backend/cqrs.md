# CQRS

## CQS(Command Query Separation)
- 메서드를 시스템의 상태를 바꾸는지 -> 기준으로 두 가지로 분류한다.
	- 상태 : 시간에 따라 변하는 값
	- Query : 결과값을 리턴, 시스템의 상태를 변화시키지 않는다. (부작용에서 자유롭다.)
	- Command : 결과를 반환하지 않고 시스템의 상태를 변화시킨다.
- 왜 굳이 나누는가?
	- 특히 로직이 복잡할 때
		- 언제 어떻게 상태가 변하는지 예측하기 힘들면 -> 유지보수가 어렵다.
	- 잘 나눈다는 것의 의미 : 메서드 단위로 SRP를 지키는 것이기도 하다.

## CQRS(Command Query Responsibility Segregation)
![출처 : https://cqrs.files.wordpress.com/2010/11/cqrs_documents.pdf](/backend/images/cqrs.png)
- Application에서 Read Model과 Write Model을 분리하자.
- 상태를 바꿀 필요가 없는 Read Model은 읽기 모드로 사용자 트래픽이 쌓인다.
	- 성능을 고려해서 미리 보기 좋은 형태로 만들어 놓자.
	- -> DB를 읽기용과 쓰기용으로 분리한다.
- Application에서 상태가 바뀔 때마다, Read Model을 업데이트 한다.
	- 업데이트를 언제 해야 할까?
		- Application 계층의 Service에서 한다.
		- 혹은 Event Sourcing을 사용한다. (단, 인프라 입장에서 세팅해야 할 리소스가 많다.)

## SSOT(Single Source Of Truth)
- Write Model은 한 곳에서만 제어한다. (본래 데이터가 저장된 곳은 단 하나)
- 원본 저장소 외 데이터가 존재하는 다른 곳에서는 원본 저장소의 데이터 위치를 참조한다.

## Materialized View
- Query의 결과를 담고 있는 DB Object.(스냅샷이라고도 함)
- 일반적으로 RDBMS에는 View가 존재한다.
	- 이 View는 Select를 할 때마다 재구성되기 때문에 퍼포먼스가 떨어진다.
- 효율적인 쿼리를 지원하기 위해 필요한 결과에 적합한 형태로 Matrialized View를 미리 생성한다.
- 무엇이 좋아지는가?
	- Application은 필요한 정보를 빠르게 가져올 수 있다.
	- 주기적으로 DBMS가 업데이트를 한다. (마치 전문화된 캐시에 해당된다.)

## OLTP & OLAP
- OLTP (Online Transaction Processing)
	- 온라인 트랜잭션 처리
	- 여러 사람이 다수의 트랜잭션을 실시간으로 실행할 수 있게 해준다.
	- 빠른 응답 시간
	- 적은 양의 데이터를 자주 수정하며, 일반적으로 read, write 작업간 균형이 유지된다.
	- 인덱스화된 데이터를 사용해서 응답 시간을 개선한다.
- OLAP (Online Analytical Processing)
	- Data를 의미 있는 정보로 치환하거나 복잡한 모델링을 가능하게끔 하는 분석 방법
	- 분석을 목적으로 트랜잭션을 질의하는 작업을 포함한다.
	- OLTP에 비교하면 느린 응답시간
	- 일반적으로 읽기 집약적인 작업이며, 다수의 레코드를 포함하는 복잡한 쿼리이다.
	- 대량의 레코드에 쉽게 액세스 할 수 있도록 칼럼 형식으로 데이터를 저장한다.

## Event Sourcing
- Application -> 이벤트 저장소
	- 데이터에서 수행된 각 작업을 명확하게 설명하는 일련의 이벤트를 보낸다.
	- 각각의 이벤트는 데이터에 대한 변경 내역들이다.
	- 이벤트 저장소는 위 이벤트(변경 내역들)를 게시하고, 이벤트 소비자에게 알리고 필요하다면 처리할 수 있도록 한다.
	- 예를 들어 소비자는 구독한 이벤트의 기록으로 다른 시스템에 적용하는 Task를 실행한다거나 특정 작업을 완료하기 위해 사용한다.
- 이벤트를 구독하여 소비 역할을 하는 시스템은 Application 코드와 분리된다.
- 무엇이 좋아지는가?
	- 이벤트를 처리하는 Task를 백그라운드에서 실행할 수 있다.
	- 트랜잭션 처리 중에 경합이 없다.
	- 이벤트는 데이터 저장소를 직접 업데이트 하는게 아니고, 단순히 적절한 시기에 처리되도록 기록만 한다.
	- DB 저장소의 객체를 직접 업데이트하지 않아도 되기 때문에 동시 업데이트로 인한 충돌을 방지할 수 있다.(대신 멱등성을 보장해야 한다.)


## Redis (Remote Dictionary Server)
- 아주 빠른 데이터 저장소
	- DB와 혼합하여 Cache로 사용
- 분산된 서버들간 커뮤니케이션(동기화, 작업 분할 등)
- 내장된 자료구조를 활용한 각종 기능 구현

## In-memory
- 데이터를 디스크에 저장하지 않음
- 휘발성 RAM에 저장
- 빠른 속도

## NoSQL
- Not Only SQL 혹은 No SQL
- 관계형 DB에서 사용하는 SQL을 사용하지 않는다는 의미
- 비 관계형 데이터베이스를 지칭할 때 사용한다.
- 왜 등장하게 되었는가?
	- 스토리지 비용이 저렴해진다.
	- RDBMS의 장점이 정규화로 인한 데이터 중복 최소화인데, 스토리지 비용이 저렴해진 지금은 데이터 중복이 큰 문제가 아니게 된다.
	- 다루는 데이터의 크기가 점점 커지게 된다.
		- 이에 따라 성능 요구사항이 커진다.
	- 분산 환경이 대중화 되었다.(Scale-out)
	- 대량의 단순 데이터가 다루기 쉽다.
- 다양한 데이터 모델
	- Key-Value
		- Redis, Memcached, Riak, DynamoDB
	- Wide-Column
		- Cassandra, HBase, Google BigTable
	- Document
		- Value에 JSON 등 형태의 오브젝트가 들어가게 됨
		- MongoDB, CouchDB
	- Graph
		- Neo4j, OrientDB, AgensGraph

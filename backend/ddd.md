# DDD (Domain Driven Design)
- Domain : 소프트웨어로 해결하고자 하는 문제 영역
- 도메인 그 자체와 도메인 로직에 초점을 맞춘다.

### Entity
- 고유 식별자를 갖는 DB 테이블 모델

### Value Object
- 식별자를 가지고 있지 않은, 불변 타입 값 객체

## Aggregate
- 서로 관련있는 Entity와 Value Object 묶음
- 트랜잭션, 분산의 단위
- Aggregate Root : 핵심 도메인 기능을 보유 하고 있는 모델




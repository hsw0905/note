# JDBC

## DB Connection 획득
- Server -> DB 드라이버 -> DB
  - DB 드라이버를 통해 커넥션 조회
  - DB 드라이버 <-> DB (TCP/IP 연결)
    - 3 way handshaking
  - 서버 : ID/PW 부가정보 전달
  - DB 드라이버: 인증 완료 + 세션 생성
    - response Connection
    - 앞으로 해당 커넥션을 통한 모든 동작은 세션에서 실행(SQL 실행)
      - (트랜잭션 시작-커밋-롤백 동작)
      - 커넥션을 닫거나, DB 쪽에서 세션 강제 종료시 세션은 종료
      - 커넥션 1개당 세션 1개 생성
  - 많은 비용

### DataSource
- 자바 진영 인터페이스 - 커넥션 획득하는 방법을 추상화
- 설정과 사용 책임 분리

### Connection Pool
- 위의 커넥션을 미리 획득(시스템 스펙에 따라 10개 ~ 그 이상)
- 서버는 미리 획득한 커넥션 객체를 바로 사용 가능
- 사용한 커넥션은 연결이 살아있는 상태로 커넥션 풀에 다시 반환 (재사용)
- 주로 오픈소스 사용 - (보통 HikariCP)


## 트랜잭션

### ACID
- 원자성: 모두 성공 혹은 모두 실패 (하나의 작업처럼)
- 일관성: 일관성 있는 DB 상태 유지
- 격리성: 서로 다른 트랜잭션 간의 간섭, 영향 X
  - 동시에 같은 데이터를 수정하지 못하도록 격리
  - 동시성 관련 성능 이슈
    - 트랜잭션 격리 수준 설정 가능
- 지속성: 트랜잭션이 끝나면 항상 그 결과가 기록되어야 한다.
  - 문제 발생시 로그 등을 확인하여 복구도 가능해야 한다.

### 트랜잭션 격리 수준
- 멀티스레드 환경 - 동시성
- ANSI 표준 - 4단계 수준으로 격리
  - READ UNCOMMITTED(커밋되지 않은 읽기)
    - 성능은 좋지만, 아직 데이터 변경중인데 다른 트랜잭션에서 변경할 우려가 있음
  - READ COMMITTED(커밋된 읽기)
    - 일반적으로 많이 사용
    - 커밋 완료된 데이터만 읽기 가능
  - REPEATABLE READ(반복 가능한 읽기)
  - SERIALIZABLE(직렬화 가능)
- 단계가 높아질수록 DB 성능은 저하, 대신 격리성 보장은 상향

### 트랜잭션 도중
- 데이터를 변경(등록, 수정, 삭제)하고 아직 커밋하기 전이라면?
  - 해당 세션 사용자에게만 변경된 데이터가 보이고 (임시 저장된)
  - 다른 세션에서는 변경 전 데이터가 보인다 (정합성)
- 변경된 데이터를 커밋하지 않고(수동 커밋모드) 놔두면?
  - 설정된 타임아웃 시간이 지나면 자동으로 롤백됨
- Default 설정 : 자동 커밋
- 트랜잭션을 시작한다는 코드의 의미 : 수동 커밋 전환


### 트랜잭션 적용 in Application
- 어느 계층에서 시작해야 되나요?
  - 서비스계층
    - 서비스계층 내부 비즈니스 계층에서 문제 발생시 롤백이 필요
- 어떻게 시작해야 되나요?
  - 커넥션 객체 필요
    - 트랜잭션을 수행하는 동안 같은 커넥션에 연결이 유지되어야 함
      - 그래야 같은 세션 사용 가능
  - 서비스계층에서 커넥션을 만들고 트랜잭션 커밋 후 커넥션 종료해야 함
  - 그러러면 서비스계층에서 만든 커넥션을 비즈니스 로직 도중 close 하면 안된다.
    - finally
      - 모든 비즈니스 로직이 종료된 후, 서비스 계층에서 커넥션을 close 한다.
      - 주의할 점은 커넥션을 닫기 전 다시 auto commit 상태로 돌려놔야 한다.
  - 서비스계층에서 트랜잭션 도중 예외 발생시 롤백을 수행한다. (catch 문)
- 트랜잭션 도중에 1차 캐시?
  - 트랜잭션 도중에만 발생하는 캐시가 있다.
  - 커밋되면 캐시 삭제
  - 실무에서 이것으로 크게 성능 차이를 내거나 하진 않는다
  - JPA는 기본 DB 락 기능이 모두 지원되며 나아가 낙관적 락이라는 기술도 제공한다.

### Raw 한 JDBC 코드를 서비스 계층에 두면 불편한 점
- 반복되는 커넥션 객체 얻는 로직, 특정 DB 기술에 종속
- 반복되는 try-catch-finally 문 (트랜잭션 시작 - 롤백 - 커밋)
- 순수한 서비스 계층에 JDBC 코드 섞임
- 트랜잭션을 위해 커넥션을 계속 파라미터로 보내줘야 함
- 데이터 접근 계층에서 발생한 예외가 서비스 계층으로 전파 (SQL Exception)

### 그리하여 위 문제를 Spring에서 해결한 방법은..
- 각기 달랐던 트랜잭션 코드를 추상화
	- 트랜잭션 동기화 매니저 제공
	- 트랜잭션 매니저 -> 트랜잭션 동기화 매니저에 커넥션 보관
	- 리포지토리는 트랜잭션 동기화 매니저에 접근하여 보관된 커넥션 사용
	- 트랜잭션 종료 : 트랜잭션 매니저 -> 보관된 커넥션을 사용하여 close()
	- 멀티스레드 환경에서, 쓰레드 로컬을 사용하여 커넥션 동기화 가능
		- 커넥션 파라미터로 보내줘야 하는 문제 해결
```java
@RequiredArgsConstructor
public class MemberService {
    private final PlatformTransactionManager transactionManager;
    private final MemberRepository memberRepository;

    public void transferAccount(String fromId, String toId, int money) {
        TransactionStatus status = transactionManager.getTransaction(new DefaultTransactionDefinition());

        try {
            //비즈니스 로직
            bizLogic(fromId, toId, money);
            transactionManager.commit(status); //성공시 커밋
        } catch (Exception e) {
            transactionManager.rollback(status); //실패시 롤백
            throw new IllegalStateException(e);
        }

    }
}
```

- 트랜잭션 템플릿 제공
	- 반복되는 패턴(try-catch-finally) 문제 해결
	- 템플릿-콜백 패턴
```java
public class MemberService {
    private final TransactionTemplate txTemplate;
    private final MemberRepository memberRepository;

    public MemberService(PlatformTransactionManager txManager, MemberRepository memberRepository) {
        this.txTemplate = new TransactionTemplate(txManager);
        this.memberRepository = memberRepository;
    }

    public void transferAccount(String fromId, String toId, int money) {
        txTemplate.executeWithoutResult((status) -> {
            try {
                //비즈니스 로직
                bizLogic(fromId, toId, money);
            } catch (Exception e) {
                throw new IllegalStateException(e);
            }
        });
    }
}
```

- 트랜잭션 프록시 제공(AOP)
- 프록시 객체는 트랜잭션 로직 처리
- 물론 관련 객체는 자동으로 빈으로 등록
- 프록시 호출 -> 프록시가 Service 로직 호출
	- 트랜잭션 처리하는 객체와 비즈니스 로직 처리하는 객체(Service)
```java
public class MemberService {
    private final MemberRepository memberRepository;

    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    @Transactional
    public void transferAccount(String fromId, String toId, int money) throws SQLException {
        bizLogic(fromId, toId, money);
    }
}
```

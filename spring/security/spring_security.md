# Spring Security

## Authentication(인증)
- Application이 이를 이용하려는 사람을 식별하는 프로세스
	-	 Server : 당신은 누구입니까?
	- Client : 나는 XXX 입니다!

## Authorization(인가)
- 인증된 유저가 특정 기능과 데이터에 대한 이용 권리가 있는지 확인하는 프로세스
	- Client : (허락되지 않은) 리소스에 접근
	- Server : 당신은 사용할 권한이 없습니다 !


### HTTP는 Stateless 하다
- HTTP는 Stateless 하다
	- HTTP Request마다 독립적이다.
	- Client는 항상 자기가 누구인지 Server에게 인증 받아야 한다.
	- Server는 내부적으로 인증과 인가 모두 검증한다.

## Cookie vs LocalStorage
- Cookie
	- 웹사이트에 의해 사용자 컴퓨터에 저장되는 작은 텍스트 파일(최대 4KB)
	- 용도
		- 사용자가 방문한 페이지 저장
		- 사용자의 로그인 정보 저장 등
		- 문자열만 저장 가능
	- 유형
		- Persistent Cookies
			- 만료일을 포함하지 않는다.
			- 브라우저가 닫히면 영구적으로 해당 쿠키는 삭제된다.
		- Session Cookies
			- 만료일을 가진다.
			- 만료일까지 유저의 디스크에 저장되고 만료일이 지나면 삭제된다.
			- 특정 웹사이트에서의 행동을 기록

- LocalStorage
	- HTML5가 등장하며 새로운 데이터 저장 옵션으로 도입
	- 새로운 JavaScript 객체 (5MB 이상)
		- SessionStorage
			- 현재 페이지가 브라우징되고 있는 브라우저 컨텍스트 내에서만 데이터가 유지된다.
			- 브라우저가 종료되면 데이터도 지워진다.
			- 브라우저를 하나 더 실행해서 같은 페이지를 보고 있다 하더라도 브라우저 컨텍스트가 다르므로 두 SessionStorage는 별개 영역이다.
		- LocalStorage
			- 브라우저를 종료해도 데이터는 보관된다.
			- 다음번 접속시 저장되어 있던 데이터를 사용 가능하다.
			- 도메인만 같다면 전역적으로 공유 가능하다.
	- 사이트의 도메인 단위로 접근이 제한된다.
		- A 도메인에서 데이터 저장 -> B 도메인에서 조회 불가
	- JavaScript 코드를 통해 삭제되지 않는한 삭제되지 않는다.
	- 문자열 외에 JavaScript 객체, 원시타입 자료형도 저장할 수 있다.
	- 장점
		- Cookie와 다르게 모든 HTTP 요청에서 데이터를 주고 받을 필요가 없다.
			- 서버간 트래픽, 낭비되는 대역폭의 양을 줄일 수 있다.
			- 인터넷이 끊어져도 데이터가 삭제되지 않는다.
	- 주의
		- 저장된 데이터는 해킹에 대비해 암호화 하는편이 좋다.
		- 인터넷 연결이 끊어졌다가 다시 연결이 되었을 때는 로컬에 있던 버전을 삭제하는 것이 좋다.

## Interceptor vs Filter (vs AOP)
![출처 : https://velog.io/@_koiil/Filter-Interceptor-AOP](/spring/security/images/filter.png)
- [Dispatcher Servlet?](/spring/webmvc/dispatcher.md)
- Filter
	- Servlet Container에 의해 관리
		- 시점 : Dispatcher Servlet에 요청이 전달되기 전, 전달된 후
		- 기능 : url 패턴에 맞는 요청에 대해서 부가작업을 처리할 수 있는 기능을 제공
		- 용도 : 전역적으로 공통된 보안 및 인증/인가 관련 작업, 이미지/데이터 압축, 인코딩
	- 스프링 밖에서 처리되지만 스프링 빈으로 등록 가능
	- 개발자가 Request, Response 객체를 다른 객체로 바꿔 제공할 수 있다.
- Interceptor
	- Spring Container에 의해 관리
		- 시점 : Dispatcher Servlet이 핸들러 매핑을 통해 컨트롤러를 호출하기 전, 호출한 후
		- 기능 : Request, Response를 참조
		- 용도 : API 호출에 대한 로깅, Controller로 넘겨주는 정보를 가공
	- 스프링의 모든 Bean 객체에 접근할 수 있다.
- AOP(참고)
	- 시점 : 대상 메소드가 호출되기 전, 호출된 후
	- 기능 : 파라미터, 어노테이션 등 다양한 방법으로 대상 메소드를 지정한다.
	- 용도 : 로깅, 트랜잭션, 에러 처리 등 비즈니스 로직 메소드의 호출 전/후로 중복되는 부분을 줄이기 위해 사용

### 암호화와 복호화
- 암호화 종류
	- 단방향
		- 용도 : 신원 증명, 인증 과정
		- 복호화 불가능
		- Hashing Algorithm
			- 완벽하게 안전하지 않음 (무차별 대입 공격, 레인보우 테이블 공격)
			- 이에 대응하고자 솔트 기법이 나오게 됨
				- 무작위 데이터를 해시 함수 입력값에 더해서 더 복잡한 출력값을 생성한다.
	- 비밀키
		- 비밀키를 사용하여 암호화 / 복호화 가능
		- 송신자와 수신자 모두 암호화 키를 알고 있어야 한다.
	- 공개키
		- 공개키 + 개인키 사용
		- 개인키를 가진 사람만 복호화 가능

### Identifier(식별자)
- 어떤 대상을 유일하게 구별할 수 있는 이름
- 예시
	- URL, ISBN, IP주소, DB key

## OAuth2
- [정리 링크](/backend/oauth2.md)

### Bearer Token
- HTTP Header
	- Authorization: Bearer "token hash value"
	-                <Type>    <Credentials>
- JWT 혹은 OAuth에 대한 토큰을 사용한다.(RFC 6750)

## SecurityFilterChain
![출처 : https://docs.spring.io/spring-security/reference/servlet/architecture.html](/spring/security/images/securityfilterchain.png)
- 클라이언트로부터 Request가 서버에 온다.
- Spring Containter에 Request가 도착하기 전 Filter에 의해 여러 기능이 처리된다.
- DelegatingFilterProxy : Spring Container에 있는 FilterChainProxy에게 Request를 위임한다.(즉, Request를 Spring Container에서 처리할 수 있게 된다.)
- FilterChainProxy는 SecurityFilterChain 인터페이스의 구현체 리스트를 가지고 있다. (다수의 Security Filter)
- Request에 매핑되는 Filter들을 모두 통과하면 DispatcherServlet으로 위임이 성공한다.
- 이후 DispatcherServlet은 적절한 Controller를 호출한다.

### @EnableWebSecurity
- Spring Boot가 Spring Security 자동 설정을 해준다.
- 모든 요청에 대해 401 응답을 내보내던 기존 설정에서 자동 설정으로 403을 내보내게 된다.
- ```Java
	@Configuration
	@EnableWebSecurity
	public class WebSecurityConfig {
	    private final TokenAuthenticationFilter tokenAuthenticationFilter;

	    public WebSecurityConfig(TokenAuthenticationFilter tokenAuthenticationFilter) {
	    // 사용자 정의 필터
	    this.tokenAuthenticationFilter = tokenAuthenticationFilter;
	  }

	    @Bean
	    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
	        http.addFilterBefore(tokenAuthenticationFilter, BasicAuthenticationFilter.class);

	        http.authorizeHttpRequests().anyRequest().authenticated();
	    return http.build();
	    }
	}
	```

## Filter vs OncePerRequestFilter
- ```Java
	public interface Filter {
	    public default void init(FilterConfig filterConfig) throws ServletException {}

	    public void doFilter(ServletRequest request, ServletResponse response,
	          FilterChain chain) throws IOException, ServletException;
	    public default void destroy() {}
	}

	...

	public abstract class OncePerRequestFilter extends GenericFilterBean {
	    ...
	    protected abstract void doFilterInternal(
			HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
			throws ServletException, IOException;
	    ...
	}
	```
- GenericFilterBean : Servlet의 Filter를 조금 더 확장하여 스프링에서 제공하는 필터이다. 이 필터 덕분에 기존 Filter에서 얻어올 수 없던 정보인 Spring 설정 정보를 가져올 수 있게 되었다.
- Filter, GenericFilterBean 두 필터 모두 매 Servlet 마다 호출이 된다.
	- Servlet은 다른 Servlet으로 dispatch 되는 경우가 있는데, 이 때 또 다시 filter chain을 거치게 된다. -> 필터가 두번 실행되는 현상
- OncePerRequestFilter : 이 추상 클래스를 구현한 필터는 Request 하나 당 딱 한번만 실행된다. 즉, 모든 서블릿에 일관된 요청을 처리하기 위해 만들어진 필터이다.

### Filter Chain
- Request 하나가 거치게 되는 필터들의 모음
- 각 필터는 서로 연쇄적으로 작동한다.

### Security Context
![출처 : https://docs.spring.io/spring-security/reference/servlet/authentication/architecture.html](/spring/security/images/securitycontextholder.png)
- SecurityContextHolder로부터 얻어진다.
- SecurityContextHolder는 ThreadLocal을 사용하기 때문에 SecurityContext는 항상 같은 스레드 안에서 이용 가능하다.
- 현재 스레드의 보안 정보를 담고 있고, 이 컨텍스트는 SecuritContextHolder에 저장된다.
- Authentication 객체를 가지고 있다.

### CSRF(Cross Site Request Forgery)
- 사이트 간 요청 위조
- 사용자가 자신의 의지와 무관하게 공격자가 의도한 행위(수정, 삭제, 등록)를 웹 사이트에 요청하게 하는 공격


### Java Date vs LocalDateTime
- Date
	- 구버전 클래스
	- 특정 시점을 날짜가 아닌 밀리초 단위로 표현한다.
	- 1900년을 기준으로 오프셋, 0부터 시작하는 달 인덱스
	- 안씀
- LocalDateTime
	- Java 8 이상
	- LocalDate와 LocalTime 두 클래스를 갖는 복합 클래스
	- 날짜와 시간 모두 표현 가능
	- ```Java
		LocalDate date = LocalDate.parse("2023-04-15");
		LocalTime time = LocalTime.parse("14:40:10");

		LocalDateTime dt = LocalDateTime.of(2023, 4, 15, 14, 39, 10)
		LocalDateTime dt2 = LocalDateTime.of(date, time);
		LocalDateTime dt3 = date.atTime(14,41,20);
		LocalDateTime dt4 = time.atDate(date);
		```

### Epoch Time
- Unix와 POSIX 등 시스템에서 날짜, 시간의 흐름을 표현할 때 기준으로 삼는 시간
- 1970년 1월 1일 0시
- (00:00:00 UTC on January 1, 1970)
	- 이 때부터 특정 시점까지 몇 초(Second)가 지났는가
	- 1970년 1월 1일 자정 이전 시점은 음수 초 단위로 표기


## PasswordEncoder (Spring Security)
- text를 암호화하는 인터페이스(단방향 암호화)
- 구현체로 여러 암호화 알고리즘이 있다.
	- 예시(매번 임의의 Salt를 생성하여 인코딩)
		- BcryptPasswordEncoder
		- Argon2PasswordEncoder
		- Pbkdf2PasswordEncoder
		- SCryptPasswordEncoder
- Argon2 : 2015년 암호 해싱 대회 우승
	- 암호를 해싱하는데 걸리는 시간, 소요되는 메모리 양을 설정할 수 있다.
	- 목적에 맞게 파라미터 변경으로 적용 가능
	- 종류
		- Argon2d
		- Argon2i
		- Argon2id
			- Password hashing, Password 기반 키 유도에 적합

### TSID(Time-Sorted Unique Identifiers)
- 생성 시간순 정렬 가능
- 13 char로 저장 가능
- 64 bit integer로 저장 가능
- UUID, ULID, KSUID보다 짧다.

## JWT(Json Web Token)
- [정리 링크](/backend/jwt.md)

## Claims-based identity
- 유니크한 식별자
- 특정 유저나 Application, computer, 기타 다른 entity를 나타낸다.
- 이 식별자로 무엇이 가능한가?
	- 별도의 자격증명 암호를 여러번 인증할 필요 없이 다양한 리소스에 접근하는 것을 가능하게 한다.
		- 예시
			- Application
			- 네트워크 리소스

### 대칭키 암호화 vs 공개키 암호화
- 대칭키 암호화
	- 암호화, 복호화에 필요한 키가 동일하다.
	- 암호화 속도가 빠르다.
	- 안전한 키 교환 방식이 요구된다. 키를 아는 사람이 많아질수록 키 관리가 어려워진다.
- 공개키 암호화
	- 송 수신자가 모두 한쌍의 공개키, 개인키를 가지고 있다.
	- 공개키 : 모든 사람이 접근 가능한 키
	- 개인키 : 각 사용자만이 가지고 있는 키
	- 예시
		- A가 B에게 데이터를 암호화해서 보내려고 한다.
			- A는 B의 공개키로 데이터를 암호화하여 보낸다.
			- B는 본인의 개인키로 해당 데이터를 복호화해서 본다.
			- 즉, 암호화된 데이터는 B의 공개키에 대응되는 개인키를 갖고 있는 B만 볼 수 있다.
	- 공개키는 공개되어 있기 때문에 따로 키교환이나 분배를 할 필요가 없다.
	- 공개키가 털린다 해도 개인키가 없으면 데이터를 복호화 할 수 없다.
	- 대칭키 암호화 방식에 비해 속도가 느리다.

### @Secured
- 지정된 역할 중 하나 이상 해당되는 유저만 메소드에 접근 가능
- ```Java
	@Secured("ROLE_ADMIN") // 어드민 역할을 가진 유저만 실행할 수 있다.
	public String getUsername() {...}
	```

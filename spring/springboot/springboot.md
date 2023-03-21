# SpringBoot
- 스프링 기반
- WAS(Web Application Server)가 내장된
- 독립 실행형(Stand Alone)
- 복잡한 설정을 자동으로 잡아주는
- 도구 모음

## SpringBoot 핵심 기능
- 내장 서버
- 자동 라이브러리 관리
- 자동 설정
	- 빈이 읽히는 2단계
		- 1단계 : @ComponentScan
			- @Component
			- @Configuration @Repository @Service @Controller @RestController
		- 2단계 : @EnableAutoConfiguration
			- spring.factories (**AutoConfiguration)
			- @**AutoConfiguration 애노테이션도 결국 @Configuration을 사용하고 있다.
			- @ConditionalOn** : 조건에 따라 빈으로 등록하기도 하고 안하기도 하고
- 외부 설정
	- properties
	- yml
	- 환경 변수
	- 커멘드 라인 아규먼트
- 모니터링과 관리 기능

### Profile과 yml 설정
- @Profile : 스프링 코어에서 제공해주는 기능
- yml 예시
	- ```yml
		spring:
		  config:
		    activate:
		      on-profile: dev
		    import: application-dev-secret.yml
		```
	- dev 프로파일 활성화시 사용

### Logging
- 로깅 퍼사드 vs 로거
- 로깅 퍼사드 : 로거 API를 추상화한 인터페이스
	- Commons Logging, SLF4j
- 로거 : 구현체
	- JUL, Log4J2, Logback


## Spring initializer
- 의존성, 버전, 언어 등등 스프링부트 프로젝트를 쉽게 만들어주는 도구

## Web Server vs Web Application Server
- Web Server : 정적 컨텐츠 제공
	- 예시 : Apache Server, Nginx ...
- Web Application Server : 동적 컨텐츠 제공
	- 예시 : Tomcat, Gunicorn ...

### Tomcat
- Java Servlet 컨테이너가 있는 WAS
- 서블릿 생명주기 관리
- 스레드 풀 관리 (멀티스레딩)

## MVC 아키텍처 패턴
- Model, View, Controller
- Model: 도메인 로직
- Controller : 입력(Request)
- View : 표현(Response)

### 관심사의 분리
- 낮은 결합도
- 높은 응집력
- 모듈의 분리

## Spring MVC
- 클라이언트로부터 HTTP 요청이 온다.
- DispatcherServlet : 모든 요청을 받는다.
	- handler 매핑 정보를 조회한다.
	- handler를 처리 가능한 handler 어댑터를 조회한다.
	- 조회한 handler 어댑터에 handler를 넘긴다.
		- handler 어댑터 : handler를 호출한다. (Controller)
			-	Controller : 비즈니스 로직 진행 (Service -> Repository...)
		- handler 어댑터 : ModelAndView를 반환한다.
- DispatcherServlet : viewResolver를 호출한다.
	- viewResolver: View를 반환한다.
- DispatcherServlet : render(Model)를 호출한다.
- View를 통해 (HTML) 응답한다.



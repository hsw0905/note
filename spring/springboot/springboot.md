# SpringBoot
- 스프링 기반
- WAS(Web Application Server)가 내장된
- 독립 실행형(Stand Alone)
- 복잡한 설정을 자동으로 잡아주는
- 도구 모음

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

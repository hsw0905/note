# CORS(Cross-Origin Resource Sharing)
- 서로 다른 출처에서 리소스를 공유한다.
- 브라우저는 OPTIONS 메시지로 먼저 헤더 정보만 확인 (Preflight)
- 서버에서 응답 헤더에 Access-Control-Allow-Origin 속성을 포함한다. (허용하는 호스트)
- 브라우저는 위 헤더를 보고 판단
- 예시
	- HTTP/1.1 200 OK
	- Access-Control-Allow-Origin: http://localhost:3000
	- (...)


## 동일 출처 정책 (Same-Origin Policy)
- 어떤 출처로부터 불러온 문서 또는 스크립트가 다른 출처에서 가져온 리소스와 상호작용 하는것을 제한하는 보안 방식
- 클라이언트는 서버에 이미 요청 후 Response까지 받은 상태
- 웹 브라우저가 처리하는 보안 정책
- 즉 출처를 비교하는 로직은 브라우저에 있다.

## 동일한 출처란 무엇인가?
- 호스트가 다르다 -> 다른 출처
- 포트가 다르다 -> 다른 출처
- 호스트 , 포트가 같지만 path가 다르다 -> 동일 출처 인정
- 예시
	- http://localhost:8080 <-> http://localhost:3000 -> 다른 출처
	- http://news.company.com <-> http://store.company.com -> 다른 출처
- Header에 Origin 확인 (호스트 + 포트)

## 요청시 Request 헤더
- 예시
	- GET /posts HTTP/1.1
	- Host: http://localhost:8080 -> Backend
	- Origin: http://localhost:3000 -> frontend
	- (...)
- 브라우저가 Host, Origin 에 대하여 출처 비교

## JSONP
- <script> 태그는 동일 출처를 비교하지 않는다.
- 서버에서 JSON이 아닌, JS 코드를 전달하는 방법
- 지금은 사용 X (대신 CORS 사용)

## @CrossOrigin
- Spring Web MVC에서 CORS 지정 가능
```java
@CrossOrigin("http://localhost:3000")
public class PostController {
  ...
  @PostMapping
  @ResponseStatus(HttpStatus.CREATED)
  public PostDto create(@RequestBody PostDto postDto) {
    ...
  }
}
```

## App 전체에 적용
```java
@SpringBootApplication
public class DemoApplication {
  public static void main(String[] args) {
    ...
  }
  @Bean
  public WebMvcConfigurer webMvcConfigurer() {
    return new WebMvcConfigurer() {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
      registry.addMapping("/**")
              .allowedMethods("GET", "POST", "PATCH", "DELETE", "OPTIONS")
              .allowedOrigins("http://localhost:3000");
      }
    };
  }
}
```

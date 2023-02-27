# IoC
- Inversion of Control
- 목적 : 컴포넌트의 의존성 제공 + 관리
- 종류 : DL, DI
- 역할
	- 빈 인스턴스 생성
	- 의존 관계 설정
	- 빈 제공

## DI(Dependency Injection) vs DL(Dependency Lookup)
- DL
	- 의존성 풀 존재
		- 예시 : xxx.xml
	- 컴포넌트 스스로 의존성 참조를 가져와야 한다.
		- 컴포넌트들을 다시 결합하는 추가 코드가 필요하다.

- DI
	- IoC 컨테이너가 컴포넌트에 의존성을 주입한다. (초기 컴포넌트 접근을 위해 의존성 룩업 필요)
	- 생성자 주입
		- Bean 객체를 불변 객체로 사용할 수 있다.
	- Setter 주입
		- 컴포넌트가 의존성을 컨테이너로 노출

- Main()
	- IoC 컨테이너 생성
	- ApplicationContext -> 의존성 가져오기
- 스프링 Web MVC 개발
	- DI 사용

### Spring Bean
- 스프링 IoC 컨테이너에 등록된 객체
- 스프링이 관리하는 객체

### 빈으로 등록되면 무엇이 좋은가?
- 의존성 관리
- 스코프
	- default : 싱글톤 객체
	- 프로토타입 : 사용할 때마다 매번 다른 객체
- 라이프사이클

### BeanFactory
```java
@DisplayName("BookService 빈 등록")
    @Test
    void getBeanFromFactory() {
        // given
        BookRepository bookRepository = new BookRepository();
        DefaultListableBeanFactory beanFactory = new DefaultListableBeanFactory();

        GenericBeanDefinition genericBeanDefinition = new GenericBeanDefinition();
        genericBeanDefinition.setBeanClass(BookService.class);

        ConstructorArgumentValues constructorArgumentValues = new ConstructorArgumentValues();
        constructorArgumentValues.addIndexedArgumentValue(0, bookRepository);

        genericBeanDefinition.setConstructorArgumentValues(constructorArgumentValues);

        // when
        beanFactory.registerBeanDefinition("bookService", genericBeanDefinition);

        // then
        BookService bookService = beanFactory.getBean("bookService", BookService.class);

        assertThat(bookService).isNotNull();
    }
```

### ApplicationContext
- BeanFactory + 좀 더 확장된 기능
	- 메시지 소스 처리
	- 이벤트 발행 기능
	- 리소스 로딩 기능
	...

### 어노테이션으로 빈 등록하기
```java
@Configuration
public class AppConfig {
    @Bean
    public BookRepository bookRepository() {
        return new BookRepository();
    }

    @Bean
    public BookService bookService() {
        return new BookService(bookRepository());
    }
}
```
```java
@DisplayName("어노테이션 설정으로 빈 등록")
    @Test
    void getBeanFromAnnotation() {
        // given
        ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);

        // when
        BookService bookService = context.getBean("bookService", BookService.class);

        // then
        assertThat(bookService).isNotNull();
    }
```

### 빈 라이프사이클
1. 빈 초기화, DI
	- XML, 혹은 @Configuration 클래스로부터 ComponentScan
	- 빈 인스턴스 생성
	- 빈 의존성 주입
2. 빈이 스프링을 알아야 하는지 확인
	- 빈이 BeanNameAware를 구현했다면
		- setBeanName() 호출
	- 빈이 BeanClassLoaderAware를 구현했다면
		- setBeanClassLoader() 호출
	- 빈이 ApplicationContextAware를 구현했다면
		- setApplicationContext() 호출
3. 빈 생성 라이프사이클 콜백
	- @PostConstruct 가 적용되어 있다면
		- 적용된 메서드 호출
	- 빈이 InitializingBean 을 구현했다면
		- afterPropertiesSet() 호출
	- init-method 혹은 @Bean(initMethod=""...) 가 있다면
		- 지정된 초기화 메서드 호출
4. 빈 소멸 콜백
	- @PreDestroy 가 적용되어 있다면
		- 해당 메서드 호출
	- 빈이 DisposibleBean을 구현했다면
		- destroy() 호출
	- destroy-method 혹은 @Bean(destroyMethod=""...) 가 있다면
		- 지정된 메서드 호출

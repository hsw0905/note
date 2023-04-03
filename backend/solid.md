# SOLID : 객체 지향 설계 원칙

## SRP(Single Responsibility Principle) : 단일 책임 원칙
- 하나의 클래스가 변경되어야 할 이유가 단 한가지여야 한다.
- 예시 : 장바구니에 상품을 담는 서비스
	- ```Java
		@Service
		@Transactional
		public class AddProductToCartService {
		    private final CartRepository cartRepository;
		    private final ProductRepository productRepository;

		    public AddProductToCartService(CartRepository cartRepository,
		                                   ProductRepository productRepository) {
		        this.cartRepository = cartRepository;
		        this.productRepository = productRepository;
		    }

		    public Cart addProduct(ProductId productId, int quantity) {
		        Product product = productRepository.findById(productId)
		                .orElseThrow();

		        Cart cart = cartRepository.findById(CartId.DEFAULT)
		                .orElse(new Cart(CartId.DEFAULT));

		        cart.addProduct(product, quantity);

		        cartRepository.save(cart);

		        return cart;
		    }
		}
		```
	- 이 클래스는 언제 변경이 필요할까?
		- 장바구니에 상품을 담는 방식(로직)이 변경될 때 (O) <-- 1가지
		- 장바구니 상태를 저장하는 방식이 변경될 때 (X)
			- CartRepository, ProductRepository로 코드를 분리했기 때문
			- (이렇게 분리해놓으면) 덤으로 Mock 테스트도 가능하다. (DB 저장하는 구체적인 방법에는 관심이 없다. 알아서 저장되겠지 !)
### SRP를 지키면 무엇이 좋은가?
- 테스트하기 쉬워진다.
- 코드를 재사용하기 편하다.
- 더 작고 이해하기 쉬운 클래스가 된다.


## OCP(Open-Closed Principle) : 개방-폐쇄 원칙
- 확장엔 열린 상태
- 수정엔 닫힌 상태(변경이 전파되면 안된다.)
- 어떻게 해결하나?
	- 추상화를 사용하며, Java에서는 interface로 해결한다.
- 예시
	- ```Java
		interface CartRepository {
			... // Repository 추상화
		}

		// 로직 변경시 구현체만 바꾸면 된다.
		public class JdbcCartRepository implements CartRepository {
			... // 구현체 1
		}

		public class MongoCartRepository implements CartRepository {
			... // 구현체 2
		}
		```

## LSP(Liskov Substitution Principle) : 리스코프 치환 원칙
- 서브타입(SubType)은 그것의 기반타입(BaseType)으로 치환 가능해야 한다.
- 예시 : 정사각형은 직사각형이다
	- 수학적 직관적으로 맞는 이야기 -> 직사각형 클래스를 상속하는 정사각형 클래스
	- 하지만 코드에서는 상속이 "행위" 관점에서 Is-A 관계인지 확인해야 한다.
	- ```Java
		// 실패하는 테스트
		@Test
		void area() {
		    Rectangle rectangle = new Square();
		    rectangle.changeWidth(5);
		    rectangle.changeHeight(4);
		    assertThat(rectangle.area()).toBe(5 * 4); // <- area() 행위 관점에서 Is-A 관계가 아니다.
		}
		```

## ISP(Interface Segregation Principle) : 인터페이스 분리 원칙
- 응집력이 없는 커다란 인터페이스를 여러 개의 작은 인터페이스로 나누어야 한다.
- 예시 : ProductRepository(interface)의 기능이 많아진 경우
	- SearchProductRepository, CommandProductRepository ... 로 기능에 따라 분리한다.

## DIP(Dependency Inversion Principle) : 의존관계 역전 원칙
- 상위 수준의 모듈은 하위 수준의 모듈에 의존하면 안된다. 양쪽 모두 추상화에 의존해야 한다.
	- 상위 수준의 모듈 : 비즈니스 로직
	- 하위 수준의 모듈 : 비즈니스 로직에서 호출하는 부분
- 추상화는 구체적인 사항에 의존해서는 안된다. 구체적인 사항은 추상화에 의존해야 한다.
- 예시 : Infarastructure Layer -> Application Layer로 의존성 방향을 역전시켜 만든다.
	- CartRepository(interface) 도입
	- JdbcCartRepository : CartRepository의 구현체 클래스
	- 의존성 방향 : JdbcCartRepository -> CartRepository
- 인터페이스에 의존하면서 구체적인 인스턴스를 활용하려면?
	- DI(Dependency Injection)가 필요하다.
	- Spring을 사용하면 아주 쉽게 DI와 DIP를 활용할 수 있다.

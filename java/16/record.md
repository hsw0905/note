# Record
- 데이터만 다룰 목적으로 만들어진 클래스
- 내부적으로 toString(), hashCode(), equals(), final private 필드와 생성자 등을 자동으로 컴파일 타임에 구현해준다.
```java
	public record Person(String name, Integer age, String address) {

}
```
- 위와 같은 형태로 레코드 클래스를 만들 수 있다.
- 위의 record를 동일한 기능의 클래스로 변경하면 다음과 같다.
```java
public final class Person {

  private final String name;
  private final Integer age;
  private final String address;

  public Person(String name, Integer age, String address) {
    this.name = name;
    this.age = age;
    this.address = address;
  }

  public String name() {
    return name;
  }

  public Integer age() {
    return age;
  }

  public String address() {
    return address;
  }

  @Override
  public boolean equals(Object obj) {
    if (obj == this) {
      return true;
    }
    if (obj == null || obj.getClass() != this.getClass()) {
      return false;
    }
    var that = (Person) obj;
    return Objects.equals(this.name, that.name) &&
        Objects.equals(this.age, that.age) &&
        Objects.equals(this.address, that.address);
  }

  @Override
  public int hashCode() {
    return Objects.hash(name, age, address);
  }

  @Override
  public String toString() {
    return "Person[" +
        "name=" + name + ", " +
        "age=" + age + ", " +
        "address=" + address + ']';
  }

}
```
- 클래스는 기본적으로 final이 붙으며 상속은 불가능하다. 추상 클래스가 될 수 없지만, 인터페이스는 구현 가능하다.
- 맴버 변수는 final로 정의된다.
- static 맴버 변수, static 메소드 사용 가능하다.
- getter 메소드 -> getxxx 이 붙지 않는다.

# Object Mapper (Jackson)
- Java에서 Object -> JSON(문자열)로 변환해주는 라이브러리 (Gson도 있다.)
- Spring에서 기본 지원
	- Java DTO -> mapper -> JSON
	- JSON -> mapper -> Java DTO

## @JsonProperty
- DTO에서 기본적으로 getXXX (Getter) 메소드의 get을 제외한 이름으로 지정해준다.
- 이를 JSON 스키마에서 다른 이름으로 사용하고 싶을 때 사용

```java
public class PostDto {
  private String title;
  private String content;

  public PostDto(String title, String content) {
    this.title = title;
    this.content = content;
  }

  public PostDto(){}

  public getTitle() {
    return title;
  }
  @JsonProperty("body") // content -> body로 이름 지정
  public getContent() {
    return content;
  }
}
```

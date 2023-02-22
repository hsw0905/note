# List

![출처: https://bikashdubey42.medium.com/arraylist-vs-linkedlist-in-java-633e68f49d22](/java/collection/images/list.png)

- 정렬된 컬렉션
- 구현체 예시 : ArrayList, LinkedList, Vector, Stack

### ArrayList
- 동적 배열
- 간단한 동적 배열 구현 : [링크](https://github.com/hsw0905/datastructure-demo/blob/main/src/main/java/me/harry/list/MyList.java)

### Arrays.asList() vs List.of()
```java
@SafeVarargs
    @SuppressWarnings("varargs")
    public static <T> List<T> asList(T... a) {
        return new ArrayList<>(a);
    }
```

```java
static <E> List<E> of(E e1) {
        return new ImmutableCollections.List12<>(e1);
    }
...
static <E> List<E> of(E e1, E e2, E e3) {
        return ImmutableCollections.listFromTrustedArray(e1, e2, e3);
    }
...
```
- 공통점 : 삽입, 삭제 불가능, Null 허용 안함
- 차이점
	- Arrays.asList() : 변경 가능 (set(), replace()), 각 원소별 Null 허용
	- List.of() : 변경 불가능, 각 원소별 Null 허용 불가능

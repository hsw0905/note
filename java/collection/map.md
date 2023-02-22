# Map

![출처 : https://bikashdubey42.medium.com/arraylist-vs-linkedlist-in-java-633e68f49d22](/java/collection/images/map.png)

- HashMap : Key, Value 순서 보장 안됨
- TreeMap : Red-Black Tree 기반, Key 기반 정렬 가능
- LinkedHashMap : Key 삽입 순서 유지


### HashMap vs HashTable
- Thread-safe
	- HashMap : X
	- HashTable : O
- Null 허용 여부
	- HashMap : O
	- HashTable : X
- 해시충돌 성능 : HashMap의 경우 보조 해시 함수를 사용 -> 해시충돌이 덜 발생한다.


### 개방 주소법(Open Addressing)
- 비어있는 해시 테이블의 공간 활용
- 종류
	- Linear Probing
	- Quardratic Probing
	- Double Hashing Probing

### 분리 연결법(Seperate Chaining)
- 중복된 버킷의 데이터에 대해 리스트 혹은 트리구조를 사용 (추가 메모리 사용)
- 해시충돌 발생시 노드 연결
- Java HashMap의 경우 리스트의 개수가 8개 이상이면 Red-Black Tree로 변경 (O(logN))

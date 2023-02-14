# Serialization (직렬화)
- 오브젝트의 상태를 오브젝트의 사본으로 다시 변환할 수 있도록 바이트 스트림으로 변환한다.
- (파일 혹은 메모리에서, 또는 네트워크 연결 사이)
	1. 직렬화된 객체를 바이트 단위로 분해한다. (마샬링)
	2. 분해된 데이터를 순서대로 전송한다.
	3. 전송 받은 데이터를 원래대로 복구한다. (언마샬링)

## Deserialization (역직렬화)
- 일련의 바이트로부터 데이터 구조를 추출하는 일

## 마샬링
- 오브젝트의 메모리에서 표현 방식을 전송에 적합한 다른 데이터 형식으로 변환하는 과정
- 오브젝트의 상태와 코드베이스를 기록한다. (코드베이스 : 이 객체의 구현을 어디서 찾을 수 있는가에 대한 정보)

## Serialization vs 마샬링
- 마샬링이 더 큰 범주의 개념
- 직렬화는 객체를 대상으로 하는 반면, 마샬링은 대상과 변환할 오브젝트가 한정되지 않는다.
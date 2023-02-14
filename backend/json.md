# JSON(JavaScript Object Notaion)
- 데이터 포멧(Key-Value 쌍)
- 변환 흐름
	- Java DTO -> 변환기 -> JSON 문자열
	- JSON 문자열 -> 변환기 -> Java DTO

- 예시
```json
{
  "name":"Harry",
  "contactNumbers":[
    {
      "type":"Home",
      "number":"123-456-789"
    },
    {
      "type":"Phone",
      "number":"123-4567-3456"
    }
  ],
  "etc":null
}
```

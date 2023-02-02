# Java text blocks
- Java 15부터 가능
- Python 텍스트 사용과 동일
- ```java
	String str = """
	이 안에 문장을 남긴다.
	줄바꿈도 해준다.
	""";
	```

# InputStream, OutputStream
- 스트림의 형태를 바이트단위로 읽기 혹은 쓰기 할 때 사용된다.

# try-with-resources
- 스트림 형태의 데이터 사용시 열고 닫는 과정을 try-with-resources 형태의 문법으로 간결하게 사용 가능하다.
- try-with-resources 형태로 사용하면서 여전히 catch도 사용 가능하다.

# File Upload

## Multipart/from-data
![출처 : https://lng1982.tistory.com/209](/spring/fileupload/images/multipart.png)
- [HTTP Message 복습](https://hsw0905.gitbook.io/note/http/subject_1/message)
- HTTP Message body에 들어가는 데이터 타입을 HTTP Header에 명시하게 되는데, 이 데이터 타입 중 하나이다.
- 클라이언트가 서버에 파일을 업로드 하게 되면, 파일 정보를 HTTP Message body에 담아 보내게 되는데, 한 번에 여러개의 파일을 전송을 하게 되면,
- body 부분에 여러 부분으로 파일이 연결되어 전송된다.
- Content-Type에 multipart/from-data로 지정이 되어 있어야 서버에서 데이터를 처리할 수 있다.
- boundary에 지정된 문자열을 이용하여 전송되는 파일의 구분한다.
- 마지막에 오는 boundary는 -- 로 끝나게 되며, 이는 body의 마지막을 알리는 의미이다.
- 하나의 패킷(기본 1500byte)보다 파일 크기가 크면 여러 개의 패킷으로 쪼개져 네트워크를 통해 서버에 도착한다.

## @ModelAttribute
- 사용자가 Request와 함께 전달하는 값을 Object 형태로 매핑해준다.
- 전달하는 값 -> HTTP body 내용, HTTP parameter 값
- @ModelAttribute를 이용할 DTO 클래스는 요청 파라미터와 매개변수 이름이 서로 같다면 생성자만 구현해도 되고, 아니면 파라미터와 이름이 같은 setter가 있을 때 해당 setter를 참조하여 값을 받을 수도 있다.
	- public으로 선언된 생성자를 찾는다.
		- 없다면 public이 아닌 생성자 중에 매개변수가 제일 적은 생성자를 선택
		- 찾은 생성자가 만약 여러 개라면, 매개변수 갯수가 제일 적은 생성자를 선택
	- 새 객체를 만든다.(생성자의 매개변수 이름 중 클라이언트가 요청한 파라미터 이름과 같은 것이 있다면 해당 매개변수에 요청 받은 파라미터 값을 넣는다.)
	- 클라이언트가 요청한 파라미터들 기준으로 setter 메서드를 찾아 실행시킨다.(위의 생성자를 통해 넣은 값과 상관 없이 실행)

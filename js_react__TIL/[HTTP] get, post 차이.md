# [HTTP] get, post 차이

강의: 블로그 및 도서
생성일: 2022년 2월 12일 오후 11:18
수정일: 2022년 2월 12일 오후 11:38
스킬 & 언어: 기본 지식
중요도: 💜💜💜

***참고***

- [참고 블로그 1](https://hongsii.github.io/2017/08/02/what-is-the-difference-get-and-post/)

---

**HTTP**는 웹상에서 클라이언트와 서버간 요청, 응답으로 데이터를 주고 받을 수 있는 프로토콜이다.

- 클라이언트가 HTTP를 통해 서버에 요청을 보내면 서버는 요청에 맞는 응답을 클라이언트에게 전송한다.
    - 이 때, 요청에 포함되는 메소드(GET, POST)는 서버가 요청을 수행하기 위해 해야할 행동을 표시하는 용으로 사용

**GET**

***서버로부터 정보를 조회하기 위해 설계된 메소드***

요청을 전송할 때 필요한 데이터를 body에 담지않고 쿼리스트링을 통해 전송한다. 

불필요한 요청을 제한하기 위해 요청이 캐시될 수 있다. 

- *데이터 양이 크고 변경될 일이 적은 정적 컨텐츠는 브라우저에서 요청을 캐시해두고 동일한 요청이 발생할 때 서버로 요청을 보내지 않고 캐시된 데이터를 사용하기 때문에 컨텐츠를 변경해도 내용이 바뀌지 않는 경우가 발생된다. 이 때 브라우저의 캐시를 지워주면 다시 컨텐츠를 조회하기 위하여 서버로 요청을 보내게 된다.*

**쿼리스트링 : URL 끝에 `?` 와 함께 이름과 값이 쌍을 이루는 요청 파라미터를 뜻함 → URL에 조회조건을 표시하기 때문에 특정 페이지 링크 및 북마크가 가능하다.*

**만약 요청 파라미터가 여러개라면 &로 연결한다.*

- 요청 파라미더 명이 name1, name2 일 때 value1, value2 의 값으로 서버에 요청을 보내게 된다. `www.example-url.com/resources?name1=value1&name2=value2`

**POST (그나마 보안↑)**

***리소스를 생성, 변경하기 위해 설계된 메소드***

요청을 전송할 때 필요한 데이터를 body에 담아 전송한다. → 길이의 제한 없이 데이터를 전송할 수 있다.

대용량 데이터 전송이 가능

POST 요청 또한 크롬 개발자 도구 등으로 요청 내용을 확인할 수 있기 때문에 민감한 데이터는 암호화 처리를 거쳐야 한다.

***POST로 요청을 보낼 때***

- *요청 헤더의 content-Type에 요청 데이터의 타입을 표시해야 한다. (서버는 내용이나 URL에 포함된 리소스의 확장자 명 등으로 데이터 타입을 유추함. 만약 알 수 없는 경우 `application/octet-stream` 로 처리를 한다.)*

**GET과 POST의 차이**

get은 서버에게 동일한 요청을 여러번 전송했을 때 동일한 응답이 돌아온다. 즉, 설계원칙에 따라 데이터나 상태를 변경시키지 않아야하기 때문에 주로 조회를 할 때 사용해야 한다.

post는 동일한 요청을 여러번 전송했을 때 응답이 다를 수 있다. 이에 따라 서버의 상태나 데이터를 변경시킬 때 사용된다. 즉 post로 요청을 하게 되면 서버는 변경되도록 해야한다. `(etc. 생성:post , 수정:put, 삭제:delete)`
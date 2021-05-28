# Web

## 브라우저

## CORS (Cross-Origin Resource Sharing)

브라우저는 보안을 기본적으로 서로 다른 도메인간의 HTTP 요청을 제한한다. 이 때문에 발생하는 오류가 CORS. 서로 다른 도메인은 다음과 같은 경우를 말한다.

* http://domain1.com → http://domain2.com
* http://localhost:3000 → http://localhost:8080
* http://domain.com → https://domain.com



**해결방법**

* CORS 에러는 서버단에서 해결하는 것이 일반적이다.
  Access-Control-Allow-Origin header에서 도메인을 허용할 수 있다.
* 또는 프론트엔드 프록시 서버를 만들어 해결할 수 있다.



브라우저가 다른 도메인 요청을 할 때는 본요청 전에 `OPTIONS: preflight` 를 통해 사전요청을 보낸다. 이 요청을 통해 도메인이 허용된 도메인인지 감지한다. preflight 은 캐시를 이용해 저장될 수 있다.



## HTTP (Hypertext Transfer Protocol)

인터넷상에서 데이터를 주고받기 위한 통신 규약. 서버/클라이언트 모델 프로토콜로, 클라이언트가 요청하면 서버가 응답을 보내주는 방식을 사용한다. TCP/IP 프로토콜 위에서 작동한다.

**HTTP는 stateless, connectionless 라는 특성을 가진다.**

* 서버와 클라이언트간의 한 번의 요청이 끝나면 그대로 연결을 종료한다 (connectionless)
* 연결이 끊기는 순간 서버와 클라이언트의 통신이 끝나고, 상태정보는 저장되지 않는다 (stateless)
  여기에서 발생하는 문제를 해결하기 위해 **쿠키와 세션**을 사용한다



### HTTP Message 

![Screen Shot 2021-05-29 at 2.44.59 AM](/Users/jeongminlee/Library/Application Support/typora-user-images/Screen Shot 2021-05-29 at 2.44.59 AM.png)

* method: GET, POST, PATCH 등 http verb
* path: 요청하는 자원의 위치를 명시한 URI
* status code / status message: 요청의 성공 여부와 간단한 메세지 
  (2XX-성공, 3XX-redirection, 4XX-클라이언트 에러, 5XX-서버 에러)



## 쿠키와 세션



## REST (Reprasantational State Transfer)

자원을 이름으로 구분해 자원의 상태 정보를 주고받는 것. HTTP URI를 이용해 자원을 명시하고 HTTP Method를 이용해 해당 자원의 CRUD Operation을 정의하는 것

| HTTP Method | Route      | CRUD                    |
| ----------- | ---------- | ----------------------- |
| `GET`       | /users     | 모든 유저 정보 가져오기 |
| `GET`       | /users/:id | 특정 유저 정보 가져오기 |
| `POST`      | /users     | 새로운 유저 생성        |
| `PATCH`     | /users/:id | 특정 유저 정보 수정     |
| `DELETE`    | /users/:id | 특정 유저 정보 삭제     |



### PATCH vs PUT

둘 다 특정한 자원의 정보를 수정한다는 공통점. `PATCH` 는 body에 명시된 부분을 수정하는 요청이고(자원의 부분 교체), `PUT`은 body에 명시된 부분으로 수정하는 요청이다(자원의 전체 교체).



* `{ "name": "jeongmin", "age": 27 }`이라는 객체가 있다고 했을 때
* `PATCH /users/1 { "age": 28 }` 요청을 보내면, 
  객체는 `{ "name": "jeongmin", "age": 28 }` 로 수정된다.
* `PUT /users/1 { "age": 28 }` 요청을 보내면, 
  객체는 `{ "name": null, "age": 28 }` 로 수정된다.


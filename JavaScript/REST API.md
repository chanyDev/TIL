# REST API

REST API란 REST 아키텍쳐의 규칙을 준수하는 API 를 뜻한다.

API(Application Programming Interface) : 일종의 소프트웨어 인터페이스로서, 애플리케이션에서 데이터를 주고 받기 위한 방법을 의미한다.

## REST API 규칙

REST는 두 가지 기본적인 규칙을 가진다.

1. URI는 리소스를 표현하는데 집중한다.
2. 리소스에 대한 행위는 HTTP Method를 통해 정의한다.

```
// bad case
GET /getTodos
GET /todos/get/1

// good case
GET /todos
PATCH /todos/1
DELETE /todos/1
```

## HTTP Method

- GET : 모든, 특정 리소스를 조회

- POST : 리소스를 생성 (페이로드 o)

- PUT : 리소스의 전체를 교체 (페이로드 o)

- PATCH : 리소스의 일부를 교체 (페이로드 o)

- DELETE : 모든, 특정 리소스의 삭제

리소스에 대한 행위의 내용은 페이로드로 전달한다.

PUT, PATCH의 차이점을 살펴보자.

```js
// todos
[
  { "id": 1, "content": "운동하기", "isDone": true },
  { "id": 2, "content": "자바스크립트 공부하기", "isDone": false }
]

// PUT /todos/1
payload : { content: "리액트 복습하기", isDone: false } // { id: 1, content: '리액트 복습하기', isDone: false }

// PATCH /todos/1
payload : { content: "알고리즘 풀기"} // { id: 1, content: '알고리즘 풀기', isDone: true }
```

## Reference

📙 모던 자바스크립트 Deep Dive <br>
https://ko.wikipedia.org/wiki/API

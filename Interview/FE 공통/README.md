# FE 공통

## 1. 브라우저 렌더링 과정에 대해 설명하세요.

www.google.com을 예시로 설명하자면 아래와 같은 과정을 거쳐 화면에 렌더링 된다.

1. 브라우저 주소창에 특정 주소를 입력한다.
2. 루트 요청에 대해 index.html 파일을 서버로부터 응답받는다.
3. index.html을 파싱하면서 DOM Tree를 생성한다.
4. link 태그 또는 style 태그를 만나 css 요청이 발생하면 html 파싱을 중단하고 css를 파싱해 CSSOM Tree를 생성한다.
5. css 파싱이 종료 되면 중단 되었던 html 파싱을 진행하여 DOM Tree를 완성한다.
6. 완성된 DOM Tree와 CSSOM Tree를 결합하여 Render Tree를 생성한다.
7. script 태그를 만나게 되면 JS 파일을 파싱하기 위해 html 파싱을 중단하고 자바스크립트 엔진에 제어권을 넘긴다.
8. 자바스크립트 엔진은 JS 파일을 파싱하여 AST(Abstract Syntax Tree, 추상적 구문 트리)를 생성하고, 실행한다.
9. JS 파싱과 실행이 종료되면 제어권을 브라우저 엔진에게 넘기고 다시 html 파싱을 진행하여 DOM Tree를 생성한다.

### 추가 정리

JS 코드에서 DOM API를 사용하는 경우 이미 DOM 또는 CSSOM이 생성되어 있어야 하는데 script 태그 위치에 따라 HTML 파싱이 블로킹 된 경우 문제가 발생할 수 있다. 이와 같은 문제를 해결하기 위한 방법은 아래와 같다.

- body 태그 가장 아래에 script 태그를 위치시킨다.
- script 태그의 defer 어트리뷰트를 활용한다.

✨ defer 어트리뷰트는 HTML파싱과 외부 JS 파일의 로드가 비동기적으로 진행된다. 단, JS 파싱과 실행은 DOM 생성이 완료된 직후 실행된다.

<br>

## 2. GET과 POST의 차이점에 대해 설명하세요.

1. GET은 특정 리소스를 요청할 때, POST는 서버의 리소스를 생성하거나 업데이트할 때 사용한다.
2. GET은 요청에 body가 존재하지 않고, POST는 body가 존재한다.
3. GET 요청은 멱등성을 가지고, POST 요청은 멱등성을 갖지 않는다.

### 추가 정리

✨ 멱등성 : 동일한 요청을 한 번 보내는 것과 여러번 보내는 것이 같은 효과를 지니고, 서버의 상태도 동일한 경우를 의미한다. POST, PATCH 메서드를 제외한 HTTP 메서드는 멱등성을 갖는다.

<br>

### 3. REST API에 대해 설명하세요.

REST API는 REST 아키텍쳐를 준수하는 API(Application Programing Interpace)를 뜻한다.

REST에서 가장 중요한 규칙 두 가지는 다음과 같다.

1. URI는 리소스를 표현해야 한다.
2. 리소스에 대한 행위는 HTTP 메서드로 표현한다.

```
# bad case
GET /todos/show/1
GET /todos/delete/1

# good case
GET /todos/1
DELETE /todos/1
```

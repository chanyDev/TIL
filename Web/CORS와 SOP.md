# CORS(Cross-Origin Resource Sharing)와 SOP(Same-Origin Policy)

웹 개발을 하면서 API 요청을 하다보면 아래와 같은 CORS 에러를 자주 만나볼 수 있다.

![CORS](https://github.com/chanyDev/TIL/blob/main/img/Web/CORS%20%EC%97%90%EB%9F%AC.PNG?raw=true)

CORS란 정확히 무엇이고 어떻게 CORS 에러를 해결할 수 있는지 정리해보자.

<br>

## 동일 출처 정책(Same-Origin Policy)

> 동일 출처 정책은 어떤 출처에서 불러온 문서나 스크립트가 다른 출처에서 가져온 리소스와 상호작용하는것을 제한하는 것을 제한하는 보안 방식이다.

다시 말해 SOP는 보안상의 이유로 다른 출처로의 리소스 요청을 제한하는 정책이다.

그럼 출처란 무엇일까?

### 출처의 정의

웹 콘텐츠의 출처(origin)는 접근할 때 사용하는 URL의 스킴(프로토콜), 호스트, 포트로 정의된다.

따라서 두 객체의 스킴, 호스트, 포트가 모두 일치하는 경우 같은 출처를 가진다고 말할 수 있다.

스킴(프로토콜) : 컴퓨터 사이에서 데이터의 교환 방식을 정의하는 규칙

호스트 : 네트워크에 연결된 장치에게 부여되는 고유한 이름이며 일반적으로 IP 주소(ex: 172.217.175.35:443) 대신 이해하기 쉬운 호스트 이름(ex: www.google.co.kr)으로 사용된다.

포트 : 네트워크 서비스나 특정 프로세스를 식별하는 논리 단위이며 포트는 포트 번호로 구별된다. 포트 번호는 IP 주소와 함께 쓰여 해당하는 프로토콜에 의해 사용된다. 예를 들어, HTTP 프로토콜의 기본 포트는 80이고 HTTPS 프로토콜의 기본 포트는 443이다.

웹 페이지의 프로토콜, 호스트, 포트를 확인 하는 방법은 간단하다. 개발자 도구를 열고 `location`을 입력하면 Web API인 Location 객체에 접근해 확인할 수 있다.

![Location 객체](https://github.com/chanyDev/TIL/blob/main/img/Web/Location%20%EA%B0%9D%EC%B2%B4.PNG?raw=true)

`https://www.google.co.kr` 의 출처를 기준으로 살펴보자.

- `https://www.google.co.kr/main.html` => 요청 성공(동일 출처)
- `http://www.google.co.kr/main.html` => 요청 실패(프로토콜이 다르다)
- `https://www.naver.com/main.html` => 요청 실패(호스트가 다르다)
- `https://www.google.co.kr:81/main.html` => 요청 실패(포트가 다르다 https는 기본 보트가 443번)

보안상의 이유로 XMLHttpRequest가 동일한 출처로만 요청이 가능하도록 제한되어있었으나 웹 개발에 다른 출처로의 요청이 필요하게 되면서 생긴 방법이 교차 출처 리소스 공유(CORS)다.

<br>

## 교차 출처 리소스 공유(CORS)

> CORS는 추가 HTTP 헤더를 사용하여, 한 출처에서 실행중인 웹 애플리케이션이 다른 출처의 리소스에 접근할 수 있는 권한을 부여하도록 알려주는 체제이다. 웹 애플리케이션은 리소스가 자신의 출처와 다를 때 교차 출처 HTTP 요청을 실행한다.

![CORS](https://github.com/chanyDev/TIL/blob/main/img/Web/CORS.PNG?raw=true)

웹 애플리케이션의 XMLHttpRequest와 Fetch API는 SOP를 따르기 때문에 다른 출처의 리소스를 불러오기 위해서는 올바른 CORS 헤더를 포함한 응답을 반환해야 한다.

CORS 표준은 웹 브라우저에서 해당 정보를 읽는 것이 허용된 출처를 서버에 설명할 수 있는 새로운 HTTP 헤더를 추가함으로써 동작한다.

CORS는 브라우저에 포함되는 정책이기 때문에 브라우저를 통하지 않고 서버 간 통신을 할 때는 이 정책이 적용되지 않는다.

<br>

## CORS의 동작

그럼 어떻게 서로 다른 출처를 가진 리소스를 안전하게 사용할 수 있는지 알아보자.

예를 들어 `https://foo.example` 이라는 출처에서 `https://bar.other` 출처의 리소스에 요청을 보낸다고 가정해보자.

1. 브라우저와 서버간의 통신을 한다.
2. 브라우저는 요청 헤더의 [Origin](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Origin) 필드에 요청을 보내는 출처를 함께 보낸다.
3. 서버는 이에 대한 응답으로 [Access-Control-Allow-Origin](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Access-Control-Allow-Origin) 헤더를 다시 브라우저에 전송한다. 이때 이 필드에는 리소스에 접근 가능한 출처가 담겨있다.<br>
   3-1. 모든 출처에서 접근이 가능하다면 `Access-Control-Allow-Origin: *`를 응답한다.<br>
   3-2. `Access-Control-Allow-Origin: https://foo.example`를 응답한다면 해당 출처에만 리소스에 대한 접근을 허용한다.
4. 브라우저는 자신이 보낸 요청의 `Origin`과 서버가 보낸 응답의 `Access-Control-Allow-Origin`를 비교한 후
   유효한 응답인지 아닌지를 결정한다.

기본적으로 위와 같은 흐름으로 동작한다, 하지만 CORS가 동작하는 방식은 세 가지 시나리오가 존재하고 이에 따라 변경된다.

세 가지 시나리오에 대해 자세히 살펴보자.

<br>

### Preflight Request

프리플라이트 방식은 웹 애플리케이션 개발 시 가장 자주 마주치는 시나리오다.

브라우저는 예비 요청(Preflight Request)을 전송하여 [OPTIONS](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/OPTIONS) 메서드를 통해 다른 도메인의 리소스로 HTTP 요청을 보내 실제 요청을 전송하기에 안전한지 확인한다.

이후 브라우저는 자신이 보낸 예비 요청과 서버가 응답한 허용 정책을 비교한 후 안전하다고 판단되면 같은 엔드포인트로 다시 실제 요청을 보내게 된다.

만약 안전하지 않다면 실제 요청을 보내지 않고 에러를 발생시킨다.

![Preflight](https://github.com/chanyDev/TIL/blob/main/img/Web/preflight.png?raw=true)

Preflight 시나리오는 개발자 도구를 통해 간단하게 재현해 볼 수 있다.

https://www.google.co.kr 페이지에서 블로그의 리소스에 요청을 보내면 `OPTIONS` 메서드를 사용하여 예비 요청을 보내는 것을 확인할 수 있다.

```js
const headers = new Headers({
  'Content-Type': 'application/json',
});

fetch('https://chany-dev.tistory.com/15', { headers });
```

```http
OPTIONS https://chany-dev.tistory.com/15

Accept: */*
Accept-Encoding: gzip, deflate, br
Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
Access-Control-Request-Headers: content-type
Access-Control-Request-Method: GET
Cache-Control: no-cache
Connection: keep-alive
Host: chany-dev.tistory.com
Origin: https://www.google.co.kr
Pragma: no-cache
Referer: https://www.google.co.kr/
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: cross-site
```

실제 브라우저가 보낸 요청을 확인하면, 단순히 `Origin`에 대한 정보뿐 아니라 예비 요청 이후에 보낼 실제 요청에 대한 다른 정보들도 포함되어 있는것을 확인할 수 있다.

예비 요청에서 브라우저는 `Access-Control-Request-Headers`를 사용하여 실제 요청에서 `content-type` 헤더를 사용할 것을 알려주거나, `Access-Control-Request-Method`를 사용하여 `GET` 메서드를 사용할 것을 서버에게 미리 알려준다.

예비 요청에 대한 서버의 응답도 확인해보자.

```http
OPTIONS https://chany-dev.tistory.com/15 200 OK

Access-Control-Allow-Origin: https://chany-dev.tistory.com
Content-Encoding: gzip
Content-Length: 16191
Content-Type: text/html; charset=utf-8
Date: Tue, 22 Feb 2022 09:28:20 GMT
P3P: CP='ALL DSP COR MON LAW OUR LEG DEL'
Vary: Accept-Encoding
X-UA-Compatible: IE=Edge
```

서버의 응답을 확인하면 `Access-Control-Allow-Origin: https://chany-dev.tistory.com` 라는 값을 확인할 수 있다.<br>
이는 리소스에 접근이 가능한 출처는 `https://chany-dev.tistory.com` 뿐이라는 이야기다.

따라서 요청을 보낸 `https://www.google.co.kr` 출처는 허용되지 않은 출처이므로 다음과 같이 에러를 발생시킨다.

![CORS 에러](https://github.com/chanyDev/TIL/blob/main/img/Web/CORS%20%EC%97%90%EB%9F%AC.PNG?raw=true)

### Simple Request

단순 요청(Simple Request)은 실제 요청을 보낸 후, 서버가 응답으로 `Access-Control-Allow-Origin` 값을 보내주면 브라우저가 요청의 `Origin` 값과 비교하여 CORS 정책 위반 여부를 검사한다.

즉, 프리플라이트 방식과 예비 요청의 유무만의 차이가 있다.

하지만 단순 요청은 아래와 같은 조건을 모두 충족해야 사용 가능하다.

1. `GET`, `HEAD`, `POST` 중 하나의 메서드를 사용해야 한다.
2. [Accept](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Accept), [Accept-Language](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Accept-Language), [Content-Language](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Content-Language), [Content-Type](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Content-Type)을 제외한 헤더를 사용하면 안된다.
3. `Content-Type` 헤더를 사용하는 경우 `application/x-www-form-urlencoded`, `multipart/form-data`, `text/plain` 값만 허용한다.
4. 요청에 [ReadableStream](https://developer.mozilla.org/ko/docs/Web/API/ReadableStream) 객체가 사용되지 않아야 한다.

대부분의 HTTP API는 `text/xml` 이나 `application/json`과 같은Content-type을 갖도록 설계되기 때문에
일반적으로 웹 애플리케이션은 위 조건을 모두 충족시키기는 어렵다.

### Credential Request

세번째 시나리오는 인증정보를 포함한 요청을 사용하는 방법이다.<br>
다른 출처 간의 통신에서 보안을 강화하고 싶을 때 사용하는 방법이다.

예를 들어, [fetch](https://developer.mozilla.org/en-US/docs/Web/API/fetch) 메서드를 사용한다면 `credentials` 옵션을 사용해 인증과 관련된 정보를 담을 수 있다.

`credentials` 옵션에는 3가지 값을 사용할 수 있다.

1. omit : 모든 요청에서 인증 정보를 제외한다.
2. same-origin : 동일 출처 간 요청에서 인증 정보를 포함한다.
3. include : 모든 요청에서 인증 정보를 포함한다.

`same-origin` 또는 `inclue` 같은 옵션 값을 사용해서 요청에 인증 정보가 포함된다면 브라우저는 `Access-Control-Allow-Origin` 만 확인하는 것이 아니라 좀 더 빡빡한 검사 조건을 추가하게 된다.

요청에 인증 정보가 담겨 있는 상태에서 다른 출처의 리소스를 요청하게 되면 브라우저는 CORS 정책 여부를 검사하는 룰에 다음 두 가지를 추가한다.

1. `Access-Control-Allow-Origin` 헤더 값에 출처를 지정해야 한다. 즉, `*`이 아닌 명시적인 URL을 지정해야 한다.
2. 응답 헤더에는 `Access-Control-Allow-Credentials: true`를 지정하여 자격 증명으로 실제 요청을 수행할 수 있음을 나타내야 한다.

![credential](https://github.com/chanyDev/TIL/blob/main/img/Web/credential.png?raw=true)

<br>

### 참고 자료

https://developer.mozilla.org/ko/docs/Web/Security/Same-origin_policy<br>
https://developer.mozilla.org/ko/docs/Web/HTTP/CORS<br>
https://im-developer.tistory.com/165<br>
https://evan-moon.github.io/2020/05/21/about-cors/

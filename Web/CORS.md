# CORS(Cross-Origin Resource Sharing)

웹 개발을 하면서 API 요청을 하다보면 아래와 같은 CORS 에러를 자주 만나볼 수 있다.

![CORS](https://github.com/chanyDev/TIL/blob/main/img/Web/CORS.PNG?raw=true)

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

`https://www.google.co.kr` 의 출처를 기준으로 살펴보자.

- `https://www.google.co.kr/main.html` => 요청 성공(동일 출처)
- `http://www.google.co.kr/main.html` => 요청 실패(프로토콜이 다르다)
- `https://www.naver.com/main.html` => 요청 실패(호스트가 다르다)
- `https://www.google.co.kr:81/main.html` => 요청 실패(포트가 다르다 https는 기본 보트가 443번)

보안상의 이유로 XMLHttpRequest가 동일한 출처로만 요청이 가능하도록 제한되어있었으나 웹 개발에 다른 출처로의 요청이 필요하게 되면서 CORS라는 방법이 생겼다.

<br>

### 참고 자료

https://developer.mozilla.org/ko/docs/Web/Security/Same-origin_policy<br>
https://developer.mozilla.org/ko/docs/Web/HTTP/CORS<br>
https://im-developer.tistory.com/165<br>
https://velog.io/@yejinh/CORS-4tk536f0db
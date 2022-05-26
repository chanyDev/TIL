# SPA(Single Page Application)

## SPA의 특징

SPA는 기본적으로 단일 페이지로 구성되며 새로운 페이지에 대한 요청 시 API 서버로부터 갱신에 필요한 데이터만을 전달받아 페이지를 동적으로 갱신한다.

`<a>` 태그를 사용하는 전통적인 웹 방식은 새로운 페이지 요청 시마다 정적 리소스가 다운로드되고 전체 페이지를 다시 렌더링한다.

대표적인 SPA 프레임워크로는 React, Angular, Vue 등이 있다.

## SPA의 장단점

### 장점

- 트래픽의 감소 및 서버 부하 감소
- 뛰어난 사용자 경험(UX)

### 단점

- 초기 구동 속도
- SEO 문제

-> 위와 같은 단점은 React의 경우 Nextjs, Nuxtjs 등의 SSR 라이브러리로 대응 가능하다.

<br>

## SPA와 Routing

애플리케이션의 Routing(라우팅)이란 사용자가 태스크를 수행하기 위해 화면을 전환하는 네비게이션을 관리하기 위한 기능을 의미한다.

history(방문한 웹페이지의 기록)의 관리를 위해서는 각 페이지는 브라우저의 주소창에서 구별할 수 있는 유일한 URL을 소유해야 한다.

-> React에서는 [react-router](https://reactrouter.com/) 를 통해 단일 페이지로 위와 같은 기능을 구현할 수 있다.

### link 방식

link 방식은 `<a>`태그를 클릭하면 href 속성의 값인 페이지의 경로가 URL에 추가되고 해당 페이지의 새로운 HTML 파일을 서버에 요청한다. 따라서 고유의 URL이 존재하기 때문에 SEO 최적화 및 history 관리에 문제가 없다.

link 방식은 아래와 같은 문제점이 존재한다.

1. link 방식은 이전 페이지와 비교해 변경할 필요가 없는 부분까지 포함된 HTML 파일을 서버로부터 매번 다시 전송받기 때문에 불필요한 데이터 통신이 발생한다.

2. HTML 파일을 응답받아 처음부터 다시 렌더링하기 때문에 새로고침이 발생하여 사용성이 저하된다.

3. 서버와의 통신이 동기적으로 동작하기 때문에 HTML을 응답받기 전까지는 다음 태스크가 블로킹된다.

```html
<ul>
  <li><a href="/">Home</a></li>
  <li><a href="login.html">Login</a></li>
</ul>
```

ex) 웹 주소/login

![link 방식](https://github.com/chanyDev/TIL/blob/main/img/JS/link%20%EB%B0%A9%EC%8B%9D.PNG?raw=true)

이미지 출처 : https://poiemaweb.com/js-spa

<br>

### Ajax 방식

Ajax 방식은 비동기로 서버와 브라우저가 데이터를 교환할 수 있는 통신 방식이며, 필요한 리소스만을 서버에 요청하고 변경된 내용만을 갈아끼워 HTML을 완성한다. => Web API인 XMLHttpRequest 객체를 기반으로 동작한다!

따라서 link 방식의 단점인 불필요한 리소스 요청 및 리렌더링을 방지할 수 있어 사용성면에서 뛰어나고 서버와의 통신이 비동기적으로 동작하기 때문에 태스크가 블로킹되지 않는다.

Ajax 방식은 아래와 같은 문제점이 존재한다.

1. URL을 변경시키지 않기 때문에 history 관리가 되지 않는다.
2. 하나의 주소로 동작하기 때문에 새로고침 시 첫 페이지가 다시 로딩되며 SEO 이슈도 존재한다.

![Ajax 방식](https://github.com/chanyDev/TIL/blob/main/img/JS/ajax%20%EB%B0%A9%EC%8B%9D.PNG?raw=true)

이미지 출처 : https://poiemaweb.com/js-spa

### Pjax 방식

Pjax 방식은 History API인 pushState와 popstate 이벤트를 사용한 방식(pushState + Ajax)이다.

pushState 메서드는 URL을 변경하고 history로 추가하지만 서버로 HTTP 요청을 하지는 않는다.

Pjax 방식은 URL을 변경하기 때문에 새로고침 시 요청이 서버로 전달된다. 따라서 서버 렌더링과 ajax 방식을 섞은 방식으로 서버의 자원이 필요하다.

<br>

🎈 **모든 아키텍처에는 trade-off가 존재하기 때문에 애플리케이션의 성향 및 상황을 고려하여 적절한 방법을 선택하자!!**

## Reference

https://poiemaweb.com/js-spa

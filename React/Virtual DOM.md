# Virtual DOM

Virtual DOM은 리액트의 주요 특징으로써, 리액트는 Virtual DOM을 사용해 실제 DOM을 업데이트한다.

Virtual DOM에 대해 살펴보기 전에 DOM과 브라우저의 렌더링 과정에 대해 간략하게 알아보자.

<br>

## DOM(Document Object Model)

브라우저의 렌더링 엔진은 우리가 작성한 웹 문서(HTML, XML ...)를 렌더링하기 위해서 브라우저가 이해할 수 있는 자료 구조(객체)로 메모리에 저장하는데, 이를 DOM이라 한다.

다시말해, DOM은 HTML 문서의 계층적 구조와 정보를 표현하는 트리 자료 구조이며 웹 문서의 동적 변경을 위해 DOM API를 제공한다.

<br>

## 브라우저의 렌더링 과정

아래는 브라우저의 렌더링 과정을 간략하게 나타낸 이미지다.

![브라우저의 렌더링 과정](https://github.com/chanyDev/TIL/blob/main/img/React/Rendering.png?raw=true)

위 이미지의 과정을 정리하면 다음과 같다.

1. 브라우저는 서버로부터 응답받은 HTML 파일을 파싱하여 노드(node)들로 구성된 트리 자료구조인 DOM을 생성한다.
2. CSS를 로드하는 태그를 만나면 CSS를 파싱하여 상속 관계가 반영된 CSSOM을 생성한다.
3. DOM과 CSSOM을 결합하여 Render Tree를 생성한다.
4. Render Tree를 기반으로 레이아웃과 페인트 과정을 거쳐 화면에 렌더링한다.

DOM이나 CSSOM을 변경하는 경우에 브라우저에서는 리플로우, 리페인트가 발생한다.

- 리플로우(Reflow) : 레이아웃을 다시 계산하는 것을 말하며 노드 추가/삭제, 요소의 크기/위치 변경 등 레이아웃에 영향을 주는 변경이 발생한 경우 실행된다.

- 리페인트(Repaint) : 렌더 트리를 기반으로 다시 그리는 것을 말하며 색상 변경 등 레이아웃에 영향이 없는 경우 리플로우 없이 리페인트만 실행된다.

문제는 리플로우, 리페인트 과정은 성능에 악영향을 끼친다는 것이다.

따라서, 이 문제를 해결하기 위해 리액트는 DOM을 최소한으로 조작하기 위한 방식으로 Virtual DOM을 사용한다.

이제 어떻게 Virtual DOM이 동작하는지에 대해서 살펴보자.

<br>

### 참고 자료

https://bitsofco.de/what-exactly-is-the-dom/?utm_source=CSS-Weekly&utm_campaign=Issue-341&utm_medium=email <br>
https://ko.wikipedia.org/wiki/%EB%AC%B8%EC%84%9C_%EA%B0%9D%EC%B2%B4_%EB%AA%A8%EB%8D%B8 <br>
https://ko.reactjs.org/docs/faq-internals.html#gatsby-focus-wrapper <br>
https://ko.reactjs.org/docs/reconciliation.html

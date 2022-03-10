# Virtual DOM

Virtual DOM은 리액트의 주요 특징으로써, 리액트는 Virtual DOM을 사용해 실제 DOM을 업데이트한다.

Virtual DOM에 대해 살펴보기 전에 DOM과 브라우저의 렌더링 과정에 대해 간략하게 알아보자.

<br>

## DOM(Document Object Model)

브라우저의 렌더링 엔진은 우리가 작성한 웹 문서(HTML, XML ...)를 렌더링하기 위해서 브라우저가 이해할 수 있는 자료 구조(객체)로 메모리에 저장하는데, 이를 DOM이라 한다.

DOM은 네 종류의 노드로 구성된다.

- 문서 노드(Document Node) : DOM tree의 최상위에 존재하며, DOM에 접근하기 위한 진입점(entry point)이다.
- 요소 노드(Element Node) : HTML 요소를 표현한다.
- 어트리뷰트 노드(Attribute Node) : HTML 요소의 어트리뷰트를 표현한다.
- 텍스트 노드(Text Node) : HTML 요소의 텍스트를 표현한다.

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

따라서, 이 문제를 해결하기 위해 리액트는 DOM 처리 횟수를 최소화 하고 효율적으로 진행하기 위한 방식으로 Virtual DOM을 사용한다.

이제 어떻게 Virtual DOM이 동작하는지에 대해서 살펴보자.

<br>

## Virtual DOM의 동작

Virtual DOM은 실제 DOM에 접근하여 조작하는 대신 이를 추상화한 객체를 구성하여 사용한다.

리액트에서 데이터가 변경되어 브라우저에 실제 DOM을 업데이트 할 때는 다음 절차를 밟는다.

1. 데이터를 업데이트하면 전체 UI를 Virtual DOM에 리렌더링한다.
2. 이전 Virtual DOM에 있던 내용과 현재 내용을 비교한다.
3. 바뀐 부분만 실제 DOM에 적용한다.

이미지로 표현하면 아래와 같다.

![Virtual DOM](https://github.com/chanyDev/TIL/blob/main/img/React/Virtual%20DOM.png?raw=true)

리액트에서는 이 과정을 재조정(Reconciliation)이라 하며, 비교 알고리즘(Diffing Algorithm)을 통해 빠르고 효율적으로 재조정을 진행한다.

<br>

## 비교 알고리즘(Diffing Algorithm)

비교 알고리즘은 다음 두 가지 가정을 기반으로 구현되어 있다.

1. 서로 다른 타입의 두 엘리먼트는 서로 다른 트리를 만들어낸다.
2. 개발자가 key prop을 통해 여러 렌더링 사이에서 어떤 자식 엘리먼트가 변경되지 않아야 할 지 표시해 줄 수 있다.

위 가정을 전제로 어떻게 비교 알고리즘을 통해 재조정이 진행되는지 알아보자.

### 루트 엘리먼트의 타입이 다른 경우

두 루트 엘리먼트의 타입이 다르면, 리액트는 이전 트리를 버리고 완전히 새로운 트리를 구축한다.

```html
<!-- before -->
<div id="root">
  <Component />
</div>

<!-- after -->
<span id="root">
  <Component />
</span>
```

위 예시처럼 루트 엘리먼트가 다른 타입으로 변경된다면 `<Component>` 는 사라지고 새로 마운트된다.

### DOM 엘리먼트의 타입이 같은 경우

같은 타입의 엘리먼트를 비교할 때 두 엘리먼트의 어트리뷰트를 확인하여 동일한 내역은 유지하고 변경된 어트리뷰트만 갱신한다.

```html
<!-- before -->
<div className="before" title="test" style={{ color: 'blue', fontSize: 10 }} />

<!-- after -->
<div className="after" title="test" style={{ color: 'red', fontSize: 10 }} />
```

위 예시의 경우 className 어트리뷰트와 style 어트리뷰트의 color 속성만 변경한다.

### 자식 요소에 대한 재귀처리

DOM 노드의 자식 요소들을 재귀 처리할 때, 리액트는 기본적으로 두 리스트를 순회하고 차이점이 있으면 변경을 생성한다.

```html
<!-- before -->
<ul>
  <li>Dongchan</li>
  <li>Jungjin</li>
</ul>

<!-- after -->
<ul>
  <li>Hyunwoo</li>
  <li>Dongchan</li>
  <li>Jungjin</li>
</ul>
```

위 예시와 같이 리스트의 맨 뒤가 아닌 위치에 엘리먼트가 추가되는 경우 리액트는 모든 자식 요소들을 변경한다. 이는 성능적으로 문제가 있다.

`<li>Dongchan</li>` 과 `<li>Jungjin</li>`은 변경되지 않았지만, 리스트 상 순서의 위치가 변경되었기 때문에 순회하며 비교할 때 다르다고 판단하여 모든 리스트를 변경한다.

이런 문제를 해결하기 위해 리액트는 key 어트리뷰트를 지원한다.

리액트는 아래 예시와 같이 자식 요소들이 key를 가지고 있다면, key를 통해 기존 트리와 이후 트리의 자식 요소들이 일치하는지 확인함으로서 효율적인 처리를 구현한다.

```html
<!-- before -->
<ul>
  <li key="one">Dongchan</li>
  <li key="two">Jungjin</li>
</ul>

<!-- after -->
<ul>
  <li key="zero">Hyunwoo</li>
  <li key="one">Dongchan</li>
  <li key="two">Jungjin</li>
</ul>
```

이제 리액트는 key를 통해 기존 엘리먼트를 이동만 하면 된다는 것을 알 수 있다.

<br>

✔ Virtual DOM을 사용한다고 해서 무조건 빠른 것은 아니다. 작업이 간단한(단순한 라우팅 정도만 있는 정적인 페이지)경우는 오히려 리액트를 사용하지 않는 편이 더 나은 성능을 보이기도 한다. 리액트에서 항상 제공할 수 있는 것은 업데이트 처리의 간결성이다.

<br>

### 참고 자료

📙 리액트를 다루는 기술 <br>
https://ko.wikipedia.org/wiki/%EB%AC%B8%EC%84%9C_%EA%B0%9D%EC%B2%B4_%EB%AA%A8%EB%8D%B8 <br>
https://bitsofco.de/what-exactly-is-the-dom/?utm_source=CSS-Weekly&utm_campaign=Issue-341&utm_medium=email <br>
https://ko.reactjs.org/docs/faq-internals.html#gatsby-focus-wrapper <br>
https://ko.reactjs.org/docs/reconciliation.html

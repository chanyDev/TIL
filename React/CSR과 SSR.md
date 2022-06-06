# CSR과 SSR

TTV(Time To View) : 유저가 웹 사이트를 볼 수 있는 시점

TTI(Time To Interact) : 유저가 웹 사이트와 상호작용을 할 수 있는 시점

SSG(Static Site Generation) : HTML 파일을 빌드 타임에 생성하고 각 요청에 따라 재사용

Pre-Rendering : 서버에서 클라이언트에게 응답하기 전에 미리 어느정도 완성된 HTML 파일을 만들는 행위

Hydration : Pre-Rendering 과정을 마치고 응답받은 HTML 파일에 남은 JS 코드들을 실행하는 동작

## CSR(Client-Side-Rendering)

CSR은 애플리케이션에 필요한 모든 리소스를 최초에 한번 다운로드하고 새로운 페이지 요청 시 필요한 부분만 갱신한다.

서버는 오로지 api 서버의 역할만 수행하게 된다.

클라이언트의 성능이 좋아지고 SPA(Single Page Application)가 대두됨에 따라 CSR 방식이 각광받기 시작했다.

### 동작원리

1. 브라우저의 요청에 따라 서버는 **비어있는 HTML 파일을** 응답으로 전송한다.
2. 브라우저는 HTML 파일에 연결된 JS 파일을 다운받는다.
3. 브라우저는 JS 로드가 완료되면 React 어플리케이션을 실행한다.
4. React 어플리케이션이 정상 실행된 후 화면이 렌더링되어 유저가 볼 수 있고 인터렉션이 가능해진다. -> TTV와 TTI가 동시에 이루어진다

![CSR](https://github.com/chanyDev/TIL/blob/main/img/React/CSR.png?raw=true)

### 장점

- 빠른 페이지 전환
- 트래픽 감소, 속도, 반응성의 향상 등에 따른 사용자 경험(UX)의 향상

### 단점

- 구글 검색엔진을 제외한 SEO(검색엔진 최적화) 문제 -> 비어있는 HTML 파일을 받기 때문에 크롤러가 데이터를 수집할 수 없음
- 초기 로딩 속도가 느림 -> 코드 스플리팅(code spliting)으로 해결 가능

</br>

## SSR(Server-Side_Rendering)

SSR은 페이지를 이동할 때마다 새로운 페이지에 대한 리소스를 서버에 요청하고 응답받는다.

### 동작원리

1. 브라우저의 요청에 따라 서버는 **렌더링할 준비가 된 HTML 파일을** 응답으로 전송한다.
2. 브라우저는 HTML 파일을 렌더링하여 유저가 볼 수 있게 되고 JS 파일을 다운받는다. -> TTV
3. 브라우저는 JS 로드가 완료되면 React 어플리케이션을 실행한다.
4. React 어플리케이션이 정상 실행된 후 유저가 인터렉션이 가능해진다. -> TTI

![SSR](https://github.com/chanyDev/TIL/blob/main/img/React/SSR.png?raw=true)

### 장점

- 초기 로딩 속도가 빠름
- 모든 검색엔진에 대해 SEO(검색엔진 최적화) 가능

### 단점

- 잦은 서버 요청에 따른 트래픽, 서버 부하 증가
- 페이지의 리로딩에 따른 사용자 경험(UX) 저하
- 서비스의 동적 활용에 딜레이가 있다 -> TTV와 TTI가 동시에 이루어지지 않기 때문에 화면은 보이지만 JS파일이 로드되기 전까지는 아무런 인터렉션이 발생하지 않는다

# Next.js 시작하기

Next.js는 리액트를 위한 프레임워크다. 애플리케이션 개발 및 생산 단계를 위한 다양한 기능을 제공한다.

## Setup

보일러 플레이트 설치

```
npx create-next-app
```

`--typescript` 플래그를 사용하면 타입스크립트 프로젝트로 시작할 수 있다.

```
npx create-next-app --typescript
```

수동 설치

```
npm install next react react-dom
```

`package.json` 에 스크립트 추가

```json
"scripts": {
  "dev": "next dev",
  "build": "next build",
  "start": "next start",
  "lint": "next lint"
}
```

## SSR(Server Side Rendering)

Next.js는 서버 사이드 렌더링을 내장하고 있다. 서버 사이드 렌더링에 대한 내용은 전에 작성한 [포스트](https://github.com/chanyDev/TIL/blob/main/Web/%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8%20%EC%82%AC%EC%9D%B4%EB%93%9C%20%EB%A0%8C%EB%8D%94%EB%A7%81%EA%B3%BC%20%EC%84%9C%EB%B2%84%20%EC%82%AC%EC%9D%B4%EB%93%9C%20%EB%A0%8C%EB%8D%94%EB%A7%81.md)를 확인하자.

물론 리액트 에서도 [서버 사이드 렌더링](https://ko.reactjs.org/docs/react-dom-server.html#gatsby-focus-wrapper)을 구현할 수 있는 기능이 제공되지만 추가 설정이 필요하기 때문에 까다롭다.

서버 사이드 렌더링을 활용하면 초기 로딩 속도 향상, SEO 최적화 등의 이점을 가질 수 있다.

Next.js를 활용하면 초기 로딩에는 서버 사이드 렌더링을 사용하고 경로 탐색, 사용자 인터렉션 등은 클라이언트 사이드 렌더링을 사용하는 등 두 가지 방식을 혼합해 사용할 수 있다.

클라이언트 라우터 전환은 `Link` API를 사용해 처리할 수 있다.

아래는 간단한 예시다.

```tsx
import Link from 'next/link';

// pages/index.tsx
<Link href='/'>
  <a>Home</a>
</Link>;

// pages/about.tsx
<Link href='/about'>
  <a>About</a>
</Link>;

// pages/blog/[id].tsx
<Link href='/blog/1'>
  <a>Post</a>
</Link>;
```

## Routing

리액트는 기본적으로 라우팅을 지원하지 않는다. 따라서 [react-router](https://reactrouter.com/)와 같은 라이브러리를 사용하여 라우팅을 위한 코드를 작성해야 한다.

Next.js는 기본적으로 파일 기반 라우팅을 지원한다. 따라서 라우팅을 위한 추가 코드를 작성할 필요가 없다.

기본적인 사용법은 다음과 같다.

`pages` 디렉토리에 파일을 추가하면 자동으로 라우터로 사용할 수 있다.

- `pages/index.tsx` => `/`
- `pages/blog/post.tsx` => `/blog/post`

동적 경로는 대괄호 표기법을 사용하여 생성할 수 있다.

- `pages/post/[id].tsx`

파라미터는 `useRouter` Hook을 사용해 router 객체에 접근할 수 있다.

```tsx
import { useRouter } from 'next/router';

const router = useRouter();
const { id } = router.query;
```

## API Routes

Next.js는 Node.js를 이용해 리액트 프로젝트에 백엔드 API를 추가할 수 있는 기능을 제공한다. 즉, 별도의 백엔드 프로젝트 없이 데이터 베이스에 접근하거나 인증을 추가하는 등 하나의 프로젝트로 풀스택 개발을 가능하게 한다.

이를 위해 Next.js는 `api` 디렉토리를 지원한다. `pages/api` 디렉토리 내의 모든 파일은 `/api/*` 경로로 매핑되며 클라이언트의 번들 크기를 늘리지 않는다.

아래는 `pages/api/user.ts` 경로에 대한 간단한 예시다.

```ts
export default const handler = (req, res) => {
 res.status(200).json({name: 'dongchan'})
}
```

✨ 이번에는 간단한 설정 및 Next.js에서 지원하는 핵심 기능들에 대해서 간단히 살펴봤다. 보다 자세한 내용은 추가로 정리하도록 하자.

## 참고 자료

https://nextjs.org/docs/getting-started

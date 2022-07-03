# Next.js 시작하기

Next.js는 리액트를 위한 프레임워크다. 애플리케이션 개발 및 생산 단계를 위한 다양한 기능을 제공한다.

이번에는 간단한 설정 및 기본적인 기능에 대해서 살펴보자.

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

## Routing

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

클라이언트 라우터 전환은 `Link` API를 사용해 처리할 수 있다.

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

## 참고 자료

https://nextjs.org/docs/getting-started

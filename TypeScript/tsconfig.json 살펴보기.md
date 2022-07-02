# tsconfig.json 살펴보기

## tsc

타입스크립트를 지역(`-g` 옵션을 지정하지 않고 설치)으로 설치 했다는 전제하에 설명하겠다.

tsc 명령어를 통해 타입스크립트를 자바스크립트로 컴파일 할 수 있다. 아래 명령어를 입력하면 타입스크립트 컴파일러가 `index.ts` 파일을 컴파일하여 동일한 경로에 `index.js` 파일을 생성한다.

```
npx tsc index.ts
```

tsc 명령어에는 많은 컴파일 옵션들이 존재한다. `tsc --help` 명령어를 통해 확인할 수 있다.

매번 명령어를 통해 옵션을 지정하기 힘들기 때문에 `tsconfig.json` 파일을 통해 설정을 관리할 수 있다.

## tsconfig.json

디렉토리에 tsconfig.json 파일이 존재한다면 해당 디렉토리가 타입스크립트의 프로젝트의 루트가 된다.

tsconfig.json 파일은 프로젝트를 컴파일하는 데 필요한 루트 파일과 컴파일 옵션을 설정할 수 있다.

다음 명령어를 통해 기본적인 형태의 tsconfig.json 파일을 생성할 수 있다.

```
npx tsc --init
```

tsconfig에는 다양한 옵션들이 존재하지만 주로 사용되는 옵션에 대해서만 알아보도록 한다.

더 많은 옵션과 자세한 내용은 [tsconfig 공식문서](https://www.typescriptlang.org/tsconfig)에서 살펴보자.

### 루트 옵션

tsconfig의 루트 옵션은 타입스크립트 또는 자바스크립트 프로젝트가 설정되는 방식과 연관이 있다.

#### include

include 옵션은 컴파일할 대상 파일 이름 또는 패턴의 배열을 지정한다. `tsconfig.json` 파일이 위치한 디렉토리를 기준으로 확인한다.

기본값 : `**`

아래 살펴볼 exclude 옵션과 include 옵션은 glob 패턴을 위한 와일드카드를 지원한다.

와일드 카드란?

- `*` : 해당 디렉토리의 모든 파일 검색(0개 이상의 문자와 일치)
- `?` : 임의의 하나의 문자와 일치
- `**` : 하위 디렉토리를 재귀적으로 접근(하위 디렉토리의 하위 디렉토리가 존재하는 경우 반복해서 접근)

패턴에 파일 확장자가 포함되어있지 않다면 `.ts`, `.tsx`, `.d.ts` 확장자만을 포함한다. 아래 살펴볼 allowJs 옵션을 true로 설정한다면 `.js`, `.jsx` 확장자도 포함된다.

아래 예시는 src 디렉토리에 포함된 모든 디렉토리의 모든 파일을 포함한다.

```json
{
  "include": ["src/**/*"]
}
```

#### exclude

exclude 옵션은 컴파일에 제외할 대상 파일 이름 또는 패턴의 배열을 지정한다.

기본값 : `node_modules`, `bower_components`, `jspm_packages`, outDir 옵션에 지정한 경로

주의할 점은 include 옵션에 지정하지 않은 파일은 적용되지 않는다.

아래 예시는 node_modules 디렉토리를 제외한다. 단, include 옵션에 지정되어있지 않다면 무시된다.

```json
{
  "exclude": ["node_modules"]
}
```

### 컴파일러 옵션

컴파일러 옵션은 타입스크립트 구성의 대부분을 구성하며 언어가 작동하는 방식을 다룬다.

#### target

tsc가 코드를 컴파일 했을 때 생성되는 자바스크립트의 버전을 지정한다. 최신 브라우저는 모든 `ES6` 기능을 지원하므로 기본적으로는 `ES6`로 지정해서 사용하면 좋을 듯 하다.

기본값 : `es3`

#### lib

컴파일에 포함될 라이브러리 목록을 지정한다.

주의할 점은 컴파일 단계에서 이런 기능을 이용할 수 있다는 사실을 타입스크립트에 알릴 뿐 실제로 기능을 구현하는 것은 아니다. 따라서 실행 환경에 따라 폴리필을 제공해야 한다. [core-js](https://github.com/zloirock/core-js#readme)같은 라이브러리를 사용해 필요한 기능을 설치할 수 있다.

기본값 : target 옵션에 지정한 버전의 기본 라이브러리가 지정된다.

아래 예시처럼 target 옵션을 `ES6`로 지정했다면 lib 옵션을 생략할 경우 `DOM`, `DOM.Iterable`, `ES6`, `ScriptHost` 가 기본 라이브러리로 지정된다.

```json
{
  "compilerOptions": {
    "target": "ES6"
  }
}
```

예를 들어 target을 `ES5`로 지정했는데 lib을 별도로 지정하지 않고 Promise를 사용할 경우 에러가 발생한다.

![tsconfig lib error](https://github.com/chanyDev/TIL/blob/main/img/TS/tsconfig%20lib%20%EC%97%90%EB%9F%AC.png?raw=true)

#### jsx

JSX 코드를 어떻게 컴파일할지 지정한다. (`react`, `react-jsx`, `preserve` 등...)

cra로 생성한 프로젝트의 tsconfig의 경우 react-jsx로 지정한다. react-jsx는 리액트 v17.0 이후로 지원하는 변환 방식으로 공식 문서에서는 몇 가지 이점을 제공한다고 설명한다. react-jsx 설정에 대한 자세한 내용은 [React 공식문서](https://reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html)를 참고하자.

React 사용 시 기본적으로 react-jsx 설정을 사용하면 좋을듯 하다.

#### module

tsc가 코드를 컴파일할 대상 모듈 시스템을 지정한다. (`CommonJS`, `ES6`, `ESNext` 등...)

#### rootDir

모듈을 컴파일한 후 어떤 디렉토리를 루트로 현재 구조를 유지할지 지정한다.

따로 지정하지 않는다면 타입스크립트는 자동으로 해당 모듈의 엔트리 포인트가 되는 파일을 찾고 해당 파일이 위치한 디렉토리를 루트로 지정한다.

아래와 같은 구조를 갖는 프로젝트가 있다고 해보자.

```
Project
├─ src
│   ├─ index.ts
│   └─ utils
|       └─ math.ts
└─ tsconfig.json
```

rootDir 옵션을 지정하지 않은 경우 엔트리 포인트인 `src/index.ts`를 기준으로 `src`를 루트 디렉토리로 인식하고 아래와 같은 출력 결과물을 생성한다.

```json
{
  "compileOptions": {
    "outDir": "./dist"
  }
}
```

```
dist
├─ index.ts
└─ utils
    └─ math.ts
```

rootDir을 아래와 같이 지정한 경우 `Project` 을 기준으로 출력 결과물을 생성한다.

```json
{
  "compileOptions": {
    "outDir": "./dist",
    "rootDir": "."
  }
}
```

```
dist
└─ src
    ├─ index.ts
    └─ utils
        └─ math.ts
```

주의할 점은 rootDir 옵션을 사용한다면 모든 컴파일 대상은 해당 디렉토리 밑에 위치해야 한다.

## 참고 자료

https://www.typescriptlang.org/tsconfig <br>
https://typescript-kr.github.io/ <br>
https://joshua1988.github.io/ts/config/tsconfig.html#%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%84%A4%EC%A0%95-%ED%8C%8C%EC%9D%BC-%EC%86%8D%EC%84%B1

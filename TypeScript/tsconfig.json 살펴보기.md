# tsconfig.json 살펴보기

## tsc

tsc 명령어를 통해 타입스크립트를 자바스크립트로 컴파일 할 수 있다. 아래 명령어를 입력하면 타입스크립트 컴파일러가 `index.ts` 파일을 컴파일하여 동일한 경로에 `index.js` 파일을 생성한다.

```
tsc index.ts
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

기본값 : `**`

include 옵션은 컴파일할 대상 파일 이름 또는 패턴의 배열을 지정한다. `tsconfig.json` 파일이 위치한 디렉토리를 기준으로 확인한다.

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

기본값 : `node_modules`, `bower_components`, `jspm_packages`, outDir 옵션에 지정한 경로

exclude 옵션은 컴파일에 제외할 대상 파일 이름 또는 패턴의 배열을 지정한다.

주의할 점은 include 옵션에 지정하지 않은 파일은 적용되지 않는다.

아래 예시는 node_modules 디렉토리를 제외한다. 단, include 옵션에 지정되어있지 않다면 무시된다.

```json
{
  "exclude": ["node_modules"]
}
```

### 컴파일러 옵션

## 참고 자료

https://www.typescriptlang.org/tsconfig <br>
https://typescript-kr.github.io/ <br>
https://velog.io/@sooran/tsconfig.json-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%95%8C%EA%B3%A0-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0

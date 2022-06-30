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

## 참고 자료

https://www.typescriptlang.org/tsconfig <br>
https://typescript-kr.github.io/ <br>
https://velog.io/@sooran/tsconfig.json-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%95%8C%EA%B3%A0-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0

# TypeScript Setup

### 1. 파일 생성 후 `package.json` 생성

```
//typescript-tutorial
npm init
```

### 2. typescript 설치

typescript는 개발할 때만 사용하므로 dev dependency로 설치한다

```
//typescript-tutorial
npm i --save-dev typescript
```

잘 설치 되었다면 `package.json`의 devDependencies에 typescript가 추가 됐을 것이다.

### 3. `tsconfig.json` 생성

typescript를 사용할 때 필요한 설정 파일 만들기

여기에는 타입스크립트 컴파일러가 포함된다

```
//typescript-tutorial
npx tsc --init
```

잘 설치 되었다면 루트 경로에 `tsconfig.json`이 생성되었을 것이다.

### 4. 컴파일러 설정

`package.json`에서 타입스크립트를 빌드 시 컴파일러로 설정한다.

```json
  "scripts": {
    "build": "tsc"
  },
```

이제 실행할 때 TS 코드를 JS 코드로 바꿔줄 것이다.

### 5. 빌드 해보기

이제 간단한 TS 코드를 작성해보자

루트 경로에 `main.ts` 파일을 생성한 후 아래 코드를 작성해 보자

```js
// main.ts
console.log("Hello Typescript!");
```

이제 빌드해보자

```
npm run build
```

루트 경로 `main.ts` 위에 `main.js`파일이 생성된 걸 확인할 수 있을 것이다.

```js
// main.js
"use strict";
console.log("Hello Typescript!");
```

`use strict`는 JS를 strict 모드로 실행할 때 사용한다.

> [strict mode | mdn web docs](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Strict_mode)

### npm start 설정

`package.json`에 아래 설정을 추가하면 앞으로 `npm start` 입력 시 원하는 파일을 실행 가능하다

```json
  "scripts": {
    "build": "tsc",
    "start": "node <파일명>"
  },
```

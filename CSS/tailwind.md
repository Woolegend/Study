# Tailwind CSS

## 설치

### 1. React 프로젝트 생성

```
npm create-react-app my-project
cd my-project
```

### 2. Tailwind CSS 설치

npm을 통해 `tailwindcss`를 설치할 한 후, `tailwind.config.js`를 생성하자

```
npm install -d tailwindcss
npx tailwindcss init
```

### 3. 템플릿 경로 설정

`tailwind.config.js `에 모든 템플릿 코드의 경로를 추가하자

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

### 4. CSS에 Tailwind 지시문 추가

각 레이어에 대한`@tailwind` 지시문을 `./src/index.css`에 추가해보자

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

VSCode에서는 다음과 같은 경고가 발생할 수 있다.

```css
@tailwind base; /*  unknown Rule @tailwind */
@tailwind components; /* unknown Rule @tailwind */
@tailwind utilities; /* unknown Rule @tailwind */
```

여러가지 [해결 방법](https://byby.dev/at-rule-tailwind)들 중, 필자가 사용한 방식은 아래와 같다.

```css
@import "tailwindcss/base";
@import "tailwindcss/components";
@import "tailwindcss/utilities";
```

### 빌드 프로세스 시작

```
npm run start
```

### Tailwind 적용하기

스타일이 적용되지 않는다면 `index.css`이 제대로 import 되어 있는지 확인하자

```js
export default function App() {
  return (
    <div>
      <h1 className="text-3xl font-bold underline">Hello world!</h1>
    </div>
  );
}
```

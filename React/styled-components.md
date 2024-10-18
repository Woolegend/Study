> **last edit 24.10.16**

# Styled Components

Styled Components는 컴포넌트를 만들면서 바로 스타일을 작성한다. 마친 React 컴포넌트를 만드는 것처럼 React스럽게 CSS를 사용하는 방식이다. 컴포넌트를 중심으로 스타일을 지정하는 방식은 편리할 뿐만 아니라, 개발 속도 또한 향상된다.

**설치**

```
npm install styled-components

npm i styled-components
```

```js
// App.js
import styled from "styled-components";

const Button = styled.button`
  background-color: #ededed;
  border: none;
  border-radius: 8px;
`;

function App() {
  return (
    <div>
      <h1>안녕 Styled Components!</h1>
      <Button>확인</Button>
    </div>
  );
}

export default App;
```

**※목차**

- [Styled Components](#styled-components)
  - [1. CSS의 문제점](#1-css의-문제점)
    - [1.1. 컴포넌트 기반 아키텍처와의 불일치](#11-컴포넌트-기반-아키텍처와의-불일치)
    - [1.2. 스타일 재활용의 어려움](#12-스타일-재활용의-어려움)
    - [1.3. 동적 스타일의 한계](#13-동적-스타일의-한계)
    - [1.4. 불필요한 스타일 로딩](#14-불필요한-스타일-로딩)
  - [2 Styled Components 만들기](#2-styled-components-만들기)
  - [3 네스팅(Nesting)](#3-네스팅nesting)
    - [3.1. `&` 선택자](#31--선택자)
    - [3.2. 스타일 컴포넌트 선택자](#32-스타일-컴포넌트-선택자)
  - [4. 다이나믹 스타일링](#4-다이나믹-스타일링)
    - [4.1. 변수 사용하기](#41-변수-사용하기)
    - [4.2. 함수 사용하기](#42-함수-사용하기)
    - [4.3. 논리 연산자 사용하기](#43-논리-연산자-사용하기)
  - [5. 스타일 재사용](#5-스타일-재사용)
    - [5.1. 스타일 상속](#51-스타일-상속)
    - [5.2. JSX 컴포넌트 스타일 적용](#52-jsx-컴포넌트-스타일-적용)
    - [5.3. CSS 함수](#53-css-함수)
  - [6. 글로벌 스타일](#6-글로벌-스타일)
  - [7. 애니메이션 `keyframes`](#7-애니메이션-keyframes)
  - [8. 테마 `ThemeProvider`](#8-테마-themeprovider)

## 1. CSS의 문제점

React는 컴포넌트 기반 아키텍처를 사용하지만, 전통적인 CSS는 전역 스코프를 가진다. 이로 인해 다음과 같은 문제가 발생할 수 있다.

- **스타일 충돌**:  
  <small>여러 컴포넌트에서 동일한 클래스 이름을 사용할 경우 의도치 않은 스타일 오버라이딩이 발생할 수 있다.</small>

- **스타일 재활용의 어려움**:  
  <small>CSS만으로는 재사용 가능한 스타일 컴포넌트를 만들기 어렵다.</small>

- **동적 스타일링의 한계**:  
  <small>전통적인 CSS는 props나 state에 따라 스타일을 동적으로 변경하기 어렵다</small>
- **불필요한 스타일 로딩**:  
  <small>전체 CSS 파일을 로드하면 실제 사용하지 않는 스타일도 함께 로드되어 성능에 영향을 줄 수 있다.</small>

### 1.1. 컴포넌트 기반 아키텍처와의 불일치

```js
// App.js
import Dashboard from './Dashboard';
import App.css;

function App() {
   return (
     <div className="container">
       <Dashboard> ... </Dashboard>
     </div>
   );
}

export default App
```

```css
/* App.css */
.container {
  background-color: #eeeeee;
}
```

```js
// Dashboard.js
import Dashboard.css;

function Dashboard({ children }) {
   return (
     <div className="container">
       {children}
     </div>
   );
}

export default Dashboard;

```

```css
/* Dashboard.css*/
.container {
  font-size: 20px;
}
```

`App` 컴포넌트와 `Dashboard` 컴포넌트 모두 `container`라는 클래스 이름이 존재한다. `App` 컴포넌트에는 `App.css`만 적용되고, `Dashboard`에는 `Dashboard.css`만 적용이 된다면 상관 없지만, 실제로는 두 컴포넌트에 두 스타일이 모두 적용된다.

그럼 class를 작명할 때 겹치지 않으면 문제 없는거 아니야?

물론 맞는 말이다. 프로젝트의 규모가 작아 작명할 class가 비교적 적다면 충분히 가능할 것이다. 하지만 프로젝트의 규모가 커지면 굉장히 힘들 것이다. 예를 들어, 페이스북 프로젝트에는 약 5만여 개가 넘는 컴포넌트를 사용한다고 한다. 이 프로젝트에서 **클래스 이름이 겹치지 않게 짓는 것은 사실상 불가능에 가깝다**.

```js
const StyledApp = styled.div`
  background-color: #000000;
`;

const Dashboard = styled.div`
  font-size: 16px;
`;

function App() {
  return (
    <StyledApp가가
      <Dashboard> ... </Dashboard>
    </StyledApp>
  );
}
```

Styled Components는 컴포넌트 별로 스타일을 격리하며 이를 해결했다. Styled Components를 사용하면 개발자가 코드를 작성할 때 클랙스를 작명할 필요가 없다. 개발이 끝나면 Styled Components가 각 디자인에 고유한 클래스 이름을 부여한다.

### 1.2. 스타일 재활용의 어려움

```js
// App.js
import Dashboard from './Dashboard';
import Card from './Card';
import App.css;

function App() {
   return (
     <div className="container">
       <Dashboard>
         <Card> ... </Card>
       </Dashboard>
     </div>
   );
}

export default App
```

```css
/*App.css*/
.shadow20 {
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.2);
}

.shadow40 {
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.4);
}
```

```js
// Card.js
import Card.css;

function Card({ children }) {
  return (
    <div className="container shadow20">
    </div>
  );
}

export default Card;
```

`App` 컴포넌트 안에 `Dashboard` 를 배치하고, 그 안에 `Card` 컴포넌트를 되어있다.
`.shadow20`이라는 클래스를 만들어 두고 이걸 `Card` 컴포넌트에 적용했다.

여기서 문제는, `App.css` 에 정의된 `shadow20`만 보고 이게 어디에서 사용되는 스타일인지 알기 어렵다. JS와 달리 CSS 코드는 VSCode 같은 코드 에디터에서 추적하기 어렵기 때문에 직접 찾아봐야 한다. 또한 코드가 늘어날수록 스타일의 유지 보수 및 재사용이 힘들어진다.

```js
const shadow20 = css`
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.2);
`;

const shadow40 = css`
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.4);
`;

const Card = styled.div`
  ${shadow20}
  ...
`;
```

Styled Components는 스타일 재사용이 필요한 상황에서 클래스를 대신해 JS 변수로 만든다. 변수로 스타일을 관리하게 되면 에디터를 통해 어디에 사용하는 스타일인지 쉽게 확인 가능하고, 수정 혹은 삭제도 편리하다.

### 1.3. 동적 스타일의 한계

CSS 파일은 JS와 분리 되어 있어, 컴포넌트의 상태 변화를 CSS에 직접 반영하기 어렵다. JS의 로직을 활용해 CSS로 조건부 스타일링이 가능하긴 하지만, 동적인 React에는 정적인 CSS가 적합하지 않다.

```js
// Button.js
import "./Button.css";

const Button = ({ primary, size, label }) => {
  let className = "button";
  if (primary) className += " button-primary";
  if (size === "large") className += " button-large";
  if (size === "small") className += " button-small";

  return <button className={className}>{label}</button>;
};

export default Button;
```

```css
/* Button.css */
.button {
  padding: 10px 15px;
}

.button-primary {
  background-color: blue;
  color: white;
}

.button-large {
  font-size: 18px;
  padding: 15px 25px;
}

.button-small {
  font-size: 12px;
  padding: 5px 10px;
}
```

위 코드를 살펴보면 몇가지 불편한 점을 찾을 수 있다.

가장 먼저 눈에 뛰는 단점은 클래스의 이름이 너무 복잡하다. 현재는 하나의 버튼의 상태만 표현하면 되기에 큰 문제가 없을 수 있지만 버튼의 종류와 상태가 늘어날수록 이 문제는 더욱 부각된다.

또 다른 단점은 동적인 값을 다룰 수 없다. 만약 더 작은 버튼을 스타일링하기 위해서는 새로운 클래스를 작명해야 할 것이다.

```js
import styled, { css } from "styled-components";

const colors = {
  primary: "blue",
  secondary: "gray",
};

const sizeStyles = {
  small: css`
    padding: 5px 10px;
    font-size: 12px;
  `,
  medium: css`
    padding: 10px 15px;
    font-size: 16px;
  `,
  large: css`
    padding: 15px 25px;
    font-size: 18px;
  `,
};

const StyledButton = styled.button`
  background-color: ${colors[props.bgcolor]};
  color: ${colors[props.color]};
  ${sizeStyles[props.size || "medium"]};
`;

const Button = ({ bgcolor, color, size, label }) => {
  return (
    <StyledButton bgColor={bgcolor} color={color} size={size}>
      {label}
    </StyledButton>
  );
};

export default Button;
```

위 코드와 같이 Styled Components 스타일을 적용할 경우 여러가지 장점이 있다. 가장 큰 장점은 porps를 활용해 동적인 스타일 적용이 가능하다. 또한 해당 컴포넌트에서 사용한는 스타일을 관리하기 용이하다.

### 1.4. 불필요한 스타일 로딩

기본적으로 웹 페이지에서는 전체 CSS 파일을 로드하기 때문에 불필요한 스타일을 불러오는 경우도 많다. 이 경우 성능의 저하로 이어질 수 있다.

반면 Styled Components는 실제 사용되는 스타일만 DOM에 주어지기 때문에 비교적 좋은 성능을 유지할 수 있다.

## 2 Styled Components 만들기

Styled Components는 클래스 대신 컴포넌트를 만든다. 가장 기본적인 컴포넌트를 만드는 방식을 살펴보자

```js
// Button.js
import styled from "styled-components";

const Button = styled.button`
  background-color: #6750a4;
  border: none;
  color: #ffffff;
  padding: 16px;
`;

export default Button;
```

위 코드는 `<button>` 태그에 스타일을 지정한 컴포넌트다. `styled.tagname`의 `tagname` 부분에는 스타일을 적용할 HTML 태그 이름을 작성한다. 그리고, 바로 뒤에 템플릿 리터럴 문법으로 CSS 코드를 작성한다.

```js
// App.js
import Button from "./Button";

function App(){
  return (
    <div>
      <Button>Hello Styled</Burron>
    </div>
  )
}
export default App;
```

완성된 `Button` 컴포넌트를 `App` 컴포넌트에 사용해보자

![1번]()

버튼 태그에 스타일이 잘 적용된 것을 확인할 수 있다.

```html
<div id="root">
  <button class="sc-blHHsb VERNM">Hello Styled</button>
</div>
```

개발자 도구를 확인해보면 Styled Components가 버튼에 고유한 클래스 이름 부여했다.

## 3 네스팅(Nesting)

Styled Components에서의 네스팅은 CSS 전처리기와 유사한 방식으로 스타일을 중첩하여 작성할 수 있게 해주는 기능이다. 이를 통해 컴포넌트의 구조를 더 직관적으로 표현하고 코드의 가독성을 높일 수 있다.

하지만 과도한 네스팅은 오히려 구조를 복잡하게 만들고 가독성을 떨어뜨린다. 과도한 네스팅은 지양하도록 하자

```css
/*과도한 네스팅*/
.container {
  /* ... */
  & .button {
    /* ... */
    & .icons {
      /* ... */
      &img {
        /* ... */
      }
    }
  }
}
```

### 3.1. `&` 선택자

```js
// Button.js
import styled from "styled-components";

const Button = styled.button`
  background-color: #6750a4;
  border: none;
  color: #ffffff;
  padding: 16px;

  &:hover,
  &:active {
    background-color: #463770;
  }
`;

export default Button;
```

Nesting에서 `&`는 부모 선택자를 의미한다. CSS코드로 비유해 보자.

```css
.Button {
  background-color: #6750a4;
  border: none;
  color: #ffffff;
  padding: 16px;
}

.Button:hover,
.Button:active {
  background-color: #463770;
}
```

`button` 태그의 클래스 이름을 `.Button`이라 가정할 때, `&:hover`는 `.Button:hover`를 의미한다.

### 3.2. 스타일 컴포넌트 선택자

Styled Components에선 클래스 이름을 사용하지 않는다. 그럼 컴포넌트 안에 다른 컴포넌트를 선택하고 싶다면 어떻게 하면 될까?

예를 들어 버튼 안에 존재하는 아이콘 이미지를 선택한다고 가정하자.

```js
import styled from "styled-components";
import nailImg from "./nail.png";

const Icon = styled.img`
  width: 16px;
  height: 16px;
`;

const StyledButton = styled.button`
  background-color: #6750a4;
  border: none;
  color: #ffffff;
  padding: 16px;

  & ${Icon} {
    margin-right: 4px;
  }

  &:hover,
  &:active {
    background-color: #463770;
  }
`;

function Button({ children, ...buttonProps }) {
  return (
    <StyledButton {...buttonProps}>
      <Icon src={nailImg} alt="nail icon" />
      {children}
    </StyledButton>
  );
}

export default Button;
```

`Button` 컴포넌트의 템플릿 리터럴 내부에 `Icon` 컴포넌트를 삽입하면 된다. 이 때 사용된 `& ${Icon} { ... }` 부분을 **자손 결합자**(Descendant Combinator)라고 부른다.

```css
.StyledButton {
  ...;
}

/* & ${Icon} { ... } */
.StyledButton .Icon {
  margin-right: 4px;
}
```

자손 결합자를 기존 CSS로 표현해 본다면 위와 같이 나타낼 수 있다.

```js
const StyledButton = styled.button`
  background-color: #6750a4;
  border: none;
  color: #ffffff;
  padding: 16px;

  ${Icon} {
    margin-right: 4px;
  }

  &:hover,
  &:active {
    background-color: #463770;
  }
`;
```

자손 결합자를 사용할 경우 `&`를 생략해도 동일하게 동작합니다.

```js
const StyledButton = styled.button`
  &:hover,
  &:active {
    background-color: #7760b4;

    ${Icon} {
      opacity: 0.2;
    }
  }
`;
```

중첩 [Nesting](#3-nesting)도 가능하다.

## 4. 다이나믹 스타일링

props로 전달 받은 값을 활용해 동적으로 스타일을 적용할 수 있다. 템플릿 리터럴 안에 `${}`를 사용하여 JS코드를 삽입하는 것을 **표현식 삽입법**(Expression Interpolation)이라고 부른다. 표현식 삽입법을 사용해 전달받은 props에 따라 컴포넌트의 스타일을 다르게 보여줄 수 있다.

### 4.1. 변수 사용하기

```js
const SIZES = {
  large: 24,
  medium: 20,
  small: 16,
};

const Button = styled.button`
  ...
  font-size: ${SIZES["medium"]}px;
`;
```

가장 기본적인 사용방법인 JS 변수를 그대로 삽입하는 방법

### 4.2. 함수 사용하기

```js
const SIZES = {
  large: 24,
  medium: 20,
  small: 16,
};

const Button = styled.button`
  ...
  font-size: ${(props) => SIZES[props.size] ?? SIZES["medium"]}px;
`;
```

props를 이용한 함수의 리턴값을 삽입하는 방법

### 4.3. 논리 연산자 사용하기

```js
const Button = styled.button`
  ...
  ${({ round }) => round && `border-radius: 9999px;`}
`;
```

`&&` 연산자 사용하기

```js
border-radius: ${({ round }) => round ? `9999px` : `3px`};
```

삼항 연산자 사용하기

## 5. 스타일 재사용

### 5.1. 스타일 상속

`styled()` 함수는 Styled Components로 만들어진 컴포넌트를 상속할 때 사용한다.

```js
// Button.js
import styled from "styled-components";

const SIZES = {
  large: 24,
  medium: 20,
  small: 16,
};

const Button = styled.button`
  background-color: #6750a4;
  border: none;
  color: #ffffff;
  font-size: ${({ size }) => SIZES[size] ?? SIZES["medium"]}px;
  padding: 16px;
`;

export default Button;
```

```js
// App.js
import styled from "styled-components";
import Button from "./Button";

const SubmitButton = styled(Button)`
  background-color: #de117d;
`;

function App() {
  return (
    <div>
      <SubmitButton>상속</SubmitButton>
    </div>
  );
}

export default App;
```

`styled(Button)`으로 `SubmitButton`은 `Button`의 스타일을 상속 받았다.

### 5.2. JSX 컴포넌트 스타일 적용

[스타일 상속](#51-스타일-상속)을 활용해 JSX 컴포넌트에도 스타일을 적용할 수 있다.

```js
// TermsOfService.js
function TermsOfService({ className }) {
  // className을 props로 받음
  return (
    // 스타일을 적용할 위치에 className을 적용
    <div className={className}>
      <h1>Styled Components 고수가 될거야!</h1>
    </div>
  );
}

export default TermsOfService;
```

```js
// App.js
import styled from "styled-components";
import TermsOfService from "./TermsOfService";

const StyledTermsOfService = styled(TermsOfService)`
  background-color: #ededed;
  border-radius: 8px;
  padding: 16px;
  width: 400px;
`;

function App() {
  return (
    <div>
      <StyledTermsOfService />
    </div>
  );
}

export default App;
```

JSX 컴포넌트는 HTML 요소와 같이 `styled.tagname`의 방식으로 스타일을 적용할 수 없다. 때문에 스타일 상속과 같이 `styled()` 함수를 활용해 컴포넌트에 스타일을 적용한다.

차이가 있다면 JSX문법으로 생성한 컴포넌트는 스타일이 적용될 위치를 정확히 알 수 없다. 그러므로 `className`을 props로 전달해 스타일을 적용할 정확한 위치를 명시해야한다.

### 5.3. CSS 함수

자주 사용되는 CSS 코드들은 변수처럼 저장해 사용하고 싶은 경험이 있을 것이다.
그런 상황에 사용할 수 있는 `css` 함수를 알아보자

```js
import styled from "styled-components";

const SIZES = {
  large: "24px",
  medium: "20px",
  small: "16px",
};

const Button = styled.button`
  ...
  font-size: ${({ size }) => SIZES[size] ?? SIZES["medium"]};
`;

const Input = styled.input`
  ...
  font-size: ${({ size }) => SIZES[size] ?? SIZES["medium"]};
`;
```

[앞에서는](#4-다이나믹-스타일링) 스타일 컴포넌트에 함수식을 바로 삽입해 동적인 스타일을 구현했다. 이 방법도 충분히 훌륭하다. 하지만, 함수식을 필요로 하는 컴포넌트가 더 늘어난다면, 이 함수식을 하나의 함수로 정의하는 것이 더 바람직할 것이다.

```js
// css 함수를 추가로 import 해야한다.
import styled, { css } from "styled-components";

const SIZES = {
  large: "24px",
  medium: "20px",
  small: "16px",
};

const fontSize = css`
  font-size: ${({ size }) => SIZES[size] ?? SIZES["medium"]};
`;

const Button = styled.button`
  ...
  ${fontSize}
`;

const Input = styled.input`
  ...
  ${fontSize}
`;
```

`css` 함수는 styled components에 사용할 스타일을 재사용할 수 있도록 정의하는 함수다. 이 함수를 사용할 땐 꼭 `css`를 템플릿 리터럴 앞에 붙여야한다. `css`를 붙이지 않으면 props 값을 사용할 수 없다.

props를 사용하지 않더라도 스타일을 정의할 땐 꼭 `css`함수를 사용하자

## 6. 글로벌 스타일

```js
import { createGlobalStyle } from "styled-components";

const GlobalStyle = createGlobalStyle`
  * {
    box-sizing: border-box;
  }

  body {
    font-family: 'Noto Sans KR', sans-serif;
  }
`;

function App() {
  return (
    <>
      <GlobalStyle />
      <div>글로벌 스타일</div>
    </>
  );
}

export default App;
```

`createGlobalStyle` 함수는 다른 Styled Components 함수들과 마찬가지로 템플릿 리터럴 문법으로 사용한다. 이 함수는 `style` 태그를 컴포넌트로 만드는데, 해당 위치에 `style` 태그가 생성 되는건 아니다. Styled Components가 내부적으로 처리해서 `head` 태그 안에 우리가 작성한 CSS 코드를 삽입한다.

```html
<head>
  <!-- ... -->
  <style data-styled="active" data-styled-verstion="...">
    * {
      box-sizing: border-box;
    }
    body {
      font-family: "Noto Sans KR", sans-serif;
    }
  </style>
</head>
```

적용된 스타일은 개발자 도구에서 확인할 수 있다.

## 7. 애니메이션 `keyframes`

CSS에서는 `@keyframes`를 사용했지만 stlyed components에서는 `keyframes` 함수를 사용한다. 이 함수는 다른 'styled` 함수들과 마찬가지로 템플릿 리터럴을 사용한다.

```js
// placeholder.js
import styled, { keyframes } from "styled-components";

const placeholderGlow = keyframes`
  50% {
    opacity: 0.2;
  }
`;

export const PlaceholderItem = styled.div`
  background-color: #888888;
  height: 20px;
  margin: 8px 0;
`;

const Placeholder = styled.div`
  animation: ${placeholderGlow} 2s ease-in-out infinite;
`;

export default Placeholder;
```

```js
// App.js
import styled from "styled-components";
import Placeholder, { PlaceholderItem } from "./Placeholder";

const A = styled(PlaceholderItem)`
  width: 60px;
  height: 60px;
  border-radius: 50%;
`;

const B = styled(PlaceholderItem)`
  width: 400px;
`;

const C = styled(PlaceholderItem)`
  width: 200px;
`;

function App() {
  return (
    <div>
      <Placeholder>
        <A />
        <B />
        <C />
      </Placeholder>
    </div>
  );
ㄴㄴ

export default App;
```

`keyflames` 으로 만든 애니메이션을 `${placeholderGlow}` 처럼 템플릿 리터럴에 삽입하는 형태로 사용한다.

`keyflames`이 반환하는 변수는 단순한 문자열이 아니라 JS 객체다. 때문에 `styled` 혹은 `css` 함수를 통해 사용해야 한다는 것을 주의하자

## 8. 테마 `ThemeProvider`

테마 기능을 만들기 위해서는 현재 테마로 설정된 값을 사이트 전체에서 참조할 수 있어야 한다. React에는 이런 상황에 쓰기 딱 좋은 Context가 있다. Styled Components에서도 `ThemeProvider` 를 활용해 Context 기반의 테마를 사용할 수 있다.

```js
import { ThemeProvider } from "styled-components";
import Button from "./Button";

function App() {
  const theme = {
    primaryColor: "#1da1f2",
  };

  return (
    <ThemeProvider theme={theme}>
      <Button>확인</Button>
    </ThemeProvider>
  );
}

export default App;
```

`ThemeProvider` 는 Context Provider처럼 `theme` 이라는 객체를 내려준다. `ThemeProvider` 내부에서 사용하는 Stlyed Components로 만든 컴포넌트에서는 props를 사용하듯 `theme` 이라는 객체를 사용할 수 있다.

```js
const Button = styled.button`
  background-color: ${({ theme }) => theme.primaryColor};
  /* ... */
`;
```

만약 여러 테마를 선택하고 싶다면 `useState` 를 활용해 보자

```js
import { useState } from "react";
import { ThemeProvider } from "styled-components";
import Button from "./Button";

function App() {
  const [theme, setTheme] = useState({
    primaryColor: "#1da1f2",
  });

  const handleColorChange = (e) => {
    setTheme((prevTheme) => ({
      ...prevTheme,
      primaryColor: e.target.value,
    }));
  };

  return (
    <ThemeProvider theme={theme}>
      <select value={theme.primaryColor} onChange={handleColorChange}>
        <option value="#1da1f2">blue</option>
        <option value="#ffa800">yellow</option>
        <option value="#f5005c">red</option>
      </select>
      <br />
      <br />
      <Button>확인</Button>
    </ThemeProvider>
  );
}

export default App;
```

테마 설정 페이지를 만들다보면 styled components로 만들지 않은 컴포넌트에서도 `theme`을 참조할 필요가 있는데, 그럴 땐 `useContext`를 사용하여 일반적인 React Context를 불러오듯 `ThemeContext` 를 불러오면 된다.

```js
import { useContext } from "react";
import { ThemeContext } from "styled-components";

...

function SettingPage() {
  const theme = useContext(ThemeContext);
}
```

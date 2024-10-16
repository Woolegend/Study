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

### 1.1 컴포넌트 기반 아키텍처와의 불일치

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
    <StyledApp>
      <Dashboard> ... </Dashboard>
    </StyledApp>
  );
}
```

Styled Components는 컴포넌트 별로 스타일을 격리하며 이를 해결했다. Styled Components를 사용하면 개발자가 코드를 작성할 때 클랙스를 작명할 필요가 없다. 개발이 끝나면 Styled Components각 각 디자인에 고유한 클래스 이름을 부여한다.

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

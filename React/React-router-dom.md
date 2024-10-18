> **last edit : 24.10.15**

# React Router DOM

SPA에서 페이지 나누기를 도와주는 라이브러리

## 설치

```
// @6는 6버전대를 의미
npm install react-router-dom@6

// install 대신 i만 써도 됨
npm i react-router-dom@6
```

> [react-router-dom | npm](https://www.npmjs.com/package/react-router-dom)

**목차**

- [React Router DOM](#react-router-dom)
  - [설치](#설치)
  - [1. 핵심 컴포넌트](#1-핵심-컴포넌트)
    - [1.1. Router](#11-router)
    - [1.2. Routes, Route](#12-routes-route)
    - [1.3. Link](#13-link)
    - [1.4. NavLink](#14-navlink)
  - [2. 라우트 구조화](#2-라우트-구조화)
    - [2.1. 중첩 라우트](#21-중첩-라우트)
    - [2.2. Outlet](#22-outlet)
  - [3. 동적 라우트](#3-동적-라우트)
    - [3.1 useParams](#31-useparams)
    - [3.2. useSearchParams](#32-usesearchparams)
    - [3.3. 잘못된 주소](#33-잘못된-주소)
    - [3.4. Navigate](#34-navigate)
    - [3.5. useNavigate](#35-usenavigate)

## 1. 핵심 컴포넌트

### 1.1. Router

```js
import {BrowserRouter} form 'react-router-dom';

function Main() {
  return <BrowserRouter> ... </BrowserRouter>;
}

export default Main;
```

라우터에서 사용하는 모든 데이터를 가지고 있는 컴포넌트
Context Provider와 같이 React Router에서 제공하는 기능을 사용하려면 `<Router />` 컴포넌트 안에서 사용해야 한다.

### 1.2. Routes, Route

```js
import { Routes, Route } from "react-router-dom";

<Routes>
  <Route path="/" element={<HomaPage />} />
  <Route path="/courses" element={<CourseListPage />} />
  <Route path="/courses/1" element={<CoursePage />} />
  <Route path="*" element={<NotFoundPage />} />
</Routes>;
```

Routes 컴포넌트 내부의 Route 컴포넌트를 활용해 보여줄 페이지의 경로와 컴포넌트를 지정한다. 예를들어, 사용자가 `http://localhost:3000/courses`라는 경로는 진입할 경우, 위에서부터 차례대로 `<Route />`컴포넌트의 `path`를 확인한다. 만약 일치하는 `path`가 존재할 경우 `element`의 값인 `<CoursesPage />` 컴포넌트를 렌더링한다.

### 1.3. Link

```js
import { Link } from "react-router-dom";


<Link to="/">홈페이지</Link>
<Link to="/courses">수업 탐색</Link>
<Link to="/questions">커뮤니티</Link>
```

React Router에서 `<a>` 태그 대신 사용한다.

### 1.4. NavLink

```js
import { NavLink } from "react-router-dom";


function getLinkStyle({ isActive }) {
  return {
    textDecoration : isActive ? "underline : undefined;
  }
}

<NavLink to="/courses" style={getLinkStyle}>
```

`<Link>` 컴포넌트와 사용법은 동일하다. 차이점이 있다면 style 속성을 부여할 수 있다.

## 2. 라우트 구조화

### 2.1. 중첩 라우트

중첩 라우트(Nested Routes)는 여러 레벨의 경로 구조를 관리하기 위해 사용한다. 이를 관련된 경로를 그룹화하고, 문서 구조를 더욱 명확하게 만들 수 있다.

```js
import { Routes, Route } from "react-router-dom";

<App>
  <Routes>
    <Route path="/" element={<HomePage />} />
    <Route path="/list" element={<StackList />} />
    <Route path="/list/javascript" element={<Stack />} />
    <Route path="/list/react" element={<Stack />} />
    <Route path="/list/nextjs" element={<Stack />} />
  </Routes>
</App>;
```

코드를 살펴보면 `/list`를 같은 레벨의 경로로 가진 라우트가 4개 존재한다. 이 코드에 중첩 라우팅을 적용해보면 아래와 같이 나타낼 수 있다.

```js
import { Routes, Route } from "react-router-dom";

<App>
  <Routes>
    <Route path="/" element={<HomePage />} />
    <Route path="/list">
      <Route index element={<StackList />} />
      <Route path="/javascript" element={<Stack />} />
      <Route path="/react" element={<Stack />} />
      <Route path="/nextjs" element={<Stack />} />
    </Route>
  </Routes>
</App>;
```

코드의 길이는 조금 길어졌지만 같은 하위 경로를 공유하는 라우트를 명확하게 표현하고 있다.

예시처럼 짧은 경로의 라우트에서는 큰 필요성을 느낄 수 없겠지만, `/list/stack/algorithm/dinamic-programing`과 같이 경로의 레벨이 늘어 날 때 중첩 라우트는 빛을 발한다.

### 2.2. Outlet

`Outlet`은 중첩 라우트를 사용할 때 하위 라우트의 컴포넌트를 렌더링하기 위해 사용하는 컴포넌트다. `Outlet`은 부모 라우트의 컴포넌트 내에서 하위 라우트를 삽입할 위치를 지정한다.

```js
import { Routes, Route } from "react-router-dom";

function App({ children }) {
  return (
    <>
      <Nav />
      {children}
    </>
  );
}

// Routes는 Route만을 하위 컴포넌트로 가질 수 있다.
<App>
  <Routes>
    <Route path="/" element={<HomePage />} />
    <Route path="/list">
      <Route index element={<StackList />} />
      <Route path="/javascript" element={<Stack />} />
      <Route path="/react" element={<Stack />} />
      <Route path="/nextjs" element={<Stack />} />
    </Route>
  </Routes>
</App>;
```

위 코드를 보자. 개발자는 `<App />`의 모든 하위 컴포넌트에서도 `<Nav />` 를 보여주고 싶어 `children` 을 활용해 기능을 구현했다. `children` 대신 `Outlet` 컴포넌트를 사용한다면 다음과 같이 표현할 수 있다.

```js
import { Routes, Route, Outlet } from "react-router-dom";

function App() {
  return (
    <>
      <Nav />
      <Outlet />
    </>
  );
}
// children props를 제거하고 그 자리에 Outlet 컴포넌트가 들어갔다.

<Routes>
  <Route path="/" element={<App />}>
    <Route index element={<HomePage />} />
    <Route path="/list">
      <Route index element={<StackList />} />
      <Route path="/javascript" element={<Stack />} />
      <Route path="/react" element={<Stack />} />
      <Route path="/nextjs" element={<Stack />} />
    </Route>
  </Route>
</Routes>;
```

`children`을 사용해도 되지만, `Outlet`을 사용을 권장하는데 다음과 같은 이유가 존재한다.

1. **명확한 의도 표현** :  
   <small>
   `Outlet` 은 중첩 라우트를 위해 특별히 설계된 컴포넌트다. 이는 보는 사람에게 중첩 라우트의 존재를 쉽게 알릴 수 있어, 코드 가독성 향상된다.
   </small>
2. **라우트 구성의 일관성** :  
   <small>
   `react-router-dom`의 라우트 구조를 따르기 때문에, 라우트 정의와 렌더링 방식이 일관된다. `Outlet` 을 사용하면 라우트가 계층적으로 구성되어 있다는 것을 쉽게 이해할 수 있다.
   </small>
3. **상태 관리** :  
   <small>
   `Outlet`을 사용하면 각 하위 라우트가 독립적인 상태를 가질 수 있다. `children` 을 사용할 경우 상태 관리가 더 복잡해질 수 있다.
   </small>
4. **중첩된 여러 레벨 지원** :  
   <small>
   `Outlet` 은 중첩된 여러 레벨의 라우트를 지원한다. 여러 개의 `Outlet` 을 사용할 수 있어, 복잡한 라우트 구조를 쉽게 관리할 수 있다.
   </small>

## 3. 동적 라우트

### 3.1 useParams

`useParam` 은 URL의 동적 파라미터를 추출하는데 사용되는 훅이다. 이 훅은 URL에서 추출한 파라미터를 포함하는 객체를 반환한다. 객체의 key는 Route에서 정의한 동적 파라미터의 이름과 일치한다.

```js
// https://localhost:3000/list/react

// App.js
import { Route, useParams } from "react-router-dom";

<Route path="list" element={<StackListPage />} />
 <Route path=":stack" element={<StackPage />} />
</Route>

//StackPage.js
function StackPage() {
  const { stack } = useParams()
  console.log(stack) // react
}
```

위 코드를 보면 `list`를 경로로 가진 라우트의 내부에 `:stack` 파라미터를 경로로 가진 라우트가 들어있다. 이 경우 상수 `stack`의 값으로 'react'가 들어온다.

### 3.2. useSearchParams

```js
// https://localehost:3000/search?keyword=react
import { useSearchParams } from "react-router-dom";

const [searchParams, SetSearchParams] = useSearchParams();
const keyword = searchParam.get("keyword");
console.log(keyword); // react
```

쿼리의 값을 변경하고 주소를 이동하려면 setter함수로 객체를 넘겨줘야 한다. 객체의 `key`와 `value`로 쿼리가 결정된다.

```js
setSearchParams({
  keyword: javascript,
});
```

### 3.3. 잘못된 주소

만약 사용자가 잘못된 경로로 접속한 경우를 생각해보자. React는 일치하는 라우트를 찾지 못해 아무런 페이지도 보여주지 못할 것이다.

이 때 사용할 수 있는 방법은 바로 `*`를 활용하는 것이다. `*`는 React에서 와일드 카드 문자로 불리며, 모든 URL 경로를 포괄하는 역할을 한다.

```js
// https://localhost:3000/dev
import { Routes, Route } from "react-router-dom";

<Routes>
  <Route path="/" element={<HomePage />} />
  <Route path="list" element={<StackListPage />}>
    <Route path=":stack" element={<StackPage />} />
  </Route>
  <Route path="user" element={<User />} />
  <Route path="*" element={<NotFoundPage />} />
</Routes>;
```

`*`를 경로로 가진 라우트를 마지막 요소로 배치하면 위에서 매칭되지 않은 모든 주소를 처리한다. 이를 통해 사용자에세 유용한 피드백을 줄 수 있다.

### 3.4. Navigate

사용자를 특정한 이유로 다른 주소로 이동시켜야 할 경우는 `Navigate` 컴포넌트를 사용해 볼 수 있다. `Navigate` 컴포넌트가 렌더링 되는 시점에 경로를 전환한다.

```js
// https://localhost:3000/user
import { Navigate } from "react-router-dom";

const user = getUserData();

// 유저 정보가 없을 경우 로그인 페이지로 리다이렉트
if (!user) {
  return <Navigate to="login" />;
}
```

### 3.5. useNavigate

`useNavigate`도 경로를 전환할 때 사용하는 훅이다. 주로 이벤트 핸들러나 함수 내에서 사용한다는 점에서 `Navigate`와 차이가 있다.

```js
import { useNavigate } from "react-router-dom";

const navigate = useNavigate();

const handleHomeClick = () => navigate("/");

<button onClick={handleHomeClick}>Home</button>;
```

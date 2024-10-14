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
<Routes>
  <Route path="/" element={<HomaPage />} />
  <Route path="/courses" element={<CourseListPage />} />
  <Route path="/courses/1" element={<CoursePage />} />
  <Route path="*" element={<NotFoundPage />} />
</Routes>
```

Routes 컴포넌트 내부의 Route 컴포넌트를 활용해 보여줄 페이지의 경로와 컴포넌트를 지정한다. 예를들어, 사용자가 `http://localhost:3000/courses`라는 경로는 진입할 경우, 위에서부터 차례대로 `<Route />`컴포넌트의 `path`를 확인한다. 만약 일치하는 `path`가 존재할 경우 `element`의 값인 `<CoursesPage />` 컴포넌트를 렌더링한다.

### 1.3. Link

```js
<Link to="/">홈페이지</Link>
<Link to="/courses">수업 탐색</Link>
<Link to="/questions">커뮤니티</Link>
```

React Router에서 `<a>` 태그 대신 사용한다.

### 1.4. NavLink

```js
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
<App>
  <Routes>
    <Route path="/" element={<HomePage />} />
    <Route path="/list" element={<StackList />} />
    <Route path="/list/javascript" element={<Stack />} />
    <Route path="/list/react" element={<Stack />} />
    <Route path="/list/nextjs" element={<Stack />} />
  </Routes>
</App>
```

코드를 살펴보면 `/list`를 같은 레벨의 경로로 가진 라우트가 4개 존재한다. 이 코드에 중첩 라우팅을 적용해보면 아래와 같이 나타낼 수 있다.

```js
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
</App>
```

코드의 길이는 조금 길어졌지만 같은 하위 경로를 공유하는 라우트를 명확하게 표현하고 있다. 예시처럼 짧은 경로의 라우트에서는 큰 필요성을 느낄 수 없겠지만, `/list/stack/algorithm/dinamic-programing`과 같이 경로의 레벨이 늘어 날 때 중첩 라우트는 빛을 발할 것이다.

### 2.2 Outlet

`Outlet`은 중첩 라우트를 사용할 때 하위 라우트의 컴포넌트를 렌더링하기 위해 사용하는 컴포넌트다. `Outlet`은 부모 라우트의 컴포넌트 내에서 하위 라우트를 삽입할 위치를 지정한다.

```js
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

위 코드를 보자. 개발자는 `<App />`의 모든 하위 컴포넌트에서도 `<Nav />`를 보여주고 싶다. `Outlet` 컴포넌트를 사용한다면 다음과 같이 표현할 수 있다.

```js
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

`props`의 `children`을 사용해도 되지만, `Outlet`을 사용하는데는 다음과 같은 이유가 존재한다.

1. 명확한 의도 표현 :  
   `Outlet` 은 중첩 라우트를 위해 특별히 설계된 컴포넌트다. 이는 보는 사람에게 중첩 라우트의 존재를 쉽게 알릴 수 있어, 코드 가독성 향상된다.
2. 라우트 구성의 일관성 :  
   `react-router-dom`의 라우트 구조를 따르기 때문에, 라우트 정의와 렌더링 방식이 일관된다. `Outlet` 을 사용하면 라우트가 계층적으로 구성되어 있다는 것을 쉽게 이해할 수 있다.
3. 상태 관리 :  
   `Outlet`을 사용하면 각 하위 라우트가 독립적인 상태를 가질 수 있다. `children` 을 사용할 경우 상태 관리가 더 복잡해질 수 있다.
4. 중첩된 여러 레벨 지원 :  
   `Outlet` 은 중첩된 여러 레벨의 라우트를 지원한다. 여러 개의 `Outlet` 을 사용할 수 있어, 복잡한 라우트 구조를 쉽게 관리할 수 있다.

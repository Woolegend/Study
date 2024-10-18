# Sass(Scss)

CSS는 웹 표준이기 때문에 문법이 빠르게 바뀌지 않는다. 그래서 개발자들이 사용하기 편한 여러가지 문법을 추가해 새로운 언어를 만들었다. 그 중 가장 보편적으로 사용되는 것이 바로 Sass다.

Sass는 변수, Nesting, Mixion 등등 다양한 기능을 제공한다. 이 중에서 많은 사람들이 좋다고 생각하는 문법은 웹 표준으로 역수출 되기도 했다.

Sass는 프리프로세서(Preprocessor) 스키립트 언어라고 하는데, 프리프로세서라는 프로그램을 통해서 Sass 코드를 CSS 코드로 변환해야 하기 때문이다.

Sass에는 기존 Sass와 SCSS 두 가지 문법이 있는데, 최근 CSS의 모든 문법 위에서 확장된 문법을 사용하는 SCSS를 많이 사용한다.

## 1. 설치

### 1.1. React(create-react-app)

패키지 설치 후 큰 설정없이 적용 가능

> [Adding a Sass Stylesheet | Create React App](https://create-react-app.dev/docs/adding-a-sass-stylesheet/)

### 1.2 Next.js

패키지 설치 후 큰 설정없이 적용 가능

> [Basic Features: Built-in CSS Support | Next.js](https://nextjs.org/docs/pages/building-your-application/styling#sass-support)

## 2. 사용 예시

### 2.1. Nesting

**Scss**

```css
nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  li {
    display: inline-block;
  }

  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
  }
}
```

**CSS**

```css
nav ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

nav li {
  display: inline-block;
}

nav a {
  display: block;
  padding: 6px 12px;
  text-decoration: none;
}
```

### 2.2. Mixin

**Scss**

```css
@mixin theme($theme: DarkGray) {
  background: $theme;
  box-shadow: 0 0 1px rgba($theme, 0.25);
  color: #fff;
}

.info {
  @include theme;
}

.alert {
  @include theme($theme: DarkRed);
}

.success {
  @include theme($theme: DarkGreen);
}
```

**CSS**

```css
.info {
  background: DarkGray;
  box-shadow: 0 0 1px rgba(DarkGray, 0.25);
  color: #fff;
}

.alert {
  background: DarkRed;
  box-shadow: 0 0 1px rgba(DarkRed, 0.25);
  color: #fff;
}

.success {
  background: DarkGreen;
  box-shadow: 0 0 1px rgba(DarkGreen, 0.25);
  color: #fff;
}
```

## 참고

> [Sass: Sass Basics](https://sass-lang.com/guide/)

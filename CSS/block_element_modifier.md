# BEM

BEM(Block Element Modifier)은 CSS 클래스 이름을 짓는 규칙이다.

BEM은 다음과 같은 의미를 담고 있다.

- 블록(Block) : `div`와 같은 영역을 의미
- 요소(Element) : `button`, `input` 같은 요소를 의미
- 변경자(Modifier) : 요소의 변형을 표현

이것들은 묶어 `.block__element--modifier` 형태로 클래스를 작명한다.

로그인 폼을 만든다고 가정하고 BEM을 활용해보자

```html
<!-- signup -->
<form class="signin-form">
  <label class="signin-form__label">
    Email
    <input type="text" class="signin-form__input" />
  </label>
  <label class="signin-form__label">
    Password
    <input
      type="password"
      class="signin_form__input signin_form__input--pasword"
    />
  </label>
  <button class="signin-form__button signin-form__button--submit">
    Sign In
  </button>
</form>
```

```css
.signin-form {
  /* 로그인 폼 */
}

.signin-form__input {
  /* 로그인 폼의 인풋 */
}

.signin-form__input.signin-form__input--password {
  /* 로그인 폼의 비밀번호 인풋 */
}

.signin-form__button {
  /* 로그인 폼의 버튼 */
}

.signin-form__button.signin-form__button--submit {
  /* 로그인 폼의 제출 버튼 */
}
```

## 참고

> [예제로 이해하는 BEM. | 정찬명](https://naradesign.github.io/bem-by-example.html)  
> [BEM 101 | CSS-Tricks](https://css-tricks.com/bem-101/)  
> [Quick start / Methodology / BEM](https://en.bem.info/methodology/quick-start/)  
> [BEM — Block Element Modifier](https://getbem.com/introduction/)

# JSDOC

## 구조

JSDOC은 `/**`로 시작해서 `*/`로 끝납니다. 각 줄은 `*`로 시작한다.

JSDOC은 설명할 대상 바로 위에 위치한다.

```js
/**
 * 함수 혹은 상수에 대한 설명
 * @param {type} name 매개변수에 대한 설명
 * @return {type} 반환 값에 대한 설명
 */
function exampleFunction(param) {
  //
}
```

## `@param`

함수의 매개변수를 설명한다.

기본적인 사용 방법은 다음과 같다.

### 기본 사용법

```js
/**
 * @param {type} name  매개변수에 대한 설명
 */
```

### 필수 요소와 선택 요소

매개 변수는 필수 요소와 선택 요소로 나뉘는데 이에 대한 표기는 아래와 같다.

```js
/**
 * @param {type} name  필수 요소임
 */

/**
 * @param {type} [name]  선택 요소임
 */
```

예외적으로 매개변수의 기본값이 존재할 경우 선택 요소로 표기된다.

```js
/**
 * @param {string} text  선택 요소임
 */
function hello(text = "hello") {
  console.log(hello);
}
```

### 타입

`@param`의 타입도 기존의 js에서 사용되는 타입의 표기와 동일하다

**원시 타입**

- number
- string
- boolean

**객체 타입**

- object : 일반 객체
- Object : Object 인스턴스
- Array : 배열 실제 표기는 any[]

**함수 타입**

- function : 함수
- React.JSX : React Component

**특수 타입**

- any : 어떤 값이 들어가도 상관없음

### 객체

객체 타입의 프로퍼티의 타입이 정형화 되어 있으면 추가적인 표기가 가능하다.

```js
/**
 * @param {object} user 유저 정보 객체
 * @param {stirng} user.name 유저 이름
 * @param {number} user.age 유저 나이
 * @param {boolean} user.isKorean 한국인이냐?
 */
```

## `@return`

함수의 반환값을 설명한다.

```js
/**
 * @return {type} 반환값에 대한 설명
 */
```

**객체 반환**

`@return`의 복수형 `@returns`를 사요한다. 객체 같은 경우 객체의 형태로 key와 value의 타입을 지정한다.

```js
/**
 * @returns {{loading: boolean, error : object, execute : function}}
 */
function useAsync(asyncFunction) {
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);

  const execute = ...

  return { loading, error, execute };
}
```

**비동기 반환**

비동기적인 값을 반환할 때 다음 방법을 사용해 볼 수 있다.

```js
/**
 * @return {Promise<type>} 반환값에 대한 설명
 */
```

## `@description`

함수의 기능을 자세히 설명하기 위해 사용한다.

```js
/**
 * @description 함수에 대한 자세한 묘사
 */
```

`@description`은 JSDOC 첫줄에 태그없이 작성하는 설명과 차이가 없어보인다. 하지만 이 태그를 사용하는 이유는 설명을 할 때 HTML 태그를 사용할 수 있다.

```js
/**
 * @description 이 함수는 <code>Array</code>를 정렬.
 * <p>정렬 알고리즘으로는 퀵소트를 사용.</p>
 */
function sortArray(arr) {
  // 함수 내용
}


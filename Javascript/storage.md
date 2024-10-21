# Storage

Web Storage API의 `storage` 인터페이스는 특정 도메인을 위한 세션 저장소 또는 로컬 저장소의 접근 경로로서 데이터를 추가하고 수정하거나 삭제할 수 있다.

**목차**

- [Storage](#storage)
  - [1. Sesstion Storage](#1-sesstion-storage)
  - [2. Local Storage](#2-local-storage)
  - [3. Properties](#3-properties)
    - [`Storage.length()`](#storagelength)
  - [4. Methods](#4-methods)
    - [`Storage.key()`](#storagekey)
    - [`Storage.getItem()`](#storagegetitem)
    - [`Storage.setItem()`](#storagesetitem)
    - [`Storage.removeItem()`](#storageremoveitem)
    - [`Storage.clear()`](#storageclear)
  - [참고](#참고)

## 1. Sesstion Storage

현재 `origin`<sup>[#](https://developer.mozilla.org/ko/docs/Glossary/Origin)</sup> 의 세션 저장 공간에 접근할 수 있는 `storage` 객체

`sessionStorage` 는 아래 특징을 가진다

- 페이지 세션은 브라우저가 열려있는 한 새로고침과 페이지 복구를 거쳐도 남아 있다.
- 페이지를 새로운 탭이나 창에서 열면, 세션 쿠키의 동작과는 다르게 최상위 브라우징 맥락의 값을 가진 새로운 세션을 생성한다.
- 같은 URL을 다수의 탭/창에서 열면 각각의 탭/창에 대해 새로운 `sesstionStorage` 를 생성한다.
- 탭/창을 닫으면 세션이 끝나고 `sessionStorage` 안의 객체를 초기화 한다.
- 만약 브라우저가 의도치 않게 재시작 되었을 경우 자동으로 내용을 복구한다.

## 2. Local Storage

현재 `origin`<sup>[#](https://developer.mozilla.org/ko/docs/Glossary/Origin)</sup> 의 로컬 저장 공간에 접근할 수 있는 `storage` 객체

`localStorage` 는 아래 특징을 가진다.

- 저장된 데이터는 브라우저 세션 간 공유된다.
- 탭/창을 닫으면 세션이 끝나고 `localStorage` 가 만료되지 않는다.
- 예외적으로 시크릿 모드에서 생성한 'localStorage' 는 탭이 닫힐 때 지워진다.

## 3. Properties

### `Storage.length()`

`length` 읽기 전용 속성으로, `Storage` 객체에 저장된 데이터 항목의 수를 반환합니다.

**예제**

```js
function stackStorage() {
  localStorage.setItem("front-end", "javascript");
  localStorage.setItem("back-end", "java");
  localStorage.setItem("database", "aws");

  return localStorage.length; // return 3
}
```

## 4. Methods

### `Storage.key()`

`key()` 메서드는 숫자 `n`이 전달되면 `Storage`의 `n`번째 `key` 이름을 반환한다.

`key`의 순서는 user-agent에 의해 정의되므로 이 순서에 의존성이 있어서는 안된다.

**사용법**

```js
const keyName = storage.key(index);
```

### `Storage.getItem()`

`getItem()` 메서드는 `key`를 전달하면 `key`의 `value`를 반환한다.

만약 존재하지 않은 `key`를 전달할 경우 `null`을 반환한다.

**사용법**

```js
const foodValue = storage.getItem("food");
```

### `Storage.setItem()`

`setItem()` 메서드는 `key`와 `value`를 전달했을 때, 해당 `key`를 `Storage` 객체에 추가한다.

만약 이미 존재하는 `key`일 경우 해당 `key`의 `value`를 업데이트한다.

**사용법**

```js
starage.setItem("keyName", "keyValue");
```

### `Storage.removeItem()`

`removeItem()` 메서드에 `key`를 전달하면 `Storage`에서 해당 `key`를 제거한다.

**사용법**

```js
storage.removeItem("keyName");
```

### `Storage.clear()`

`clear()` 메서드에 존재하는 모든 `key`를 제거한다.

**사용법**

```js
storage.clear();
```

## 참고

> [Storage | mdn web docs](https://developer.mozilla.org/ko/docs/Web/API/Storage)

# useRouter

## URL 파라미터 사용

`useRouter`로 URL의 파라미터와 쿼리 파라미터를 얻을 수 있다.

`/products/:id?tag=food` 에서 `id` 파라미터와 `tag` 쿼리 파라미터를 얻을려면 다음과 같이 `useRouter`를 사용하자

```js
import { useRouter } from "next/router";

export default function Product() {
  const router = useRouter();
  const { id, tag } = router.query;
}
```

Router에서 반환된 객체에서는 파라미터와 쿼리 파라미터를 따로 구분하지 않기 때문에 동일한 방식으로 얻을 수 있다.

## 페이지 이동

페이지 이동하는 방법은 간단하다. `router`의 `push()`메서드를 사용하면 된다.

`push()` 메서드에 인수로 원하는 경로를 전달하면 끝이다.

```js
const router = useRouter();

// 메인 페이지로 이동
router.push(`/`);

// 특정 경로로 이동
router.push(`/search?q=${value}`);
```

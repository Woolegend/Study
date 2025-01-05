## Automatic Batching

아래 코드는 왜 `1`이 아닌 `0`을 보여줄까?

```js
const [count, setCount] = useState(0);

const increment = () => {
  setCount((prev) => count + 1);
  console.log(count);
};

increment(); // 0
```

이유는 바로 React의 Automatic Batching에 있다. Automatic Batching은 React에서 불필요한 리렌더링을 줄여주는 최적화 기술이다. 이 기술은 특정 스코프 내에서 여러 상태 업데이트(setState 호출)가 발생할 때, 각 업데이트마다 즉시 리렌더링을 트리거하는 대신, 해당 스코프가 완료되는 시점에 모든 상태 변경을 한 번에 처리하여 단일 리렌더링만 수행한다.

React 17 버전과 그 이전에는 React Event Handler에서만 batching을 해줬지만 React 18 버전 이후부터는 Promise, setTimeout, Navtive Event Handler 등 모든 스코프에서 batching을 처리한다.

## flushSync

특정 상황에서는 batching을 원하지 않고 매 상태 변화마다 리렌더링을 발생시키길 원할 수 있다.

이 때 `ReactDom.flushSync`을 사용해보자

해당 메서드를 사용하면 명시적으로 batching을 피할 수 있다.

```js
const [count, setCount] = useState(0);

const increment = () => {
  setCount((prev) => count + 1);
  setCount((prev) => count + 1);
}; // 리렌더링 발생

console.log(count);
// 0
// 1
```

```ts
const increment = () => {
  flushSync(() => {
    setCount((prev) => prev + 1);
  }); // 리렌더링 발생
  flushSync(() => {
    setCount((prev) => prev + 1);
  }); // 리렌더링 발생
};

console.log(count);
// 0
// 1
// 2
```

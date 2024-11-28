# Outside Click Custom Hook

특정 요소의 외부 클릭을 감지하기 위한 커스텀 훅

- **params** : `ref : RefObject` 감지할 대상의 선택자
- **return** : `flag : boolean` 외부 클릭 여부

```ts
// useOutsideClick.tsx
import { RefObject, useCallback, useEffect, useState } from "react";

const useOutsideClick = <T extends HTMLElement>(ref: RefObject<T>) => {
  const [flag, setFlag] = useState(false);

  const handler = useCallback(
    (event: MouseEvent) => {
      if (!ref?.current || ref.current.contains(event.target as Node)) {
        setFlag(true);
        return;
      }
      setFlag(false);
    },
    [ref]
  );

  useEffect(() => {
    document.addEventListener("click", handler);

    return () => {
      document.removeEventListener("click", handler);
    };
  }, [handler]);

  return { flag };
};

export default useOutsideClick;
```

### 사용법

감지할 요소의 `useRef` 선택자를 생성한 후 `useOutsideClick`의 인수로 전달한다.

```ts
// component.tsx
const listRef = useRef<HTMLUListElement>(null);
const { flag: isFocus } = useOutsideClick(listRef);

return (
  <ul ref={listRef}>
    <li>neru</li>
    <li>toki</li>
    <li>kanna</li>
    ...
  </ul>
);
```

## useRef

### 1. useRef
React에서 제공하는 훅 중 하나로, **컴포넌트가 리렌더링되어도 값이 유지되는 저장 공간**이자, DOM 요소에 직접 접근할 수 있는 도구 <br>
`useState`랑 비슷하게 상태를 저장할 수 있지만, <mark> 값이 변해도 리렌더링을 트리거하지 않는다는 차이</mark> 가 있음

```js
import { useRef } from "react";

const refContainer = useRef(initialValue);
```
- `refContainer.current`에 실제 값이 들어 있음
- 값이 바뀌어도 컴포넌트는 다시 렌더링되지 않음

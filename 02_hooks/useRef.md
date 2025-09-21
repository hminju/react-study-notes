## useRef
리렌더링을 원치 않는 데이터는 useRef로


### 1. useRef
React에서 제공하는 훅 중 하나로, **컴포넌트가 리렌더링되어도 값이 유지되는 저장 공간**이자, DOM 요소에 직접 접근할 수 있는 도구 <br>
`useState`랑 비슷하게 상태를 저장할 수 있지만, <mark> 값이 변해도 리렌더링을 트리거하지 않는다는 차이</mark> 가 있음

```js
import { useRef } from "react";

const refContainer = useRef(initialValue);
```
- `refContainer.current`에 실제 값이 들어 있음
- 값이 바뀌어도 컴포넌트는 다시 렌더링되지 않음

---
### 2. useState vs useRef
| 구분          | useState      | useRef          |
| ----------- | ------------- | --------------- |
| 값이 변하면 렌더링? | 리렌더링 발생     |  **리렌더링 없음**      |
| 주로 쓰는 곳     | UI에 보여줄 상태    | DOM 접근, 저장소 역할  |
| 값 접근 방식     | `state` 직접 사용 | `.current` 프로퍼티 |

---
### 3. 주요 사용 사례
#### 3-1. DOM 요소 직접 제어
가장 흔한 사용법 <br>
ex.input 태그에 포커스를 주고 싶을때
```jsx
import { useRef } from "react";

function InputFocus() {
  const inputRef = useRef(null);

  const handleFocus = () => {
    inputRef.current.focus(); // DOM API 직접 호출
  };

  return (
    <div>
      <input ref={inputRef} type="text" placeholder="여기에 입력하세요" />
      <button onClick={handleFocus}>포커스 주기</button>
    </div>
  );
}
```
- 여기서 `inputRef.current`는 실제 `<input>` DOM 노드를 가리킴
- 실무에서는 로그인 폼, 검색창, 모달창에서 자동 포커싱할 때 자주 씀
- 

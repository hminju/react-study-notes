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

---
#### 3-2. 값 저장소로 사용(렌더링에 영향 없음)
`useState`랑 달리, 값이 변해도 렌더링이 발생하지 않아 주로 **렌더링과 무관하게 계속 유지해야 하는 값**을 저장할 때 사용
```jsx
import { useRef, useState } from "react";

function Timer() {
  const [count, setCount] = useState(0);
  const intervalRef = useRef(null);

  const startTimer = () => {
    if (intervalRef.current) return; // 중복 실행 방지
    intervalRef.current = setInterval(() => {
      setCount((prev) => prev + 1);
    }, 1000);
  };

  const stopTimer = () => {
    clearInterval(intervalRef.current);
    intervalRef.current = null;
  };

  return (
    <div>
      <p>시간: {count}</p>
      <button onClick={startTimer}>시작</button>
      <button onClick={stopTimer}>중지</button>
    </div>
  );
}
```
- `intervalRef.current`는 값이 변해도 컴포넌트를 다시 그리지 않음
- 즉 렌더링과 상관 없는 데이터(타이머ID, 외부 라이브러리 인스턴스) 등을 저장할 때 유용

---
#### 3-3. 이전 값 기억하기
렌더링 사이에서 이전 상태를 기억하고 싶을 때 사용 가능
```jsx
import { useEffect, useRef, useState } from "react";

function PreviousValue() {
  const [value, setValue] = useState("");
  const prevValue = useRef("");

  useEffect(() => {
    prevValue.current = value; // 렌더링 끝나고 값 저장
  }, [value]);

  return (
    <div>
      <input value={value} onChange={(e) => setValue(e.target.value)} />
      <p>현재 값: {value}</p>
      <p>이전 값: {prevValue.current}</p>
    </div>
  );
}
```
- 이렇게 하면 매번 리렌더링될 때 이전 값 추적 가능
- 실무에서 폼 입력 변화 추적, 애니메이션 상태 비교 등에 쓰임

---
### 4. 팁
1. 포커스 제어: 검색창 자동 포커싱, 에러 발생 시 input focus 등
2. 외부 라이브러리 인스턴스 보관
3. 드래그 앤 드롭 좌표 저장: 렌더링할 필요 없는 마우스 좌표 관리

---
### ✨ 정리
1. `useRef`는 리렌더링 없이 값 유지 + DOM 접근을 가능하게 하는 훅
2. `useState`와 다르게 값이 바뀌어도 화면은 다시 그려지지 않음
3. 실무에서는 포커스 제어, 타이머, 외부 라이브러리 객체 관리 등에서 자주 사용

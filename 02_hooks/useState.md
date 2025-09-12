## useState
React에서 상태(state)를 관리하기 위한 가장 기본적인 훅.<br>
컴포넌트 내에서 **변하는 값**을 다룰 때 사용하며, 이 값이 변경되면 리액트가 해당 컴포넌트를 자동으로 다시 렌더링

---
### 1. 왜 useState를 사용해야 할까?
<mark>리액트에서 단순히 일반 변수(`let count=0;`)를 사용하면 값이 바뀌어도 화면에 업데이트되지 않음. </mark><br>
리액트가 상태의 변화를 감지하고, 그 변화에 따라 UI를 동기화하려면 `useState`와 같은 리액트 훅을 사용해야 함.

---
### 2. useState의 기본 사용법
`useState`는 배열을 반환하며, 2가지 요소를 가진다.
1. **현재 상태 값(state)**: 현재 상태를 담고 있는 변수
2. **상태 업데이트 함수(setter)**: 이 함수를 사용하여 상태를 새로운 값으로 변경, 이 함수를 호출해야만 리액트가 상태 변화를 감지하고 컴포넌트를 다시 그린다.

기본구조:
```js
import React, { useState } from 'react';

function MyComponent() {
  const [state, setState] = useState(initialState);
  // ...
}
```
---
### 3. 상태 업데이트 특징
#### (1) 비동기적 동작
setState는 즉시 값이 바뀌지 않고, React가 한 번에 모아 처리
```jsx
setCount(count + 1);
console.log(count); // 여전히 이전 값
```

#### (2) 함수형 업데이트
새로운 값이 이전 상태에 의존한다면 안전하게 **콜백 방식** 을 사용
```jsx
setCount(prev => prev + 1);
```

#### (3) Lazy Initialization
초기값 계산 비용이 클 경우, 함수 형태로 전달하면 처음 렌더링 시에만 실행된다.
```jsx
const [value, setValue] = useState(() => heavyCalculation());
```
#### (4) 불변성 유지
- 상태를 직접 수정하면 React가 변화 감지 못함.
- 반드시 **새로운 객체/배열**을 만들어서 업데이트 해야함
```jsx
// ❌잘못된 예
state.push(4);
setState(state);

// 올바른 예
setState(prev => [...prev, 4]);
```
---
### 4. 상태 관리 전략
관련이 없는 값들을 `useState` 여러 개로 분리하는 게 깔끔하다. <br>
강하게 연결된 값들은 객체/배열로 묶어 관리할 수 있다.
```jsx
// 독립된 값
const [name, setName] = useState('');
const [age, setAge] = useState(0);

// 연결된 값
const [user, setUser] = useState({ name: '', age: 0 });
```

---

### ✨정리
- useState는 컴포넌트 내부에서 UI와 연결되는 변하는 값을 관리하는 도구
- 상태 변경 시 **자동 렌더링**이 이루어져 React의 핵심 철학(데이터에 따른 UI 동기화)을 가능하게 한다.
- 비동기성, 함수형 업데이트, Lazy Initialization, 불변성 유지

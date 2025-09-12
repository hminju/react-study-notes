## useEffect

### 1. useEffect
- React에서 **Side Effect**를 다루는 훅
  - <mark>Side Effect</mark>: 렌더링 결과물(UI 업데이트) 말고 컴포넌트 안에서 부수적으로 일어나는 동작<br>
    ex.데이터 요청, DOM 직접 조작, 구독/타이머 등록, 콘솔 출력 등

- 쉬운 비유: **useEffect= 알람** <br>
  "이 상자(useState) 값이 바뀔 때마다" 혹은 "처음 방에 들어올 때" 알람이 울려서 어떤 일을 시킴
---
### 2. 기본 사용법
```jsx
import { useEffect } from "react";

useEffect(() => {
  // 실행할 코드
});
```
- 첫 번째 인자: 실행할 함수
- 두 번째 인자(선택): **의존성 배열**
---
### 3. 의존성 배열에 다른 동작 차이
```jsx
// 1) 의존성 배열 없음
useEffect(() => {
  console.log("매 렌더링마다 실행됨");
});

// 2) 빈 배열 []
useEffect(() => {
  console.log("처음 마운트될 때 딱 한 번 실행");
}, []);

// 3) 특정 값 [count]
useEffect(() => {
  console.log("count가 바뀔 때만 실행");
}, [count]);
```
---
### 4. cleanup 함수
어떤 효과는 정리(clean-up)가 필요함.<br>
ex. 이벤트 리스너 제거, 타이머 해제, 구독 취소

```jsx
useEffect(() => {
  const timer = setInterval(() => {
    console.log("tick");
  }, 1000);

  // cleanup
  return () => {
    clearInterval(timer);
  };
}, []);
```

---
### 5. 주요 사용 예시
1. 데이터 가져오기(API요청)
2. 브라우저 이벤트 등록/해제 (`window.addEventListener`)
3. setInterval, setTimeout 같은 타이머
4. 외부 라이브러리 초기화

---
### 🍀주의할 점
- `useEffect`안에서 상태를 변경하면 → 다시 렌더링 → 다시 effect 실행 → 무한 루프 조심!
- 의존성 배열을 잘못 설정하면 원하는 타이밍에 실행되지 않거나, 불필요하게 반복 실행될 수 있다.
---
### ✨정리
- useState = 컴포넌트 안에서 **변하는 값(상태)** 을 저장하고 관리하는 도구
- useEffect = 그 상태나 렌더링이 바뀔 때마다 **부수 효과(사이드 이펙트)** 를 실행하는 도구
즉, useState는 값 자체를 다루고, useEffect는 그 값의 변화에 반응해서 일(효과)을 하는 것

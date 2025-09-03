## State

### State란?
**컴포넌트의 상태, 변화 가능한 데이터** <br>
Props: 부모가 자식에게 주는 선물, State: 컴포넌트 자신이 스스로 관리하는 개인금고 <br>
금고의 내용물(데이터)이 바뀌면, 리액트는 컴포넌트를 리렌더링해서 화면을 업데이트함.

### 1. useState 사용법
useSate: 함수 컴포넌트에서 상태를 관리하기 위해 리액트에서 제공하는 훅
```js
import React, { useState } from 'react';

function Counter() {
  // useState를 호출하면 [현재 상태 값, 상태를 업데이트하는 함수] 배열을 반환합니다.
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>현재 카운트: {count}</p>
      <button onClick={() => setCount(count + 1)}>클릭</button>
    </div>
  );
}
```

`const [count, setCount] = useState(0);` : **구조 분해 할당** 을 이용한 것 <br>
- count: 현재 상태 값을 담고 있는 변수
- setCount: count의 값을 업데이트할 때 사용하는 전용 함수

### 2. 상태 업데이트 규칙
#### 1. 상태는 불변성을 지켜야 한다.


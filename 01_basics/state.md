## State

### State란?
**컴포넌트의 상태, 변화 가능한 데이터** <br>
Props: 부모가 자식에게 주는 선물, State: 컴포넌트 자신이 스스로 관리하는 개인금고 <br>
금고의 내용물(데이터)이 바뀌면, 리액트는 컴포넌트를 리렌더링해서 화면을 업데이트함.

### 1. useState 사용법
useState: 함수 컴포넌트에서 상태를 관리하기 위해 리액트에서 제공하는 훅
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
배열이나 객체 같은 복잡한 데이터를 업데이트할 때는 기존 상태를 직접 변경하면 안됨. **항상 새로운 배열이나 객체를 만들어서** 업데이트해야 리액트가 변경을 감지

**잘못된 예**
```js
// 기존 배열을 직접 수정
const [items, setItems] = useState(['사과']);
const addItem = () => {
  items.push('바나나'); // 직접 변경
  setItems(items);
};
```

**올바른 예**
```js
// 전개 연산자(...)를 이용해 새로운 배열 생성
const [items, setItems] = useState(['사과']);
const addItem = () => {
  setItems([...items, '바나나']); // 새로운 배열을 반환
};
```

#### 2. 상태 업데이트는 비동기적으로 동작
setCount를 호출했다고 해서 그 즉시 count의 값이 변경되지는 않음.<br>
React는 여러 상태 업데이트를 모아서 한 번에 처리해 성능을 최적화<br>

```js
const [count, setCount] = useState(0);

const handleButtonClick = () => {
  setCount(count + 1);
  console.log(count); // ⚠ 여기서는 여전히 이전 상태값인 0이 출력될 수 있음
};
```
만약 상태가 업데이트된 직후의 값을 사용해야 한다면, useEffect 훅을 사용하거나 다음 규칙을 따라야 함.

#### 3. 함수형 업데이트(Functional Update)를 사용해야 한다.
이전 상태 값에 기반하여 새로운 상태 값을 계산해야 할 때 유용함.<br>
setCount 함수에 (이전 상태) => (새로운 상태) 형태의 콜백 함수를 전달<br>

```js
const [count, setCount] = useState(0);

const handleButtonClick = () => {
  // 이전 상태(prevCount)를 인자로 받아 새로운 상태를 반환
  // 여기서는 prevCount라는 진짜 최신 값을 리액트가 넣어줌
  setCount(prevCount => prevCount + 1);
};
```
// 버튼을 연달아 2번 눌러도 정확하게 +2가 됨
<br>

함수형 업데이트는 React가 이전 상태를 보장해 주기 때문에, 연속적으로 상태를 업데이트하거나 복잡한 로직을 처리할 때 매우 안전하고 신뢰할 수 있는 방법<br>
현업에서는 복잡한 상태를 다룰 때 필수적으로 사용됨.

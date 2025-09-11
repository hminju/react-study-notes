## event-handling

### 1. 이벤트 핸들링 기본
React에서 이벤트는 camelCase로 작성<br>
그리고 JSX에서는 문자열이 아니라 **함수 참조**를 전달해야 함.<br>
```jsx
// 잘못된 예시 (HTML 방식)
<button onclick="handleClick()">Click Me</button>

// 올바른 예시 (React)
<button onClick={handleClick}>Click Me</button>
```
#### 함수 호출 vs 함수 참조
- handleClick → 함수 그 자체 (참조)
- handleClick() → 함수를 지금 즉시 실행하고 반환값을 가져옴

```jsx
//함수 참조 전달 : 버튼이 눌리면 React가 handleClick() 실행
<button onClick={handleClick} /> 
```

```jsx
//함수 '호출 결과' 전달 : 렌더링 시점에 handleClick() 즉시 실행
//그 반환값이 onClick 속성에 들어감
//즉, 실제로 버튼이 눌릴 때 실행할 함수가 없어진 상태
<button onClick={handleClick()} /> 

```
#### 핸들러 작성 방식
```js
function App() {
  const handleClick = () => {
    console.log("버튼이 클릭됨!");
  };

  return <button onClick={handleClick}>Click</button>;
}

```

#### ✨ 정리
HTML: 문자열 "코드" 넣으면 브라우저가 직접 실행.<br>
React: 반드시 **함수 자체(참조)** 를 넣어야 함.

---
### 2. SyntheticEvent(합성 이벤트)
#### 2-1.상황 이해
브라우저(크롬,사파리 등)마다 조금씩 다른 이벤트 객체 제공<br>
ex. 클릭 이벤트가 발생하면
- 크롬은 MouseEvent라는 객체를,
- 다른 브라우저는 약간 다른 속성과 동작을 가진 객체를 내놓기도 함.<br>
이러면 브라우저마다 코드를 다르게 짜야 할까? 
#### 2-2.React의 해결책: 합성 이벤트
React는 브라우저 이벤트 객체를 그대로 쓰지 않고, 자기만의 **공통 래퍼(wrapper)** 를 만들었음 = 합성이벤트
- 브라우저 이벤트 → React가 한 번 감싸서 → 항상 똑같은 모양의 이벤트 객체 제공
- 덕분에 브라우저 상관 없이 `event.type`, `event.target` 같은 속성을 똑같이 쓸 수 있음

```js
function App() {
  const handleClick = (event) => {
    console.log(event.type); // "click"
    console.log(event.target); // 클릭된 DOM 요소
  };

  return <button onClick={handleClick}>Click</button>;
}
```

#### 2-3.이벤트 위임
React는 모든 이벤트 리스너를 진짜 DOM에 다 달지 않음<br>
대신 **루트에 한 번만 달아두고**, 이벤트가 발생하면 React 내부에서 "누가 눌렀는지"를 찾아서 핸들러 실행<br>
-> 이렇게 하면 성능도 좋아지고 메모리도 절약!

#### 2-4.풀링(Pooling)
**pool**: 자원을 미리 만들어두고 재사용하는 저장소 (핵심: 매번 새로 만드면 비싸니까, 만들어둔 걸 돌려쓰자) <br>

**예전 방식 React(17이전)** : 이벤트 객체를 재활용하려고 pool에 넣었다가 다시 꺼내 쓰는 방식<br>
  - 버튼을 클릭하면 이벤트 객체 만들어짐 → 핸들러로 전달 → 핸들러 실행이 끝나면 React가 그 객체를 초기화 하고 pool에 넣어둠 → 다음 이벤트가 발생하면 다시 꺼내서 재사용
  - 문제점: 이벤트가 끝난 후 나중에 쓰려고 하면 값이 다 사라짐: 왜? 핸들러가 끝나자마자 e 객체는 초기화돼서 풀에 반납됨
    
**React17 이후: 풀링 제거**
  - 이벤트 객체가 매번 새로 생성됨.
  - 비동기 코드에서도 안전하게 이벤트 정보 사용 가능
  - 객체를 매번 생성하기 때문에 메모리 사용량이 약간 증가하지만, 현대 엔진에선 무시 가능한 수준 & 전체적으로 효율적

#### ✨ 정리
SyntheticEvent = React가 만든 “브라우저 이벤트 통합 버전” <br>
이유: 브라우저마다 다른 이벤트를 하나로 통일 <br>
동작: 루트에서 이벤트를 모아서 관리(위임)<br>
React 17부터는 풀링 제거 -> 비동기에서도 안전하게 사용 가능

---
### 3.onClick 예시
- 참고: useState: 함수형 컴포넌트에서 상태(state)를 보관하고 업데이트하게 해주는 리액트 훅 <br>
호출하면 [state, setState] 쌍을 반환
```js
import React, { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>증가</button>
    </div>
  );
}
```
- 화살표 함수로 직접 전달 가능 `onClick={() => setCount(count+1)}`
    - 짧고 간단할 때 유용, 하지만 렌더링마다 함수가 새로 만들어짐(성능 이슈)
- 또는 핸들러 함수 별도로 분리 `onClick={handleClick}`
    - 핸들러를 따로 정의해두면, 컴포넌트가 커졌을 때 코드 깔끔하고 성능 최적화에도 유리

#### 상태 업데이트 시 주의점
`setCount(count + 1)` : 현재 렌더링 시점의 count 값에 의존
`setCount(prev => prev + 1)` : 항상 최신값 기준으로 업데이트 (안전) 

#### 🍀 실무에서 자주 나오는 onClick 관련 이슈
#### 1.버튼 기본 동작 막기
`<button>`은 기본적으로 form 안에 있으면 submit 이벤트를 발생시킴, 원하지 않으면 type="button" 붙이기
```jsx
<button type="button" onClick={handleClick}>Click</button>
```
#### 2.이벤트 중복 발생 방지
중복 클릭 -> 중복 API 호출 <br>
방법: disabled 상태 주거나, 클릭 시 즉시 비활성화 처리
#### 3.키보드 접근성
`onClick`만 쓰면 마우스 전용<br>
접근성을 위해 `<button>`태그를 쓰면 키보드(Enter/Space) 자동 지원됨<br>
<div onClick={...}> 같은 건 지양 → 꼭 aria 속성, role 추가 필요

#### ✨ 정리
- onClick={함수참조} vs onClick={() => 실행} → 실행 타이밍 다름
- setState(prev => prev + 1) 패턴이 안전
- form 안 버튼은 type="button" 필수
- 중복 클릭 방지, 비동기 핸들링 고려
- 접근성까지 챙기려면 <button> 태그 적극 활용

---
### 4. onChange 예시
input, textarea, select 등은 onChange로 상태 관리
```jsx
function Form() {
  const [text, setText] = React.useState("");

  const handleChange = (e) => {
    setText(e.target.value);
  };

  return (
    <div>
      <input type="text" value={text} onChange={handleChange} />
      <p>입력값: {text}</p>
    </div>
  );
}
```
- **Controlled Component** : 상태(text)가 input의 value를 직접 제어
- defaultValue를 쓰면 Uncontrolled Component -> 상태를 관리하지 않고 DOM 자체가 값 관리

### 5. 이벤트 전파(Propagation)
이벤트는 기본적으로 버블링됨.<br>
필요에 따라 전파를 막거나 기본 동작을 막을 수 있음
```js
function App() {
  const handleParent = () => console.log("부모 div 클릭됨");
  const handleChild = (e) => {
    e.stopPropagation(); // 부모로 이벤트 전파 막음
    console.log("자식 버튼 클릭됨");
  };

  return (
    <div onClick={handleParent}>
      <button onClick={handleChild}>Click Me</button>
    </div>
  );
}
```
- event.stopPropagation() : 이벤트 전파 중단

#### event.target vs event.currentTarget : 내가 누른 것 vs 내가 걸어둔 것
`event.target`: 진짜로 클릭된 곳, 예를 들어 버튼 안에 있는 `<p>` 태그를 클릭했다면, target은 `<p>`가 된다. 
`event.currentTarget`: 이벤트 핸들러를 걸어둔 곳, 버튼에 onClick을 걸어두었으면 currentTarget은 항상 `<button>`이 된다.

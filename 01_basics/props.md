## Props

### 1. Props 기본 전달
Props는 HTML의 속성과 비슷<br>
부모 컴포넌트에서 자식을 호출할 때, `key="value"` 형태로 데이터를 전달할 수 있음

부모 컴포넌트(Parent.js)
```js
// 자식 컴포넌트인 Child에게 name이라는 속성으로 "민수"라는 값을 전달
<Child name="민수" /> 
```

자식 컴포넌트(Child.js)
```js
// Child 컴포넌트는 props 객체를 인자로 받음.
// 이 객체 안에는 부모가 전달한 { name: "민수" }가 들어있음.
function Child(props) {
  return <div>안녕하세요, 제 이름은 {props.name}입니다.</div>;
}
```

여기서 중요한건, Props는 읽기 전용! <br>
자식 컴포넌트는 전달받은 Props를 바꿀 수 없음 -> 데이터가 변경되어야 한다면 부모에서 상태(state)를 변경해야함

### 2. 구조 분해(Destructuring)
매번 `props.name`과 같이 `props.`를 붙여서 쓰는건 번거로움 <br>
이럴 때, **구조분해할당**을 사용

```js
// props 객체에서 name 속성만 바로 꺼내서 사용 가능
function Child({ name }) { 
  return <div>안녕하세요, 제 이름은 {name}입니다.</div>;
}
```

### 3. defaultProps
만약 부모에서 특정 props를 전달하지 않았는데 기본값을 설정하고 싶을 때: defaultProps 사용 <br>
```js
function Child({ name }) {
  return <div>안녕하세요, 제 이름은 {name}입니다.</div>;
}

// name이라는 props가 전달되지 않으면, 기본값으로 "아무개"를 사용합니다.
Child.defaultProps = {
  name: '아무개',
};
```

### 현업 팁
#### Props Drilling(Props 드릴링)
부모에서 자식의 자식, 그 자식에게까지 Props를 계속해서 전달하는 상황<br>
코드가 복잡해지고 유지보수 어려움 -> 이런 상황에선 Context API나 Redux 같은 상태 관리 도구 사용하는 걸 고려

#### Props 유효성 검사
prop-types 라이브러리를 사용하면 Props의 타입이나 필수 여부 미리 정할 수 있음.<br>
개발 단계에서 잠재적인 버그를 빠르게 잡아낼 수 있어서 협업할 때 굉장히 유용




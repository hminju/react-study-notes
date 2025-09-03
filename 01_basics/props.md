## Props
**
<br>

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


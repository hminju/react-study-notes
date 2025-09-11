# 02. Hooks

- [useState](./useState.md) 상태 관리 (기본/함수형 업데이트)
- [useEffect](./useEffect.md) 사이드 이펙트, cleanup
- [useRef](./useRef.md) DOM 접근, 값 저장
- [useReducer](./useReducer.md) reducer 기반 상태 관리
- [useMemo & useCallback](./useMemo-useCallback.md) 성능 최적화 훅
- [Custom Hooks](./custom-hooks.md) 사용자 정의 훅


## Hooks 개요

### Hooks란?
- React 16.8에서 도입된 기능.
- 함수형 컴포넌트에서도 **상태(state)** 와 **라이프사이클 기능**을 쓸 수 있게 함.
- 클래스형 컴포넌트의 복잡한 코드 대신, **간결하고 재사용성 높은 함수형 코드**를 작성할 수 있음.
---
### 1.왜 Hooks를 쓰는 걸까?
옛날에는 React에서 상태나 라이프사이클 기능을 쓰려면 클래스형 컴포넌트를 써야 했음 
```jsx
class Counter extends React.Component {
  state = { count: 0 };

  componentDidMount() {
    console.log("컴포넌트 마운트됨!");
  }

  render() {
    return (
      <button onClick={() => this.setState({ count: this.state.count + 1 })}>
        {this.state.count}
      </button>
    );
  }
}
```
- 코드가 길고, this.state, this.setState 같은 문법이 복잡함 <br>

근데 <mark>Hooks가 등장</mark>하면서, 함수형 컴포넌트에서도 상태와 라이프사이클을 다룰 수 있게 됨.
```jsx
function Counter() {
  const [count, setCount] = React.useState(0); //상태 관리

  React.useEffect(() => {  //라이프사이클 대체
    console.log("컴포넌트 마운트됨!");
  }, []);

  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}
//상태: useState 
//라이프 사이클:useEffect
```

### 1-1. 라이프사이클
컴포넌트도 생애주기가 있음 : 마운트 -> 업데이트 -> 언마운트
```jsx
componentDidMount() {}   // 마운트
componentDidUpdate() {}  // 업데이트
componentWillUnmount() {} // 언마운트
```
Hooks에서는 useEffect 하나로 처리 가능
```jsx
useEffect(() => {
  console.log("마운트/업데이트 실행");

  return () => {
    console.log("언마운트 실행");
  };
}, [ /*의존성 배열 */]);
```
---
### 2.Hooks 규칙
1. **항상 컴포넌트 최상위에서만 호출**  
   - 반복문, 조건문, 중첩 함수 안에서 호출하지 말 것.
   - <mark>왜냐면, React가 훅을 호출한 "순서"로 기억하기 때문</mark> -> 순서가 꼬이면 React가 어떤 상태인지 헷갈림
   - 항상 컴포넌트 렌더링 최상위 레벨에서만 실행.
2. **React 함수 안에서만 호출**  
   - 컴포넌트 함수 또는 Custom Hook 안에서만 사용 가능.  
   - 일반 JS 함수에서는 호출 불가.
  
---
### 3.Hooks의 장점
1. **코드 간결성**  
   - 클래스형에서 `this.state`, `this.setState`, `componentDidMount` 등 장황했던 코드를 훅으로 간단히 표현.
2. **로직 재사용 용이**  
   - Custom Hook으로 공통 로직을 쉽게 분리해 다른 컴포넌트에서 재사용 가능.
3. **상태와 로직의 응집도**  
   - 관련 있는 코드(useEffect, useState 등)를 한 컴포넌트 안에서 함께 관리 가능.
---
### 학습 순서 가이드
1. **useState** : 상태 관리의 기초 (카운터 예제)  
2. **useEffect** : 사이드 이펙트 (데이터 fetch, 이벤트 등록/해제)  
3. **useRef** : DOM 접근, 렌더링과 무관한 값 저장  
4. **useReducer** : 복잡한 상태 업데이트 로직 분리  
5. **useMemo & useCallback** : 성능 최적화 패턴  
6. **Custom Hooks** : 공통 로직을 추출하고 재사용  


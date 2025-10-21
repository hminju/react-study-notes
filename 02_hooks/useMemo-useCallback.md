## useMemo-useCallback

### 0. 왜 이런 훅이 필요한가?
React 컴포넌트는 state나 props가 바뀌면 다시 렌더링되는데, 어떤 경우엔 불필요하게 다시 계산되거나 함수가 새로 만들어지는 문제가 생겨서 성능이 떨어질 수 있음

### 1. useMemo - 값을 메모이제이션
#### 1-1. 개념
특정 연산의 결과값을 기억(memoize)해서, 의존값이 바뀌지 않으면 이전 계산 결과를 재사용

#### 1-2. 기본 문법
```tsx
const memoizedValue = useMemo(() => {
  return 계산할_결과값;
}, [의존값]);
```


### 2. useCallback - 함수를 메모이제이션
#### 2-1. 개념
컴포넌트가 다시 렌더링될 때마다 함수가 새로 만들어지는 것을 방지 <br>
React에서 함수는 객체이기 때문에, 렌더링될 떄마다 새로운 참조값(주소)을 가짐 <br>
그래서 props로 전달할 때 문제가 생김
```tsx
function Child({ onClick }) {
  console.log("Child 렌더링");
  return <button onClick={onClick}>클릭</button>;
}

function Parent() {
  const [count, setCount] = useState(0);

  const handleClick = () => setCount(count + 1);

  return <Child onClick={handleClick} />;
}
```
`Parent`가 렌더링될 때마다 `handleClick`이 새로 생성됨 <br>
-> Child 입장에서는 props가 바뀐 걸로 인식 <br>
-> Child도 불필요하게 리렌더링됨 <br>

#### 해결: useCallback
```tsx
const memoizedFn = useCallback(() => {
  실행할_코드;
}, [의존값]);
```

### 3. 정리 비교
| 훅             | 역할        | 리턴 | 주로 사용하는 상황                                |
| ------------- | --------- | -- | ----------------------------------------- |
| `useMemo`     | 계산 결과를 기억 | 값  | 무거운 계산 결과를 재사용하고 싶을 때                     |
| `useCallback` | 함수를 기억    | 함수 | props로 전달하는 콜백 함수가 불필요하게 새로 만들어지는 걸 방지할 때 |


### 4. 주의할 점
- 무조건 사용 X
  - useMemo/ useCallback도 메모이제이션 비용이 있기 때문에, 정말 "비싼 연산"이나 "불필요한 렌더링"이 있을 때만 사용해야 함
- 의존성 배열을 항상 정확하게 관리해야 함
  - 놓치면 값이 갱신되지 않거나, 예상치 못한 버그가 생길 수 있음


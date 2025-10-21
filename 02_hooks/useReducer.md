## useReducer

### 0. 언제 useState 대신 useReducer?
- 상태가 여러 값으로 얽혀 함께 업데이트되어야 할 때 (ex.여러 필드 의존 폼)
- 업데이트 규칙이 단계적/분기적일 때 (로딩/성공/실패)
- 업데이트 원인(이벤트)를 기록/로그/테스트하고 싶을 때
- 컴포넌트 트리 여러 곳에서 같은 로직을 재사용하고 싶을 때
- 반대로, 단순한 토글/ 숫자1-2개 수준이면 useState가 더 간단함

**보통 처음은 `useState`로 시작하는데, 프로젝트가 커지면 상태가 점점 많아지고, 서로 의존하게 되고, 업데이트 로직이 컴포넌트 여기저기 흩어짐 => 이때 "상태를 이벤트 단위로" 다루고 싶어함 <br>
즉, "지금 어떤 일이 일어났는가" 를 중심으로 하게 되는데, 그게 바로 `useReducer`**

### 1. useReducer의 기본 구조
- state: 데이터
- action: 일어난 사건
- reducer: 그 사건이 일어나면 상태를 어떻게 바꿀지 정하는 함수

#### 1-1. 기본 문법

```js
const [state, dispatch] = useReducer(reducer, initialState);
```
- `state`: 현재 상태 값
- `dispatch`: 액션을 보낼 때 쓰는 함수
- `reducer`: 상태를 실제로 바꾸는 규칙 함수

#### 1-2. 예시
```tsx
function reducer(state, action) {
  switch (action.type) {
    case 'plus':
      return { count: state.count + 1 };
    case 'minus':
      return { count: state.count - 1 };
    default:
      return state;
  }
}

export default function Counter() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });

  return (
    <div>
      <button onClick={() => dispatch({ type: 'minus' })}>-</button>
      <span>{state.count}</span>
      <button onClick={() => dispatch({ type: 'plus' })}>+</button>
    </div>
  );
}
```

이렇게 되면<br>
- 상태 변경 로직은 reducer안에 깔끔히 정리
- 화면 쪽(컴포넌트)은 어떤 일이 일어났는지(action)만 신경 씀

### 2. 이게 왜 좋은가?
| 상황         | useState일 때          | useReducer일 때        |
| ---------- | -------------------- | -------------------- |
| 상태가 많을 때   | setState들이 중구난방      | reducer에서 한눈에 정리     |
| 조건 분기 많을 때 | if/else가 컴포넌트에 섞임    | switch문으로 명확하게       |
| 로직 재사용     | 어려움                  | reducer 함수로 따로 빼기 가능 |
| 디버깅        | “누가 상태를 바꿨는지” 추적 어려움 | action 로그로 쉽게 추적     |

즉, 규모가 커질수록 유지보수하기 쉬운 구조
(폼, 모달, 비동기 요청 상태 관리에 자주 씀)



### 3. reducer 작성 원칙
1. 순수 함수: 같은 입력 -> 같은 출력, 사이드이펙트 금지(요청, 로그, 타이머 등은 바깥에서)
2. 불변성 유지: 기존 객체를 직접 수정X, 새 객체를 반환
3. 명확한 액션 이름: type은 "사용자/도메인 이벤트" 중심으로
4. 에러는 조기 발견: unknown action은 throw로 막아도 좋음
5. 상태 전이 완결성: 가능한 모든 action에 대한 처리 경로를 책임지도록

### 4. 3가지 포인트
1. `useReducer`는 `useState`의 업그레이드 버전이 아님 (상태 구조가 복잡할 때 쓰는 대체재)
2. 상태 변경 규칙을 한 곳에 모음
3. dispatch는 "무슨 일이 일어났는지"만 던짐

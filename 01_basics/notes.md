# 01. React Basics

## 0. React

### React 이름

- React라는 이름은 "상태 변화(State change)에 반응하여 ui를 업데이트한다." 는 철학에서 나옴.
- 즉, 개발자가 직접 DOM을 조작하지 않고, 상태가 변하면 React가 알아서 화면을 반응적으로 다시 그려준다.

### React 특징

1. 선언형(Declarative)
   - "어떻게 바꿀지"가 아니라 "무엇을 보여줄지" 만 선언
   - 상태가 바뀌면 React가 자동으로 필요한 부분만 업데이트
2. 컴포넌트 기반
   - UI를 작은 단위로 쪼개고 조립해서 큰 화면을 만든다.
   - 재사용 가능하고 유지보수에 유리.
3. Virtual DOM
   - 실제 DOM을 직접 수정하지 않고, 가상 DOM을 이용해 변경 사항을 효율적으로 계싼 후 최소한만 업데이트
4. 단방향 데이터 흐름
   - 데이터는 부모->자식으로 흘러간다.(props)
   - 앱의 상태 관리가 직관적이고 예측 가능


## 1. JSX (JavaScript XML)

- React에서 UI를 표현하는 문법.
- JavaScript 안에서 HTML처럼 작성 가능.
- 실제로는 `React.createElement`로 변환됨.

## 2. 컴포넌트

- 설명 …
- 예제 코드: `example/Greeting.jsx`

## 3. Props

- 설명 …
- 예제 코드: `example/Greeting.jsx`

## 4. State

- 설명 …
- 예제 코드: `example/Counter.jsx`

## 5. 이벤트 처리

- 설명 …
- 예제 코드: `example/Toggle.jsx`

## 6. 조건부 렌더링

- 설명 …
- 예제 코드: `example/Conditional.jsx`

## 7. 리스트 렌더링 & key

- 설명 …
- 예제 코드: `example/ListRendering.jsx`

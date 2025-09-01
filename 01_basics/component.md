## 컴포넌트

### 컴포넌트란?
UI를 재사용 가능한 작은 단위로 쪼개서 만든 것
- HTML 태그처럼 보이지만, 사실은 JS 함수/클래스
- 앱을 여러 개의 컴포넌트로 나눠서 관리하면 유지보수와 확장성 측면에서 좋음

ex.
```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```
사용할 때:
```
<Welcome name="mj" />
```

### 컴포넌트의 규칙
- 이름은 항상 대문자로 시작
- 반드시 하나의 부모 요소로 감싸야 함

### 컴포넌트의 종류
1. 함수형 컴포넌트(Function Component)
- 현재 React에서 표준
- 단순하고 가벼움
- 그냥 JS 함수
- props를 받아서 JSX를 return
- Hook(useState, useEffect) 사용 가능

2. 클래스형 컴포넌트 (Class Component)
- React 옛날 버전에서 주로 사용
- 상태 관리와 생명주기 메서드 지원
- class 문법으로 작성
- this.state, this.props 사용

### 컴포넌트의 장점
1. 재사용성: 같은 UI를 반복해서 쉽게 사용 가능
2. 가독성: 코드가 모듈화되어 구조가 명확
3. 유지보수 용이: 작은 단위로 나뉘어 수정이 쉬움
4. 테스트 편의성: 독립적인 단위로 테스트 가능


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

### Hooks 규칙
1. **최상위에서만 호출**  
   - 반복문, 조건문, 중첩 함수 안에서 호출하지 말 것.  
   - 항상 컴포넌트 렌더링 최상위 레벨에서만 실행.
2. **React 함수 안에서만 호출**  
   - 컴포넌트 함수 또는 Custom Hook 안에서만 사용 가능.  
   - 일반 JS 함수에서는 호출 불가.
  

### Hooks의 장점
1. **코드 간결성**  
   - 클래스형에서 `this.state`, `this.setState`, `componentDidMount` 등 장황했던 코드를 훅으로 간단히 표현.
2. **로직 재사용 용이**  
   - Custom Hook으로 공통 로직을 쉽게 분리해 다른 컴포넌트에서 재사용 가능.
3. **상태와 로직의 응집도**  
   - 관련 있는 코드(useEffect, useState 등)를 한 컴포넌트 안에서 함께 관리 가능.
  
### 학습 순서 가이드
1. **useState** : 상태 관리의 기초 (카운터 예제)  
2. **useEffect** : 사이드 이펙트 (데이터 fetch, 이벤트 등록/해제)  
3. **useRef** : DOM 접근, 렌더링과 무관한 값 저장  
4. **useReducer** : 복잡한 상태 업데이트 로직 분리  
5. **useMemo & useCallback** : 성능 최적화 패턴  
6. **Custom Hooks** : 공통 로직을 추출하고 재사용  


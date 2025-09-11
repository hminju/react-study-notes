# 02. Hooks

- [useState](./useState.md) 상태 관리 (기본/함수형 업데이트)
- [useEffect](./useEffect.md) 사이드 이펙트, cleanup
- [useRef](./useRef.md) DOM 접근, 값 저장
- [useReducer](./useReducer.md) reducer 기반 상태 관리
- [useMemo & useCallback](./useMemo-useCallback.md) 성능 최적화 훅
- [Custom Hooks](./custom-hooks.md) 사용자 정의 훅

---

## Hooks 개요

### Hooks란?
- React 16.8에서 도입된 기능.
- 함수형 컴포넌트에서도 상태(state)와 라이프사이클 기능을 쓸 수 있게 함.

### 특징
1. 클래스형보다 코드 간결 & 직관적.
2. 공통 로직을 Custom Hook으로 재사용 가능.
3. 훅 규칙: **최상위에서만 호출**, **조건문/반복문 안에서 호출 X**.

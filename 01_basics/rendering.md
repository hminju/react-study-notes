## 조건부/리스트 렌더링

### 1.조건부 렌더링 (Conditinoal Rendering)
1-1. 기본 패턴
- **&&연산자** : 조건이 true일 때만 표시
```jsx
  {isLoggedIn && <p>환영합니다!</p>}
```
- **삼항연산자** : 두 경우 중 하나 선택
```jsx
{isLoading ? <p>로딩중...</p> : <p>완료!</p>}
```
- **if문** : 조건이 길거나 여러 단계일 때 가독성 좋음
```jsx
//로딩, 에러, 데이터 없음, 정상 데이터 패턴
if (error) return <p>에러!</p>;
if (isLoading) return <p>로딩중...</p>;
if (items.length === 0) return <p>데이터 없음</p>;
return <ItemList items={items} />;
```
- undefined/null 안전 처리
- 

1-2. 개념 포인트
- JSX 안에서는 `{}`안에 **표현식**만 넣을 수 있음-> 그래서 if문 대신 `&&`, `? :` 많이 씀.
  - <mark>왜 JSX안에서는 `{}`안에 표현식만 넣을 수 있나?</mark>
  - JSX는 브라우저가 직접 이해하는 문법이 아니라, Babel같은 트랜스파일러가 js코드로 변환해주는 문법 설탕
  - 즉, `{}`안에 들어간 건 그냥 자바스크립트 값으로 바뀌어야 함.
  - 표현식: 값으로 평가될 수 있는 것(결과적으로 값 반환) -> JSX안에 `{}`는 결국 값을 채워 넣는 자리라서 표현식만 가능
- false 값 주의: `0 && <Text />` -> 0도 flasy라서 안 보임.
- 조건이 복잡하면 JSX안에서 삼항을 여러 번 쓰지 말고, 위에서 변수/if문으로 분리하는 게 유지보수에 좋음
- **undefined /null 안전 처리**
  - 값이 없을 수 있는 경우: `{user && <p>{user.name}</p>}`
  - 더 간단하게: `{user?.name}` (옵셔널 체이닝)

---
### 2. 리스트 렌더링(List Rendering)
2-1 기본 패턴
```jsx
const fruits = [
  { id: 1, name: "사과" },
  { id: 2, name: "바나나" },
  { id: 3, name: "체리" }
];

<ul>
  {fruits.map((fruit) => (
    <li key={fruit.id}>{fruit.name}</li>
  ))}
</ul>
```


2-2.key의 중요성
- **React 는 Virtual DOM을 비교(diff)할때 key를 기준으로 함.**
- key가 없으면 React는 모두 새로 생긴거라고 판단해 불필요한 재렌더링 발생
- `index`를 key로 쓰면 항목 순서가 바뀔 때 input 값 꼬임, 애니메이션 버그 발생
- **항상 고유한 값(id)** 을 key로 사용

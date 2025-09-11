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

2-2 개념 포인트
- **Fragment (<>...</>) 활용**
  - map안에서 여러 태그를 반환할 때 불필요한 `<div>`대신 `Fragment`를 쓰면 DOM이 깔끔해짐
    ```jsx
      {fruits.map(fruit => (
    <React.Fragment key={fruit.id}>
      <h3>{fruit.name}</h3>
      <p>맛있어요</p>
    </React.Fragment>
    ))}
    ```
- **컴포넌트 분리 습관**
  - map안에 JSX가 길어지면 따로 FruitItem 컴포넌트로 분리하는 게 유지보수에 좋음
  ```jsx
    {fruits.map(fruit => (
      <FruitItem key={fruit.id} fruit={fruit} />
    ))}
    ```
- **대량 데이터 렌더링 시 성능**
  - 수백~수천 개 리스트를 바로 map 하면 성능 저하 발생
  - 실무에서는 <mark>가상 스크롤링</mark> 같은 라이브러리 사용 (react-window, react-virtualized)

2-2.key의 중요성
- **React 는 Virtual DOM을 비교(diff)할때 key를 기준으로 함.**
- key가 없으면 React는 모두 새로 생긴거라고 판단해 불필요한 재렌더링 발생
- `index`를 key로 쓰면 항목 순서가 바뀔 때 input 값 꼬임, 애니메이션 버그 발생
- **항상 고유한 값(id)** 을 key로 사용

---
### 🍀 완전히 다른 목적을 가진 key와 id
#### key
- React 내부에서만 쓰는 특별한 props
- 리액트가 Virtual DOM을 비교할 때 "이 요소가 이전과 같은 건지, 새로 생긴 건지" 구분하는 기준
- 오직 리스트 렌더링 최적화를 위해 존재
- 렌더링 결과물(DOM)에는 나타나지 않음
  ```jsx
      {users.map(user => (
      <li key={user.id}>{user.name}</li>
    ))}
    ```
  - 여기서 `key={user.id}`는 React가 `li`들을 추적하기 위한 정보일 뿐, `<li>`태그 안에 `key`라는 속성이 남지 않음
#### id
- HTML 속성의 하나
- DOM 요소를 고유하게 식별하는 용도(CSS 선택자)
- 실제로 브라우저 DOM에 남아 있음
  ```jsx
  <li id="user">홍길동</li>
    ```
  - 여기서 `id="user"`는 HTML 속성이므로 CSS에서 `#user{color: red;}` 이렇게 접근 가능

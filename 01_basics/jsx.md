## JSX

### 1. JSX란?
JSX(JavaScript XML) : js안에서 HTML 같은 문법을 쓸 수 있게 해주는 React의 문법 확장<br/>
: 사실 브라우저는 JSX를 직접 이해하지 못함. 그래서 Babel 같은 트랜스파일러가 JSX를 순서 JavaScript로 변환해줌

### 1.1 JSX가 왜 필요한가?
처음 React를 접하면 JSX는 그냥 "HTML 같은 문법"으로 보이지만, 사실은 **UI를 JavaScript 안에서 표현하기 위한 도구**<br/>
과거에는 document.createElement 같은 명령어 코드로 DOM을 만들었다.<br/>
React는 "UI = 상태의 함수" 라는 철학을 따른다. 즉 **상태(state)** 가 바뀌면 UI도 자동으로 다시 그려져야 한다.<br/>
그런데 JS로만 UI를 표현하면 너무 장황해진다.

```
const element = React.createElement("h1", null, "Hello, world!");
```
이걸 더 사람 친화적인 문법으로 만든 게 JSX
```
const element = <h1>Hello, world!</h1>;
```

### 1.2 JS와 JSX의 관계
- javaScript(JS): 브라우저가 직접 이해할 수 있는 언어. 우리가 평소 쓰는 순수 JS 코드
- JSX           : **JavaScript의 확장 문법** . 브라우저는 원래 이해 못하지만, Babel 같은 도구가 변환해주면 결국 JS가 된다. <br>
-> 즉, JSX는 개발자가 쓰기 편한 **문법 설탕**일 뿐, 실행될 때는 100% JS 코드로 변한다. 
---

### 2. 흐름 이해 : JSX -> Babel -> React.createElement
- JSX: 우리가 작성하는 문법
- Babel: 브라우저가 이해할 수 있도록 JSX를 JS 함수 호출로 변환 (Babel: JavaScript 컴파일러)
- React.createElement: 변환된 결과물, 결국 React element(plain object)를 리턴
  
-> React element는 단순한 JS 객체라서 React가 **diffing(Virtual DOM 비교)** 에 활용할 수 있어요.<br>
-> 같은 일을 하지만 **JS 버전은 기계 친화적, JSX 버전은 사람 친화적**<br>
-> JSX는 JS를 대체하는 게 아니라, JS 안에서 UI를 표현하는 새로운 문법 스타일이다.

---

### 3.기본문법과 변환
**(1) HTML처럼 보이지만 JS임**
```
const element = <h1>Hello, world!</h1>;
```
이건 사실 아래 코드로 변환된다.
Babel 변환 후:
```
const element = React.createElement("h1", null, "Hello, world!");
```
<br/>

**(2) 중괄호 {}안에 JS 표현식 가능**

```jsx
const name = "mj";
const element = <h1>Hello, {name}!</h1>;
```

Babel 변환후:

```jsx
const element = React.createElement("h1", null, `Hello, ${name}!`);
```
<br/>

**(3) 속성 표기법**

- 문자열은 큰따옴표 ""
- JS 값은 중괄호 {}
```
const element1 = <img src="logo.png" alt="logo" />;
const element2 = <button disabled={true}>Click</button>;
```
  
<br/>

**(4) 반드시 하나의 부모 요소로 감싸야 함**

```
// ❌ 오류
const element = (
  <h1>Title</h1>
  <p>Description</p>
);

// 올바른 방법
const element = (
  <div>
    <h1>Title</h1>
    <p>Description</p>
  </div>
);

참고: React 16+부터는 <></>(Fragment)로도 감쌀 수 있음.
```
---

### 4. JSX에서 중요한 3가지 패턴
#### 1. JS 표현식 중괄호 {}
JSX 안에서 변수를 출력하거나 조건부 렌더링에 활용 가능하다.
```jsx
<h1>{user.name}님, 환영합니다!</h1>
{isLoggedIn && <LogoutButton />}
```

#### 2. 속성(props) 처리
HTML과 비슷하지만 camelCase 규칙을 따르고, JS 값은 중괄호 안에 넣는다.
```
<button onClick={handleClick} disabled={true}>Click</button>
```

#### 3. 컴포넌트 구조
JSX는 결국 함수 호출처럼 컴포넌트를 표현하는 문법이다.
```
<UserProfile user={user} />
```

---
✨정리
- JSX는 React.createElement 를 더 읽기 쉽게 표현한 문법
- {} 안에는 JS 표현식(변수, 함수 호출, 연산 등)을 넣을 수 있다.
- 하나의 부모 요소로 감싸야 하며, Fragment(<>...</>)도 가능하다.
- 실무에서 자주 쓰는 패턴은 중괄호 표현식 / 속성 처리 / 컴포넌트 구조 등

## DOM
React 특징 중에서 Virtual DOM이나 직접 DOM 조작을 안 한다라는 말이 나오는데, 여기서 DOM이 뭔지 모르면 헷갈릴 수 있다.
<br/>

### DOM이란?
DOM(Document Object Model)
  = "브라우저가 HTML문서를 해석해서 만든 트리 구조 객체"

쉽게 말해서 웹페이지를 js로 조작할 수 있도록 바꿔놓은 모델
ex. 아래와 같은 HTML이 있다고 할 때,

 ```html
<html>
  <body>
    <h1>Hello</h1>
    <button>Click Me</button>
  </body>
</html>
```

브라우저는 이걸 읽고 내부적으로 아래와 같이 트리구조 (DOM Tree)를 만든다.
```
Document
 └── html
      └── body
           ├── h1 ("Hello")
           └── button ("Click Me")
```
<br/>

### DOM 조작 예시
js로 DOM을 조작하면 화면도 바뀐다
```html
document.querySelector("h1").textContent = "Hi!";
```
-> 브라우저 화면의 `<h1>`텍스트가 Hello -> Hi!로 바뀐다.

<br/>

### DOM의 한계
DOM 조작은 유연하지만, **변경할 때마다 브라우저가 화면을 다시 그려야 해서** 성능 부담이 크다.
- 예를 들어, 100개 요소 중 1개만 바꾸더라도 브라우저는 레이아웃 계산, 리렌더링을 다시 실행한다.

<br/>

### Virtual DOM은?
실제 DOM을 직접 바꾸는 건 느림
React는 Virtual DOM(가상의 DOM 복사본) 을 메모리에 두고, "상태 변화에 따라 어떤 부분만 바꺼야 하는지" 계산해서 필요한 부분만 실제 DOM에 반영

즉,
1. 상태가 바뀌면 -> Virtual DOM이 먼저 업데이트 됨
2. 이전 Virtual DOM과 비교(Diffing) -> 바뀐 부분만 실제 DOM에 반영
3. 불필요한 전체 리렌더링을 줄여 성능 최적화

<br/>

### ✨정리
**DOM**: 웹페이지 구조를 트리 형태로 표현한 모델 (자바스크립트로 직접 조작 가능).
**Virtual DOM**: React가 성능 최적화를 위해 쓰는 “가짜 DOM” (실제 DOM에 필요한 부분만 반영).


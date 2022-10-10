---
title: SolidJS - Bindings - Events
date: 2022-10-10T00:18:04+09:00
last_modified_at: 2022-10-10T19:12:35+09:00
---


https://www.solidjs.com/tutorial/bindings_events

Solid의 이벤트는 on 접두사가 붙은 속성이다.

모든 on 바인딩은 대소문자를 구분하지 않기 때문에 이벤트 이름은 소문자여야 한다. 예를 들어, `onMouseMove`는 `mousemove`를 모니터링한다.

```tsx
<div onMouseMove={handleMouseMove}>
  The mouse position is {pos().x} x {pos().y}
</div>
```

`on:` 네임스페이스를 사용하면 대소문자를 구분하고 이벤트 위임을 사용하지 않는다.

```tsx
<button on:DOMContentLoaded={() => /* Do something */} >Click Me</button>
```

## 문제

TARGET DECK
전체::개발::solid

---

<!--ankiQ-->

Solid 에서 엘리먼트에 이벤트 핸들러를 등록하기 위해서는 크게 2가지 방법이 있다. 어떤 방식이 있고, 어떤 차이가 있는가?

<!--ankiA-->

```tsx
<div onMouseMove={handleMouseMove}>
  The mouse position is {pos().x} x {pos().y}
</div>
```

```tsx
<button on:DOMContentLoaded={() => /* Do something */} >Click Me</button>
```

1. on 접두사
	- ex: onMouseMove
	- 대소문자를 구분하지 않는다(이벤트 이름은 소문자여야 함)
	- 이벤트 위임 O
2. on: 네임스페이스
	- ex: on:mousemove
	- 대소문자를 구분한다
	- 이벤트 위임 X

<!--ankiE-->
<!--ID: 1664959847793-->

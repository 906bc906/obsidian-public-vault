---
title: SolidJS - Introduction - JSX
date: 2022-10-10T00:07:28+09:00
last_modified_at: 2022-10-26T01:11:30+09:00
---

https://www.solidjs.com/tutorial/introduction_jsx

Solid는 JSX 를 사용한다. 다른 프레임워크와는 달리 가능한 한 HTML 표준에 가깝게 유지하려고 노력한다.

하지만 JSX 가 HTML의 상위집합이 될 수는 없는데, 그 이유는 다음과 같다
1. void 엘리먼트가 없다. 닫는 태그 또는 셀프 클로징을 해야 한다. `<input>`, `<br>` 엘리먼트를 복사할 때 특히 유의해야 한다.
2. JSX는 단일 엘리먼트를 반환해야 한다. 여러 최상위 엘리먼트를 반환하고자 한다면 Fragment 엘리먼트를 사용해야 한다.
3. JSX는 HTML 주석(`<!-- -->`)이나 `<!DOCTYPE>` 같은 특별한 태그를 지원하지 않는다.

---

## 문제

TARGET DECK
전체::개발::solid

<!--ankiQ-->

solid는 void 엘리먼트가 없다. 이것이 의미하는 것은?

<!--ankiA-->

`<input>`, `<br>` 같은 엘리먼트를 복사할 때 닫는 태그 또는 셀프 클로징을 해야 함.

<!--ankiE-->
<!--ID: 1664943671024-->


<!--ankiQ-->

solid에서 다음의 두 최상위 엘리먼트를 반환하고 싶다면 어떻게 해야겠는가?

```ts
  <h1>Title</h1>
  <p>Some Text</p>
```

<!--ankiA-->

fragment 엘리먼트로 감싼다.

```ts
<>
  <h1>Title</h1>
  <p>Some Text</p>
</>
```

<!--ankiE-->
<!--ID: 1664943850598-->


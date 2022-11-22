---
title: SolidJS - Props - Splitting Props
date: 2022-10-10T00:19:23+09:00
last_modified_at: 2022-11-17T01:31:47+09:00
---

https://www.solidjs.com/tutorial/props_split

props를 그룹으로 나누어 현재 컴포넌트에서 일부 사용하고, 나머지는 자식 컴포넌트로 전달하고 싶은 경우가 있다.

```tsx
export default function Greeting(props) {
  const { greeting, name, ...others } = props;
  return <h3 {...others}>{greeting} {name}</h3>
}
```

위와 같이 디스트럭처링을 이용하면 반응성을 잃어 이름을 설정하더라도 업데이트되지 않는다.

대신 splitProps를 사용하면 반응성을 유지할 수 있다.

```tsx
export default function Greeting(props) {
  const [local, others] = splitProps(props, ["greeting", "name"]);
  return <h3 {...others}>{local.greeting} {local.name}</h3>
}
```

- props 객체와 추출하려는 하나 이상의 키 배열을 인수로 받는다.
- 함수의 반환값을 props 객체의 배열
	- 첫번째 원소는 추출한 키들로 구성된 props 객체
	- 두번째 원소는 추출하고 남은 키들로 구성된 props 객체
	- 둘 다 반응성을 유지함

## 문제

TARGET DECK
전체::개발::solid

---

<!--ankiQ-->

```tsx
export default function Greeting(props) {
  const { greeting, name, ...others } = props;
  return <h3 {...others}>{greeting} {name}</h3>
}
```

위 Solid 코드는 Greeting 컴포넌트의 props를 greeting, name과 나머지 키들로 쪼개 , 나머지 키들을 h3 컴포넌트에 전달하려는 목적을 가지고 있다.

하지만 구조분해할당을 하면서 반응성을 잃은 상태인데, 어떻게 수정하면 반응성을 유지하면서 props를 분리할 수 있는가?

<!--ankiA-->

```tsx
import { splitProps } from "solid-js";

export default function Greeting(props) {
  const [local, others] = splitProps(props, ["greeting", "name"]);
  return <h3 {...others}>{local.greeting} {local.name}</h3>
}
```

<!--ankiE-->
<!--ID: 1664967738921-->

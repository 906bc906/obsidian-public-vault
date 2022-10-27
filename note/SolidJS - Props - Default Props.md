---
title: SolidJS - Props - Default Props
date: 2022-10-10T00:19:16+09:00
last_modified_at: 2022-10-26T01:11:30+09:00
---

https://www.solidjs.com/tutorial/props_defaults

Props는 JSX에 바인딩된 모든 속성을 나타내는, 실행시 컴포넌트 함수에 전달되는 객체다. Props 객체는 읽기 전용이며, Object getter로 래핑된 리액티브 프로퍼티를 갖는다. 이를 통해 호출자가 시그널이나 시그널 표현식, 정적인 값을 사용했는지 여부와 관계없이 props.propName 과 같은 일관된 형식으로 액세스할 수 있다.

트래킹 스코프 밖에서 props 객체를 디스트럭처링하면서 반응성을 잃을 수 있기 때문에, 디스트럭처링을 사용하지 않는 것이 중요하다. 일반적으로 Solid의 프리미티브나 JSX 밖에서 props 객체의 프로퍼티에 액세스하면 반응성을 잃을 수 있다. 이는 디스트럭처링 뿐만 아니라 스프레드 연산자, Object.assign과 같은 함수에도 적용된다.

Solid는 props를 사용할 때 도움이 되는 몇 가지 유틸리티를 제공한다.
- mergeProps
	- 반응성을 잃지 않고 리액티브 객체를 머지(비파괴적인 Object.assgin)할 수 있다.
	- 컴포넌트의 디폴트 props를 설정할 때 일반적으로 많이 사용한다.

```tsx
export default function Greeting(props) {
  return <h3>{props.greeting || "Hi"} {props.name || "John"}</h3>
}
```

위의 코드를 mergeProps를 이용하여

```tsx
import { mergeProps } from "solid-js";

export default function Greeting(props) {
  const merged = mergeProps({ greeting: "Hi", name: "John" }, props);

  return <h3>{merged.greeting} {merged.name}</h3>
}
```

이렇게 수정할 수 있다. 

```tsx
export default function Greeting(props) {
  const merged = {  greeting: "Hi", name: "John", ...props };

  return <h3>{merged.greeting} {merged.name}</h3>
}
```

위 케이스의 경우 props를 디스트럭처링하여 반응성을 잃는다. props가 변동되더라도 재 렌더링 되지 않는다.

## 문제

TARGET DECK
전체::개발::solid

---

<!--ankiQ-->

```tsx
export default function Greeting(props) {
  return <h3>{props.greeting || "Hi"} {props.name || "John"}</h3>
}
```

위 Solid 코드는 props의 기본 값을 지정하기 위해 템플릿에 인라인으로 지정했다. 하지만 props.greeting 등의 값이 여기 저기서 쓰여야 한다면 인라인 지정은 코드를 굉장히 난잡하게 만든다.

Solid에서는 이러한 경우를 위해, 반응성을 잃지 않고 리액티브 객체를 머지할 수 있는 유틸리티를 제공한다. 이를 제공하여 위 코드를 수정하라.

<!--ankiA-->

```tsx
import { mergeProps } from "solid-js";

export default function Greeting(props) {
  const merged = mergeProps({ greeting: "Hi", name: "John" }, props);

  return <h3>{merged.greeting} {merged.name}</h3>
}
```

<!--ankiE-->
<!--ID: 1664967175913-->


---

<!--ankiQ-->

```tsx
export default function Greeting(props) {
  const merged = {  greeting: "Hi", name: "John", ...props };

  return <h3>{merged.greeting} {merged.name}</h3>
}
```

위 Solid 코드는 어떤 위험성을 내포하고 있는가?

<!--ankiA-->

props를 트래킹 스코프 바깥에서 디스트럭처링하여 반응성을 잃는다. 즉, props가 시그널 등으로 업데이트되더라도 재 렌더링되지 않을 것이다.

<!--ankiE-->
<!--ID: 1664967175930-->

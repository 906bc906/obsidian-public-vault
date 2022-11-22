---
title: SolidJS - Props - Children
date: 2022-10-10T00:19:30+09:00
last_modified_at: 2022-11-17T01:31:47+09:00
---

https://www.solidjs.com/tutorial/props_children

children 헬퍼에 대한 내용은 튜토리얼이 매우 빈약하기 때문에 [영문 api 문서](https://www.solidjs.com/docs/latest/api#children)를 참고하십시오. (한국어 api 문서는 업데이트 되지 않았음)

Solid의 성능을 높이는 부분 중 하나는 컴포넌트가 기본적으로 함수 호출이기 때문이다. 컴파일러가 객체의 getter 안에서 잠재적으로 리액티브한 표현식을 래핑하는 방식으로 업데이트를 전파한다. 컴파일 결과를 다음과 같이 그려볼 수 있다.

```tsx
// 이 JSX 표현식이
<MyComp dynamic={mySignal()}>
  <Child />
</MyComp>

// 이렇게 컴파일됩니다.
MyComp({
  get dynamic() { return mySignal() },
  get children() { return Child() }
});
```

이는 props가 지연 평가됨을 의미한다. props에 대한 액세스는 사용될 때까지 연기된다. 이로 인해 추가 래퍼나 동기화를 도입하지 않으면서도 반응성을 유지할 수 있다.

하지만 이는 동시에 props 에 재차 액세스하는 것은 자식 컴포넌트나 엘리먼트가 다시 생성될 수도 있음을 의미한다.

`props.children` 은 해당 엘리먼트의 자식 엘리먼트들을 나타내는 속성이다.  정확히 말하자면 자식 엘리먼트라는 시그널의 getter 다. props.children 에 접근할 때마다 자식이 중복해서 생기는데, 자식을 의도적으로 중복 생성하는 경우 이 동작이 도움이 되겠지만 대부분의 경우 의도하지 않게 자식 노드를 중복으로 만들어 성능 저하를 유발하는 경우가 많다.

이러한 이유로, Solid는 `children` 헬퍼를 제공한다. 이 메서드는 `children` prop에 대해 memo를 생성하고, 자식과 직접 인터랙션을 할 수 있도록 중첩된 자식들에 대해서도 리액티브한 참조를 제공한다.

아래 예시를 통해 props.children 과 children 헬퍼에 대해 어느 정도 감을 잡을 수 있다.

```tsx
//main.jsx
import { render } from "solid-js/web";
import { createSignal, For } from "solid-js";

import ColoredList from "./colored-list";

function App() {
  const [color, setColor] = createSignal("purple");
  const [child, setChild] = createSignal(["Most", "Interesting", "Thing"]);
  setTimeout(()=>{setChild([...child(), "?"])}, 3000);
  return <>
    <ColoredList color={color()}>
      <For each={child()}>{item => <div>{item}</div>}</For>
    </ColoredList>
    <button onClick={() => setColor("teal")}>Set Color</button>
  </>;
}

render(() => <App />, document.getElementById('app'));
```

main.jsx에서는 ColoredList의 자식을 For 컴포넌트로 생성하는데, 이때 For 컴포넌트의 each 속성에 child 시그널을 달았다. 3초 후에 child 시그널의 값인 배열에 "?" 가 추가되는데, 이는 colored-list.jsx 에서 자식에 대한 반응성을 보기 위함이다.

```tsx
//colored-list.jsx
import { createEffect, children } from "solid-js";

export default function ColoredList(props) {
  const c = children(() => props.children);
  const c2 = props.children();
  const c3 = props.children;
  
  createEffect(() => c().forEach(item => item.style.color = props.color));
  createEffect(()=> c2.forEach(item => item.style.color = props.color == "purple" ? props.color : 'gray'));
  createEffect(()=> c3().forEach(item => item.style.color = props.color == "purple" ? props.color : 'gray'));
  
  return <>{c()}{c2}{c3()}{props.children}</>
}
```

ColoredList 는 fragment 컴포넌트로 묶어 `c()`, `c2`, `c3()`, `props.children` 을 반환하고 있다.

- `c()`
	- 가장 이상적인 방법으로, `children` 헬퍼를 이용하여 props.children 으로 생성된 노드들을 재사용하고 있다.
	- props.children의 변경에 반응한다.
	- props.color의 변경에 반응한다.
- `c2`
	- props.children() 으로 생성된 노드들을 보관하고 있다.
	- props.children의 변경에 반응하지 않는다.
	- props.color의 변경에 반응한다.
- `c3()`
	- props.children의 변경에 반응한다.
	- props.color의 변경에 반응한다.
- `props.children`
	- props.children의 변경에 반응한다.
	- props.color의 변경에 반응하지 않는다.

`children` 헬퍼는 자식의 수에 따라 반환 형태가 다르다.
- 2개 이상 : Array
- 1개 : function
- 0개 : null
- 만약 무조건 배열로 노멀라이즈하고 싶다면 `c()` 가 아니라 `c.toArray()` 와 같이 사용해야 한다

```tsx
const Wrapper = (props) => {
  return <div>{props.children}</div>;
};
```

위 케이스와 같이 `props.children` 을 그대로 통과시키는 경우는 굳이 children 헬퍼를 이용할 필요가 없다.

```tsx
import { createEffect, children } from "solid-js";

export default function ColoredList(props) {
  const c = children(() => props.children);
  createEffect(() => c().forEach(item => item.style.color = props.color));
  return <>{c()}</>
}
```

children 헬퍼의 중요한 부분은 props.children에 즉시 접근하기 때문에 생성되도록 강요한다는 것이다. 이는 자식을 조건부 렌더링해야 되는 경우 바람직하지 않다. 

```tsx
const resolved = children(() => props.children);

return <Show when={visible()}>{resolved()}</Show>;
```

Show 조건이 충족될 때만 자식을 평가하고 싶다면 아래와 같이 수정하면 된다.

```tsx
const resolved = children(() => visible() && props.children);
```

## 문제

TARGET DECK
전체::개발::solid

---

<!--ankiQ-->

Solid의 `props.children` 은 접근할 때마다 새로운 노드가 중복 생성된다. 자식과 직접 인터랙션을 하면서도 리액티브한 참조를 얻기 위해서 사용하는 기능은 무엇인가?

사용 예시를 들라.

<!--ankiA-->

```tsx
const c = children(()=>props.children);
```

`children` 헬퍼.

<!--ankiE-->
<!--ID: 1664976941767-->

---

<!--ankiQ-->

```tsx
//main.jsx
import { render } from "solid-js/web";
import { createSignal, For } from "solid-js";

import ColoredList from "./colored-list";

function App() {
  const [color, setColor] = createSignal("purple");
  const [child, setChild] = createSignal(["Most", "Interesting", "Thing"]);
  setTimeout(()=>{setChild([...child(), "?"])}, 3000);
  return <>
    <ColoredList color={color()}>
      <For each={child()}>{item => <div>{item}</div>}</For>
    </ColoredList>
    <button onClick={() => setColor("teal")}>Set Color</button>
  </>;
}

render(() => <App />, document.getElementById('app'));
```

```tsx
//colored-list.jsx
import { createEffect, children } from "solid-js";

export default function ColoredList(props) {
  const c = children(() => props.children);
  const c2 = props.children();
  const c3 = props.children;
  
  createEffect(() => c().forEach(item => item.style.color = props.color));
  createEffect(()=> c2.forEach(item => item.style.color = props.color == "purple" ? props.color : 'gray'));
  createEffect(()=> c3().forEach(item => item.style.color = props.color == "purple" ? props.color : 'gray'));
  
  return <>{c()}{c2}{c3()}{props.children}</>
}
```

위 Solid 코드는 props.children 과 children 헬퍼의 차이를 파악하기 위해 작성되었다.

ColoredList 가 반환하는 fragment 엘리먼트 내부는 props.children과 props.color에 반응성을 가질 수 있다. 각각의 결과를 예상하라.

<!--ankiA-->

||c()|c2|c3()|props.children|
---|---|---|---|---
props.children|O|X|O|O
props.color|O|O|O|X

<!--ankiE-->
<!--ID: 1664976941790-->

---

<!--ankiQ-->

```tsx
const resolved = children(() => props.children);

return <Show when={visible()}>{resolved()}</Show>;
```

현재는 Show 조건과 상관 없이 props.children 이 평가된다.

Show 컴포넌트의 조건이 충족될 때 props.children 이 평가되게끔 하려면 어떻게 해야겠는가?

<!--ankiA-->

api)

```tsx
const resolved = children(() => visible() && props.children);
```

실제로는 false가 반환되는 문제가 있어 resolved.toArray를 걸어도  오류가 발생하는 문제가 있다.

```tsx
const resolved = children(() => visible() ? props.children : []);
```

위와 같이 적용해야 의도한 대로 동작한다.

<!--ankiE-->
<!--ID: 1664976941795-->

---
title: SolidJS - Bindings - Forwarding Refs
date: 2022-10-10T00:18:45+09:00
last_modified_at: 2022-10-26T01:11:31+09:00
---

https://www.solidjs.com/tutorial/bindings_forward_refs

Ref 전달

컴포넌트 내부의 ref를 부모에게 노출하고 싶은 경우가 많이 있는데, props.ref 를 활용하여 ref를 부모에게 전달할 수 있다.

```tsx
//부모 컴포넌트
...
	return <Canvas ref={canvas} />;

//자식 컴포넌트 (Canvas)
export default function Canvas(props) {
  return <canvas ref={props.ref} width="256" height="256" />;
}
```

## 문제

TARGET DECK
전체::개발::solid

---

<!--ankiQ-->

```tsx
//부모 컴포넌트
...
  return <Canvas />;

//자식 컴포넌트
export default function Canvas(props) {
  return <canvas width="256" height="256" />;
}

```

위의 Solid 코드에서 자식 컴포넌트 Canvas의 ref를 부모 컴포넌트에 전달하고 싶다. 이 경우 어떻게 해야 ref를 부모 컴포넌트에 전달할 수 있겠는가?

<!--ankiA-->

```tsx
//부모 컴포넌트
...
	return <Canvas ref={canvas} />;

//자식 컴포넌트 (Canvas)
export default function Canvas(props) {
  return <canvas ref={props.ref} width="256" height="256" />;
}
```

<!--ankiE-->
<!--ID: 1664963394344-->

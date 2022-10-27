---
title: SolidJS - Reactivity - On
date: 2022-10-10T00:30:37+09:00
last_modified_at: 2022-10-26T01:11:30+09:00
---

https://www.solidjs.com/tutorial/reactivity_on

https://www.solidjs.com/docs/latest/api#on

편의를 위해 Solid는 계산에 필요한 디펜던시를 명시적으로 설정할 수 있도록 on 헬퍼를 제공한다.

이는 주로 어떤 시그널이 추적되는지에 대한 명시적이고 간결한 방법으로 사용된다. 다른 시그널들을 읽더라도 기재되지 않았다면 추적하지 않는다.

```tsx
createEffect(on(a, (a) => {
  console.log(a, b());
}, { defer: true }));
```

추가로 on은 defer 옵션을 제공하는데, 이를 사용하면 계산이 즉시 실행되지 않고 (시그널 값을 초기화할 때도 이펙트가 즉시 실행되는데 이를 무시하고) 첫번째 변경시에만 실행되도록 할 수 있다.

```tsx
createEffect(on([a, b], () => {
	console.log(a(), b());
}, {defer: false}));

createEffect(on([a, b], ([_a, _b]) => {
	console.log(_a, _b);
}, {defer: false}));
```

## 문제

TARGET DECK
전체::개발::solid

---

<!--ankiQ-->

Solid의 effect 에 필요한 디펜던시를 명시적으로 설정하려면 어떤 것을 사용해야 하는가?

<!--ankiA-->

on

```tsx
createEffect(on(a, (a) => {
  console.log(a, b());
}, { defer: true }));
```

<!--ankiE-->
<!--ID: 1665050636459-->

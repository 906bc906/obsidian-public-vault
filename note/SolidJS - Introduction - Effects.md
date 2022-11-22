---
title: SolidJS - Introduction - Effects
date: 2022-10-10T00:09:20+09:00
last_modified_at: 2022-11-17T01:31:47+09:00
---

https://www.solidjs.com/tutorial/introduction_effects

시그널을 보완하기 위해, 추적 가능한 값으로 업데이트 할 수 있는 옵저버가 있다. 이펙트는 옵저버 중 하나로, 시그널에 의존하는 사이드 이펙트를 실행한다.

`solid-js`에서 `createEffect`를 import해 이펙트를 생성할 수 있다. 이펙트는 함수가 실행되는 동안 읽은 모든 시그널을 자동으로 구독하고, 시그널이 변경되면 다시 실행된다.

count가 변경될 때마다 다시 실행되는 이펙트는 아래와 같다.

```ts
createEffect(() => {
  console.log("The count is now", count());
});
```

Effect의 인자에 별도로 signal을 등록하는 부분이 없는 것을 확인할 수 있다. 위에서 말했듯 createEffect의 인자로 주어진 함수에서 `count()` 로 시그널을 받아오고, 이 부분을 createEffect가 읽고 count 시그널을 구독하게 된다.


createEffect 로 만들어진 이펙트는 렌더링이 완료된 후 실행되며, 주로 DOM과 상호 작용하는 업데이트를 스케줄링하는 데 사용된다. DOM을 좀 더 일찍 수정하려면, [createRenderEffect](https://www.solidjs.com/docs/latest/api#createrendereffect)를 사용해야 한다(createEffect가 렌더링이 완료될 때까지 기다린 후에 실행된다면, createRenderEffect는 함수를 즉시 실행한다).

---
## 문제

TARGET DECK
전체::개발::solid

<!--ankiQ-->

solid

count 시그널을 구독하여, 이를 console.log로 출력하는 effect를 생성하라.

<!--ankiA-->

```ts
createEffect(() => {
	console.log("The count is now", count());
});
```

<!--ankiE-->
<!--ID: 1664945484877-->


<!--ankiQ-->

createEffect 의 실행 시기는 주의해야 할 점이 있는데, 이것이 무엇이며, 대안은 무엇인가?

<!--ankiA-->

createEffect는 렌더링이 완료 된 후에 실행된다.

렌더링 전에 즉시 실행하고 싶다면 createRenderEffect를 사용해야 한다.

<!--ankiE-->
<!--ID: 1664945484881-->

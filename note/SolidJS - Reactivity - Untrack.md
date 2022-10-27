---
title: SolidJS - Reactivity - Untrack
date: 2022-10-10T00:30:26+09:00
last_modified_at: 2022-10-26T01:11:30+09:00
---

https://www.solidjs.com/tutorial/reactivity_untrack

리액티브 컨텍스트 내에서도 시그널 읽기 작업을 추적하지 않는 것이 필요하다. Solid는 래핑 계산이 읽기 작업을 추적하지 못하도록 하기 위해 untrack이라는 헬퍼를 제공한다.

```tsx
createEffect(() => {
	console.log(a(), b());
});
```

예제에서 b가 변경되었을 때, 로그를 출력하고 싶지 않다고 가정하자. 

```tsx
createEffect(() => {
  console.log(a(), untrack(b));
});
```

위와 같이 변경하여 b 시그널을 추적 해제할 수 있다.

시그널은 함수이기 때문에 직접 전달할 수 있지만, untrack은 더 복잡한 동작으로 함수를 래핑할 수 있다.

untrack이 읽기 추적을 비활성화하더라도, 쓰기 작업에는 영향을 미치지 않기 때문에 쓰기 작업은 여전히 추적되고 옵저버에 알림이 가게 된다.

## 문제

TARGET DECK
전체::개발::solid

---

<!--ankiQ-->

```tsx
createEffect(() => {
	console.log(a(), b());
});
```

위 이펙트에서 b 에 의한 이펙트는 발생하지 않도록, b 읽기 추적을 비활성화하려면 어떻게 해야 하는가?

<!--ankiA-->

```tsx
createEffect(() => {
  console.log(a(), untrack(b));
});
```

<!--ankiE-->
<!--ID: 1665048669559-->

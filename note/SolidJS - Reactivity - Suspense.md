---
title: SolidJS - Reactivity - Suspense
date: 2022-10-10T00:58:13+09:00
last_modified_at: 2022-10-26T01:11:30+09:00
---


https://www.solidjs.com/tutorial/async_suspense

`lazy`와 `createResource`를 단독으로 사용할 수 있지만, Solid는 여러 비동기 이벤트 표시를 조정하는 메커니즘도 제공한다, `Suspense`는 이러한 비동기 이벤트가 완료될 때까지 일부 로드된 컨텐츠 대신 fallback 플레이스홀더를 표시할 수 있는 경계 역할을 한다.

이렇게 하면 대량의 로딩 상태로 인한 버벅거림을 제거하여 사용자 경험을 개선할 수 있다. `Suspense`는 하위 비동기 읽기를 자동으로 감지하고 그에 따라 작동한다. 필요한 만큼의 `Suspense` 컴포넌트를 중첩할 수 있으며, `loading` 상태가 변경되면 가장 가까운 상위 항목만 `fallback` 상태로 변환된다.

```tsx
const Greeting = lazy(async () => {
  // simulate delay
  await new Promise(r => setTimeout(r, 1000))
  return import("./greeting")
});

...

<Greeting name="Jake" />
```

lazy component인 Greeting을 포함하는 JSX 를, Suspense로 로딩 플레이스홀더를 추가하면,

```tsx

...

<Suspense fallback={<p>Loading...</p>}>
	<Greeting name="Jake" />
</Suspense>
```

와 같이 바꿀 수 있다.

단, `Suspense` 는, Resource 시그널(lazy 컴포넌트 포함)이 읽히지 않으면 정지되지 않는다. 단순히 비동기 fetch 자체를 기다리는 것이 아님에 유의해야 한다.

## 문제

TARGET DECK
전체::개발::solid

---

<!--ankiQ-->

```tsx
<Greeting name="Jake" />
```

위 Greeting 컴포넌트는 lazy하게 불러온다. 컴포넌트가 로딩되는 동안, Suspense를 이용하여 `<p>Loading...</p>` 를 대신 출력하려면 어떻게 해야겠는가?

<!--ankiA-->

```tsx
<Suspense fallback={<p>Loading...</p>}>
	<Greeting name="Jake" />
</Suspense>
```

<!--ankiE-->
<!--ID: 1665058595393-->

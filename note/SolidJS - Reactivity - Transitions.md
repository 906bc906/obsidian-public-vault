---
title: SolidJS - Reactivity - Transitions
date: 2022-10-10T00:31:11+09:00
last_modified_at: 2022-11-17T01:31:46+09:00
---

https://www.solidjs.com/tutorial/async_transitions

`Suspense`를 사용하면 데이터가 로딩 중인 경우 fallback 컨텐츠를 표시할 수 있다. 이는 초기 로딩에는 훌륭하지만, 이어지는 네비게이션에서는 스켈레톤 상태로 폴백하는 것이 사용자 경험에 더 나쁜 경우가 있다.

`useTransition`을 활용해 fallback 상태로 돌아가는 것을 방지할 수 있는데, 이는 래퍼와 pending 인디케이터를 제공한다. 래퍼는 모든 비동기 이벤트가 완료될 때까지 커밋되지 않은 모든 다운스트림 업데이트를 트랜잭션에 넣는다.

제어 흐름이 서스펜드되면, 다음 오프-스크린을 렌더링하는 동안은 계속 현재 브랜치를 표시하게 된다. 현재 경계 내에 있는 Resource를 읽는 경우, 이를 트랜지션에 추가한다. 하지만, 새로 생성된 중첩된 `Suspense` 컴포넌트는 표시되기 전에 로딩이 완료되지 않은 경우 "fallback"을 표시한다.

예제에서 탭을 이동하는 경우, 컨텐츠가 loading 플레이스홀더로 되돌아가는 것을 볼 수 있다. `App` 컴포넌트에 트랜지션을 추가해보자. 먼저 updateTab 합수를 다음과 같이 변경한다.

```tsx
const [pending, start] = useTransition();
const updateTab = (index) => () => start(() => setTab(index));
```

`useTransition`은 pending 시그널 인디케이터와 트랜지션 시작 메서드를 반환한다.

peding 시그널을 사용해 인디케이터를 UI에 제공해야 하는데, 탭 컨테이너 div에 pending 클래스를 추가한다.

```tsx
<div class="tab" classList={{ pending: pending() }}>
```

이제 탭 전환이 훨씬 부드러워진 것을 볼 수 있다.
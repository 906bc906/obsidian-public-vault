---
title: SolidJS - Reactivity - Lazy Components
date: 2022-10-10T00:30:45+09:00
last_modified_at: 2022-10-10T19:12:34+09:00
---

https://www.solidjs.com/tutorial/async_lazy

이 내용은 code splitting에 대한 이해가 필요하다.

대부분의 번들러는 동적 임포트를 사용하는 경우 자동으로 코드 분할을 처리한다. Solid의 Lazy 메서드를 사용하면 지연된 로딩을 위해 컴포넌트의 동적 임포넌트를 래핑할 수 있다. 반환값은 JSX에서 사용할 수 있는 컴포넌트다.

다만 처음 렌더링될 때 내부적으로는 임포트한 코드를 동적으로 로드하여, 코드를 사용할 수 있을 때까지 해당 렌더링 분기를 중지한다.

```tsx
import Greeting from "./greeting";
```

위와 같은 임포트를

```tsx
const Greeting = lazy(() => import("./greeting"));
```

위와 같이 lazy 메서드로 변경할 수 있다.

```tsx
const Greeting = lazy(async () => {
  await new Promise(r => setTimeout(r, 1000));
  return import("./greeting")
});
```

눈으로 확인하기 위해 억지로 딜레이를 추가하면 지연 컴포넌트가 늦게 렌더링되는 것을 확인할 수 있다.

## 문제

TARGET DECK
전체::개발::solid

---

<!--ankiQ-->

```tsx
import Greeting from "./greeting";
```

위 import 를 lazy 컴포넌트로 변경하라.

<!--ankiA-->

```tsx
const Greeting = lazy(() => import("./greeting"));
```

<!--ankiE-->
<!--ID: 1665054490931-->

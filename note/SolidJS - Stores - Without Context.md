---
title: SolidJS - Stores - Without Context
date: 2022-10-10T00:20:38+09:00
last_modified_at: 2022-10-10T00:20:38+09:00
---


https://www.solidjs.com/tutorial/stores_nocontext

컨텍스트 미사용

컨텍스트는 Store를 위한 훌륭한 도구다. 인젝션을 처리하고, 소우권을 리액티브 그래프에 연결하고, 자동으로 할당 해제를 관리하며, Solid의 세분화된 렌더링 덕분에 렌더링 오버헤드가 없다.

때로는 컨텍스트를 사용하는게 너무 과도한 경우가 있으며, 대안은 리액티브 시스템을 직접 사용하는 것이다. 예를 들어, 전역 스코프에 시그널을 생성하고 다른 모듈에서 사용할 수 있도록 export함으로써 전역 리액티브 데이터 스토어를 구축할 수 있다.

```tsx
import { createSignal } from 'solid-js';

export default createSignal(0);

// 다른 소스 파일:
import counter from './counter';
const [count, setCount] = counter;
```

Solid의 반응성은 보편적인 개념이다. 이것이 컴포넌트 내부에 있는지 외부에 있는지는 중요하지 않다. 전역 상태 vs 로컬 상태에 대한 별도의 구분은 없으며, 모두 같은 것이다.

유일한 제한 사항은 **모든 계산(이펙트/메모)이 리액티브 루트(createRoot) 아래에 생성**되어야 한다는 점이다. Solid의 render는 이를 자동으로 수행한다.

이 예제에서는 counter.tsx가 전역 Store이다. main.tsx의 컴포넌트를 다음과 같이 수정할 수 있다.

```tsx
const { count, doubleCount, increment } = counter;

return (
  <button type="button" onClick={increment}>
    {count()} {doubleCount()}
  </button>
);
```

따라서 계산을 포함하는 더 복잡한 전역 Store를 사용할 때는 루트를 생성해야 한다.

```tsx
import { createSignal, createMemo, createRoot } from "solid-js";

function createCounter() {
  const [count, setCount] = createSignal(0);
  const increment = () => setCount(count() + 1);
  const doubleCount = createMemo(() => count() * 2);
  return { count, doubleCount, increment };
}

//export default createCounter;
export default createRoot(createCounter);
```

## 문제

TARGET DECK
전체::개발::solid

---

<!--ankiQ-->

컨텍스트를 과하게 사용하는 경우가 있고, 이에 대한 대안은 리액티브 시스템을 직접 사용하는 것이다. 

```tsx
import { createSignal } from 'solid-js';

export default createSignal(0);

// 다른 소스 파일:
import counter from './counter';
const [count, setCount] = counter;
```

위와 같이 전역 스코프에 시그널을 생성하고 export, 다른 모듈에서 import 해서 쓸 수 있다.

이 방식에 제한 사항이 있는데 무엇인가?

<!--ankiA-->

모든 계산(이펙트, 메모)이 리엑티브 루트 아래에 생성되어야 한다.

Solid의 render 에 리액티브 루트를 만드는(`createRoot`) 작업이 포함되어 있어 그 동안은 신경쓰지 않고 작업했지만, 리액티브 시스템을 직접 사용할 때는 이를 신경써야 한다.

```tsx
import { createSignal, createMemo, createRoot } from "solid-js";

function createCounter() {
  const [count, setCount] = createSignal(0);
  const increment = () => setCount(count() + 1);
  const doubleCount = createMemo(() => count() * 2);
  return { count, doubleCount, increment };
}

//export default createCounter;
export default createRoot(createCounter);
```

위처럼 createRoot로 감싸주면 된다.

<!--ankiE-->
<!--ID: 1665047073531-->

---
title: SolidJS - Stores - Context
date: 2022-10-10T00:20:05+09:00
last_modified_at: 2022-10-10T00:20:05+09:00
---

https://www.solidjs.com/tutorial/stores_context

Solid에서 데이터를 다루다 보면, props를 사용하지 않고 전역적으로 데이터를 관리할 필요가 있다. 이럴때 사용하는 것이 Context API다. Context 는 시그널과 스토어를 공유할 때 유용하며, 리액티브 시스템의 일부로 생성되고, 관리된다는 장점이 있다.

```tsx {title="counter.jsx"}
const CounterContext = createContext();
```

먼저 위와 같이 Context 객체를 생성한다.

```tsx {title="counter.jsx"}
...
export function CounterProvider(props) {
	const [count, setCount] = createSignal(props.count || 0);
}
```

전역으로 공유하고자 하는 count, setCount 시그널을 생성한다.

```tsx {title="counter.jsx"}
export function CounterProvider(props) {
  const [count, setCount] = createSignal(props.count || 0);

  return (
    <CounterContext.Provider value={[count, setCount]}>
      {props.children}
    </CounterContext.Provider>
  );
}
```

`createContext` 로 생성한 컨텍스트 객체에는 `Provider` 가 포함되어 있다. 이는 데이터를 주입하는데 사용되는 컴포넌트이므로, 우리가 만든 컨텍스트 컴포넌트의 반환값을 컨텍스트 객체의 Provider로 감싼다.

`CounterProvider`로 App 을 감쌀 것이기 때문에, 반환값 안에 props.children 을 패싱한다.

```tsx {title="main.jsx"}
import { render } from "solid-js/web";
import Nested from "./nested";
import { CounterProvider } from "./counter";

function App() {
  return <>
    <h1>Welcome to Counter App</h1>
    <Nested />
  </>
};

render(() => (
  <CounterProvider count={1}>
    <App />
  </CounterProvider>
), document.getElementById("app"));
```

렌더 부분을 `CounterProvider` 로 감싼다. 이제 `CounterProvider` 의 데이터가 App 내부에 주입된다.

주입된 데이터를 사용하기 위해서는, 컨텍스트 객체로부터 useContext 컨슈머를 import 해야 한다.

```tsx {title="counter.jsx"}
import { createSignal, createContext, useContext } from "solid-js";

const CounterContext = createContext();

export function CounterProvider(props) {
  const [count, setCount] = createSignal(props.count || 0);

  return (
    <CounterContext.Provider value={[count, setCount]}>
      {props.children}
    </CounterContext.Provider>
  );
}
//여기 아래
export function useCounter() { return useContext(CounterContext); }
```

위와 같이 컨텍스트 객체의 컨슈머를 export 하고,

```jsx {title="nested.jsx"}
import { useCounter } from "./counter";

export default function Nested() {
  const [count, setCount] = useCounter();
  return (
    <>
      <div>{count()}</div>
      <button onClick={() => setCount(count()+1)}>+</button>
      <button onClick={() => setCount(count()-1)}>-</button>
    </>
  );
};
```

컨텍스트를 사용할 곳에서는 컨슈머를 import하여 사용한다. `useCounter` 호출 시 Provider 컴포넌트의 value로 지정했던 데이터를 얻을 수 있다.

## 문제

TARGET DECK
전체::개발::solid

---

<!--ankiQ-->

counter.jsx

```tsx
import { createSignal } from "solid-js";

export function CounterProvider(props) {
  const [count, setCount] = createSignal(props.count || 0);

  return (
		{props.children}
  );
}
```

app.jsx

```tsx
import { render } from "solid-js/web";
import { CounterProvider } from "./counter";

function App() {
	//이 곳에서 count와 setCount를 쓰고 싶다!
  return <>
    <h1>Welcome to Counter App</h1>
  </>
};

render(() => (
	<App />
), document.getElementById("app"));
```

counter.jsx의 count, setCount 시그널을 전역으로 공유하고 싶다.

Context를 이용하여 위 코드를 수정하라.

<!--ankiA-->

counter.jsx

```tsx
import { createSignal, createContext, useContext } from "solid-js"; //1

const CounterContext = createContext(); //2

export function CounterProvider(props) {
  const [count, setCount] = createSignal(props.count || 0);

  return (
    <CounterContext.Provider value={[count, setCount]}> //3
      {props.children}
    </CounterContext.Provider>
  );
}
//여기 아래
export function useCounter() { return useContext(CounterContext); }
```

1. solid-js 에서 createContext, useContext import
2. 최상위에서 Context 객체 생성
3. 컨텍스트 객체의 Provider로 반환값 감싸고 value 지정
4. useContext export

app.jsx

```tsx
import { render } from "solid-js/web";
import Nested from "./nested";
import { CounterProvider, useCounter } from "./counter"; //1

function App() {
  const [count, setCount] = useCounter(); //2
  return <>
    <h1>Welcome to Counter App</h1>
  </>
};

render(() => (
  <CounterProvider count={1}> //3
    <App />
  </CounterProvider>
), document.getElementById("app"));
```

1. 커스텀 컨텍스트로부터 useCounter import
2. useCounter로 컨텍스트 value 획득
3. CounterProvider로 app을 감싼다

<!--ankiE-->
<!--ID: 1665044963832-->

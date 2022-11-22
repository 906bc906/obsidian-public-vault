---
title: SolidJS - Stores - Create Store
date: 2022-10-10T00:19:52+09:00
last_modified_at: 2022-11-22T20:09:20+09:00
---

https://www.solidjs.com/tutorial/stores_createstore

저장소 생성

중첩된 반응성에 대한 Solid의 대답은 Store 다. Store는 프록시 객체[^프록시]이며, 프록시 내부의 프로퍼티를 추적할 수 있으며 프록시 내부에 자동으로 래핑되는 다른 객체들을 포함한다.


[^프록시]: 객체를 감싸는 객체로, 객체에 가해지는 작업(읽기, 쓰기 등)을 가로채서 사전 정의된 행동을 한다. 

Signal이 하나의 반응성 값을 다룬다면, Store는 주로 반응성 값들의 collection을 다루는데 사용한다. object나 array 가 있겠다.

Store를 가볍게 유지하기 위해, Solid는 추적 범위 내에서 접근하는 프로퍼티에 대해서만 내부적으로 시그널을 생성한다. 즉 스토어의 모든 시그널들은 필요시에만 지연 생성된다.

```tsx
const [todos, setTodos] = createSignal([])
```

createStore 함수 호출시 초기값을 인자로 받아서, 시그널과 비슷하게 읽기/쓰기 튜플을 반환한다. 첫번째 항목은 읽기 전용 스토어 프록시이며, 두 번째 항목은 setter 함수이다.

setter 함수의 가장 기본적인 형식은 인자로 객체를 받으며, 받은 객체의 프로퍼티를 현재 상태에 머지한다. 또한 중첩된 업데이트를 할 수 있도록 path syntax도 지원한다.

이러한 방법으로 반응성에 대한 제어를 유지하면서도 원하는 부분만 선택해 업데이트할 수 있게 되었다.

> Solid의 Path Syntax에는 많은 형식이 있으며, 반복과 범위를 위한 강력한 구문도 포함하고 있다. 전체 레퍼런스를 보려면 [API 문서](https://www.solidjs.com/docs/latest/api#updating-stores)를 참고.

```tsx
  const [todos, setTodos] = createSignal([])
  const addTodo = (text) => {
    setTodos([...todos(), { id: ++todoId, text, completed: false }]);
  }
  const toggleTodo = (id) => {
    setTodos(todos().map((todo) => (
      todo.id !== id ? todo : { ...todo, completed: !todo.completed }
    )));
  }
```

위의 코드를 store를 이용해서 개선하면,

```tsx
const [todos, setTodos] = createStore([]);
const addTodo = (text) => {
  setTodos([...todos, { id: ++todoId, text, completed: false }]);
};
const toggleTodo = (id) => {
  setTodos(
    (todo) => todo.id === id,
    "completed",
    (completed) => !completed
  );
};
```

위와 같다.

## 문제

TARGET DECK
전체::개발::solid

---

<!--ankiQ-->

```tsx
const [state, setState] = createStore({
  counter: 2,
  list: [
    { id: 23, title: 'Birds' }
    { id: 27, title: 'Fish' }
  ]
});
```

위 Solid 코드에서, counter 를 1 증가시키는 코드를 작성하라.

<!--ankiA-->

```tsx
setState('counter', c => c + 1);
```

<!--ankiE-->
<!--ID: 1665038687117-->


---

<!--ankiQ-->

```tsx
const [state, setState] = createStore({
  counter: 2,
  list: [
    { id: 23, title: 'Birds' },
    { id: 27, title: 'Fish' }
  ]
});
```

위 Solid 코드에서, list에 {id: 43, title: 'Marsupials'} 를 추가하는 코드를 작성하라.

<!--ankiA-->

```tsx
setState('list', l => [...l, {id: 43, title: 'Marsupials'}]);
```

<!--ankiE-->
<!--ID: 1665038687160-->

---

<!--ankiQ-->

```tsx
const [state, setState] = createStore({
  counter: 2,
  list: [
    { id: 23, title: 'Birds' },
    { id: 27, title: 'Fish' }
  ]
});
```

위 Solid 코드에서, list의 index 1 (2번째) 요소에 read: true 프로퍼티를 추가하는 코드를 작성하라.

<!--ankiA-->

```tsx
setState('list', 1, 'read', true);
```

<!--ankiE-->
<!--ID: 1665038687168-->

---

<!--ankiQ-->

```tsx
const [state, setState] = createStore({
  todos: [
    { task: 'Finish work', completed: false },
    { task: 'Go grocery shopping', completed: false },
    { task: 'Make dinner', completed: false }
  ]
});
```

위 Solid 코드에서, todos의 index 0, 2 요소의 completed 를 true로 바꾸는 코드를 작성하라.

<!--ankiA-->

```tsx
setState('todos', [0, 2], 'completed', true);
```

<!--ankiE-->
<!--ID: 1665038687173-->

---

<!--ankiQ-->

```tsx
const [state, setState] = createStore({
  todos: [
    { task: 'Finish work', completed: false },
    { task: 'Go grocery shopping', completed: false },    { task: 'Make dinner', completed: false }
  ]
});
```

위 Solid 코드에서, todos의 index 0부터 1까지,  completed 를 토글하는 코드를 작성하라.

<!--ankiA-->

```tsx
setState('todos', { from: 0, to: 1 }, 'completed', c => !c);
```

<!--ankiE-->
<!--ID: 1665038687177-->

---

<!--ankiQ-->

```tsx
const [state, setState] = createStore({
  todos: [
    { task: 'Finish work', completed: false },
    { task: 'Go grocery shopping', completed: true },
    { task: 'Make dinner', completed: false }
  ]
});
```

위 Solid 코드에서,  todos 중 completed 가 true인 요소의 task 문자열의 맨 뒤에 '!' 을 추가하는 코드를 작성하라.

<!--ankiA-->

```tsx
setState('todos', todo => todo.completed, 'task', t => t + '!')
```

<!--ankiE-->
<!--ID: 1665038687181-->

---

<!--ankiQ-->

```tsx
const [state, setState] = createStore({
  todos: [
    { task: 'Finish work', completed: false },
    { task: 'Go grocery shopping', completed: false },
    { task: 'Make dinner', completed: false }
  ]
});
```

위 Solid 코드에서, todos의 모든 요소에 marked: true 속성을 추가하는 코드를 작성하라.

<!--ankiA-->

```tsx
setState('todos', {}, todo => ({ marked: true}))
```

<!--ankiE-->
<!--ID: 1665038687186-->

---

<!--ankiQ-->

```tsx
const [todos, setTodos] = createSignal([])
const addTodo = (text) => {
	setTodos([...todos(), { id: ++todoId, text, completed: false }]);
}
const toggleTodo = (id) => {
	setTodos(todos().map((todo) => (
		todo.id !== id ? todo : { ...todo, completed: !todo.completed }
	)));
}
```

위 Solid 코드는 to do 목록을 관리하기 위해 작성되었으나, completed 토글 시 DOM 엘리먼트를 다시 생성해야 하기 때문에 비효율적이다. 이를 Solid의 store 기능을 이용해 개선한다면 어떻게 할 수 있겠는가?

<!--ankiA-->

```tsx
const [todos, setTodos] = createStore([]);
const addTodo = (text) => {
  setTodos([...todos, { id: ++todoId, text, completed: false }]);
};
const toggleTodo = (id) => {
  setTodos(
    (todo) => todo.id === id,
    "completed",
    (completed) => !completed
  );
};
```

<!--ankiE-->
<!--ID: 1665038687191-->

---
title: SolidJS - Stores - Mutation
date: 2022-10-10T00:19:58+09:00
last_modified_at: 2022-10-10T00:19:58+09:00
---

https://www.solidjs.com/tutorial/stores_mutation

Solid는 상태를 업데이트할 때 얕은 불변 패턴을 사용할 것을 강력하게 권장한다. 읽기와 쓰기를 분리함으로써 컴포넌트 계층들을 통과할 때 프록시에 대한 변경 사항을 잃어버릴 위험 없이 시스템의 반응성을 더 잘 컨트롤할 수 있다. 이 점은 시그널에 비해 스토어에서 훨씬 더 증대된다.

하지만 Mutation이 더 추론하기 쉬운 경우도 있다.[^불변성유지] 그렇기 때문에 Solid는 setStore 호출 내에서 Store 객체의 쓰기 가능한 프록시 버전을 수정할 수 있도록 immer[^immer]에서 영감을 받은 produce store 수정 함수를 제공한다.

[^immer]: React용 불변성 관리 라이브러리. https://github.com/immerjs/immer

[^불변성유지]: 데이터가 복잡하면 복잡할 수록 불변성을 유지하면서 상태를 업데이트하기 위한 코드 작성 또한 복잡해지며, 이럴 때 지역적인 Mutation을 사용하면 코드가 훨씬 간결해진다. 함부로 불변성을 깨고 직접 수정하면 프레임워크가 상태 변화를 인식하지 못할 위험성이 있다. 자세한 내용은 우측 참고. [23. Immer 를 사용한 더 쉬운 불변성 관리 · GitBook](https://react.vlpt.us/basic/23-immer.html)

이는 컨트롤을 포기하지 않으면서도 작은 영역의 mutation을 허용하는 좋은 도구이다.

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

위의 Todo 예제에서 이벤트 핸들러를 다음과 같이 변경해서 produce를 사용하면,

```tsx
const [todos, setTodos] = createStore([]);

const addTodo = (text) => {
  setTodos(
    produce((todos) => {
      todos.push({ id: ++todoId, text, completed: false });
    })
  );
};
const toggleTodo = (id) => {
  setTodos(
    (todo) => todo.id === id,
    produce((todo) => (todo.completed = !todo.completed))
  );
};
```

와 같이 바꿀 수 있다.

## 문제

TARGET DECK
전체::개발::solid

---

<!--ankiQ-->

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

위 Solid 코드를 Produce를 이용해 수정하라.

<!--ankiA-->

```tsx
const [todos, setTodos] = createStore([]);

const addTodo = (text) => {
  setTodos(
    produce((todos) => {
      todos.push({ id: ++todoId, text, completed: false });
    })
  );
};
const toggleTodo = (id) => {
  setTodos(
    (todo) => todo.id === id,
    produce((todo) => (todo.completed = !todo.completed))
  );
};
```

<!--ankiE-->
<!--ID: 1665040051778-->

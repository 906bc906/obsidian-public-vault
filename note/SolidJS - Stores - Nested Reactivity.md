# SolidJS - Stores - Nested Reactivity

<!--Basic Template V0.0.2 Start -->
```dataview
TABLE WITHOUT ID  file.link AS title, tags
FROM -"templates"
SORT file.mtime, tags desc
WHERE contains(tags, [[]])
```
<!--Basic Template V0.0.2 End -->
[tags::[[SolidJS]]]

https://www.solidjs.com/tutorial/stores_nested_reactivity

중첩된 반응성

Solid에서 세분화된 반응성을 제공할 수 있는 이유 중 하나는 중첩된 업데이트를 독립적으로 처리할 수 있기 때문이다. 사용자 리스트가 있고, 이 중의 한 사용자 이름을 업데이트한다고 했을 때, Solid는 리스트 자체의 내용을 비교하지 않으면서 DOM 에 있는 이름의 위치만 업데이트한다. UI 프레임워크 중에서 이런 것이 가능한 프레임워크는 거의 없다.

예를 들어, 일반적인 프레임워크에서는 todo를 완료 상태로 설정하기 위해, todo의 복제를 사용해 기존 항목을 교체한다. 리스트를 비교해서 DOM 엘리먼트를 다시 생성해야하기 때문에 비효율적이다.

```tsx
const [todos, setTodos] = createSignal([])
const addTodo = (text) => {
	setTodos([...todos(), { id: ++todoId, text, completed: false }]);
}
```

```tsx
const toggleTodo = (id) => {
  setTodos(
    todos().map((todo) => (todo.id !== id ? todo : { ...todo, completed: !todo.completed })),
  );
};
```

반면, Solid 는 다음과 같이 중첩된 시그널로 데이터를 초기화한다.

```tsx
const [todos, setTodos] = createSignal([])
const addTodo = (text) => {
  const [completed, setCompleted] = createSignal(false);
  setTodos([...todos(), { id: ++todoId, text, completed, setCompleted }]);
};
```

```tsx
const toggleTodo = (id) => {
  const index = todos().findIndex((t) => t.id === id);
  const todo = todos()[index];
  if (todo) todo.setCompleted(!todo.completed())
}
```

이제 추가 비교 작업 없이, todos 시그널에 추가된 오브젝트의 setCompleted를 호출해서 완료 상태를 업데이트할 수 있다. 이는 복잡한 부분을 뷰에서 데이터로 이동했기 때문에 가능한 것이다. 

## 문제

TARGET DECK
전체::개발::solid

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

위의 solid 코드는 to do 리스트를 관리하기 위해 작성되었다. 이 코드는 solid 에서 지원하는 기능에 비해 비효율적인데, 이를 어떻게 개선할 수 있는가?

단, 이 문제에서 store는 사용할 수 없다.

<!--ankiA-->

```tsx
const [todos, setTodos] = createSignal([])
const addTodo = (text) => {
	const [completed, setCompleted] = createSignal(false); 
	setTodos([...todos(), { id: ++todoId, text, completed, setCompleted }]);
}
const toggleTodo = (id) => {
	const index = todos().findIndex((t) => t.id === id);
	const todo = todos()[index];
	if (todo) todo.setCompleted(!todo.completed())
}
```

중첩된 반응성을 사용한다.

질문의 코드는 completed를 토글할 때 DOM 엘리먼트를 다시 생성해야 해서 비효율적이지만, Solid에서는 중첩된 반응성을 제공하기 때문에 목록에 시그널을 집어넣고 수정하는 방식으로 완료 상태를 업데이트할 수 있다.

<!--ankiE-->
<!--ID: 1665034139955-->

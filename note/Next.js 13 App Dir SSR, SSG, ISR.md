---
title: Next.js 13 App Dir SSR, SSG, ISR
date: 2022-11-22T13:17:05+09:00
last_modified_at: 2022-11-22T20:09:22+09:00
---

```ts
import React from 'react'
import { Todo } from '../../../typings';

type PageProps = {
  params: {
    todoId: string;
  };
};

const fetchTodo = async (todoId: string) => {
  const res = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${todoId}`,
  );

  const todo: Todo = await res.json();
  return todo; 
}

async function TodoPage({ params: { todoId }}: PageProps) {
  const todo = await fetchTodo(todoId);
  return (
    <div className="p-10 bg-yellow-200 border-2 m-2 shadow-lg">
      <p>
        #{todo.id}: {todo.title}
      </p>
      <p>Completed: {todo.completed ? "Yes" : "No"}</p>
      
      <p className="border-t border-black mt-5 text-right">
        By User: {todo.userId}
      </p>
    </div>
  )
}

export default TodoPage
```

현재 위의 코드는 Next.js 13의 App 디렉토리 구조에 배치되었다. Nest.js 13의 App 디렉토리 구조에서는 모든 컴포넌트가 기본적으로 React Server Components, 즉 SSR을 한다.[^default-ssr]

[^default-ssr]: https://beta.nextjs.org/docs/routing/fundamentals#the-app-directory

위 코드를 바탕으로 Next.js 13의 App 디렉토리 구조에서 CSR, SSR, SSG, ISR을 하려면 어떻게 해야되는지 알아본다.

https://beta.nextjs.org/docs/rendering/static-and-dynamic-rendering#using-dynamic-functions

client 컴포넌트와 server 컴포넌트에 대한 내용도 반드시 읽어보도록 한다: https://beta.nextjs.org/docs/rendering/server-and-client-components

## CSR

```ts
'use client';

import React from 'react'
import { Todo } from '../../../typings';
...
```

CSR을 하기 위해서는 `"use client"` 를 코드 최상단에 배치한다. import 전에 해야한다.[^csr]

[^csr]: https://beta.nextjs.org/docs/rendering/server-and-client-components#client-components

이 뒤로 `useState`나 `useEffect` 같은 클라이언트 훅을 사용할 수 있다.

클라이언트 훅이 없는 컴포넌트는 지시문 없이 그대로 두는 것이 가장 좋은데, 다른 클라이언트 컴포넌트에서 가져오지 않을 때 자동으로 서버 컴포넌트로 렌더링될 수 있도록 하기 위해서입니다. 이는 클라이언트측 Javascript를 최소한으로 줄일 수 있게 합니다.[^csr-leaves]

`use client` 선언문은 새로운 리액트 기능으로 소개되었으며 여전히 많은 리액트 서드 파티 패키지들은 Client-only 기능만 사용하는 경우가 많다. 이 경우 [해당 페이지](https://beta.nextjs.org/docs/rendering/server-and-client-components#third-party-packages) 참고

[^csr-leaves]: https://beta.nextjs.org/docs/rendering/server-and-client-components#moving-client-components-to-the-leaves

## SSR

```ts
const fetchTodo = async (todoId: string) => {
  const res = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${todoId}`,
    { cache: "no-cache" }
  );
```

## SSG

```ts
const fetchTodo = async (todoId: string) => {
  const res = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${todoId}`,
    { cache: "force-cache" }
  );
```

### prebuild

```ts
...
export default TodoPage

export async function generateStaticParams() {
  const res = await fetch("https://jsonplaceholder.typicode.com/todos/");
  const todos: Todo[] = await res.json();

  //10 페이지만 prebuild함- 200개씩 API 날렸다가 API 제한 먹는걸 피하기 위해
  const trimmedTodos = todos.splice(0, 10);

	//리턴 형태: [{todoId: }, {todoId: }, ...]
  return trimmedTodos.map((todo) => ({
    todoId: todo.id.toString(),
  }));
}
```

dynamic route 세그먼트를 미리 빌드할 수 있게 해주는 함수.[^gSP]

[^gSP]: https://beta.nextjs.org/docs/api-reference/generate-static-params

### dynamicParams

```ts
import React from 'react'
import { Todo } from '../../../typings';

export const dynamicParams = true; //

type PageProps = {
  params: {
```

generateStaticParams 에 의해 생성되지 않은 dynamic segment 에 방문했을 때의 행동을 정의함.
- true
	- default.
	- 그 때 그 때 생성함.
- false
	- 404 에러 반환.

## ISR

```ts
const fetchTodo = async (todoId: string) => {
  const res = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${todoId}`,
    { next: { revalidate: 60 } }
  );
```

Incremental Static Regeneration

SSR과 SSG의 적절한 중간 느낌. (뇌피셜)

리퀘스트가 들어오면 페이지를 새로 만든다.

위 코드를 기준으로, 만약 새로 페이지를 만든지 60초가 지나지 않았다면, 그 사이에 리퀘스트가 들어와도 새로 페이지를 만들지 않고 만들어놨던 페이지를 그대로 서빙한다.

만약 60초가 지난 경우, 바로 페이지를 재생성하진 않고, 60초가 지난 뒤에 다시 리퀘스트가 온 경우에만 페이지를 재생성하고, 다시 60초를 돌린다.


---
title: SolidJS - Control Flow - Show
date: 2022-10-10T00:15:54+09:00
last_modified_at: 2022-10-10T00:15:54+09:00
---


https://www.solidjs.com/tutorial/flow_show

가장 기본적인 제어 흐름은 조건문이다. Solid 에서는 다음의 세 가지 방법으로 조건문을 사용할 수 있다.
1. 삼항 연산자(`a ? b : c`)
2. 불 표현식 (`a && b`)
3. Show 컴포넌트

## 원본

```tsx
  return (
    <>
      <button onClick={toggle}>Log out</button>
      <button onClick={toggle}>Log in</button>
    </>
  );
```

`loggedIn` 시그널에 따라 Log out과 Log in을 바꿔서 표시하려고 한다. 가장 간단한 것은 내부의 Log out, Log in 텍스트를 `loggedIn`에 따라 바꿔주는 것이겠지만 조건 상 log out 버튼과 log in 버튼이 수행해야 하는 역할이 달라 엘리먼트 단위로 구분해야 한다고 가정하자.

## 삼항 연산자

```tsx
  return (
    <>
    {
      loggedIn()
      ? <button onClick={toggle}>Log out</button>
      : <button onClick={toggle}>Log in</button>
    }
    </>
  );
```

## 불 표현식

```tsx
  return (
    <>
    {
      loggedIn() && <button onClick={toggle}>Log out</button>
    }
    {
      !loggedIn() && <button onClick={toggle}>Log in</button>
    }
    </>
  );
```

## Show 컴포넌트

`solid-js`에서 `Show`를 import 해야 한다.

```tsx
  return (
    <Show
      when={loggedIn()}
      fallback={<button onClick={toggle}>Log in</button>}
    >
      <button onClick={toggle}>Log out</button>
    </Show>
  );
```

fallback prop은 else처럼 동작하며, when에 넘겨진 조건이 false인 경우에만 표시된다.

---
## 문제

TARGET DECK
전체::개발::solid

<!--ankiQ-->
```tsx
  return (
    <>
      <button onClick={toggle}>Log out</button>
      <button onClick={toggle}>Log in</button>
    </>
  );
```

`loggedIn` 시그널에 따라 Log out과 Log in을 바꿔서 표시하려고 한다. 가장 간단한 것은 내부의 Log out, Log in 텍스트를 `loggedIn`에 따라 바꿔주는 것이겠지만 조건 상 log out 버튼과 log in 버튼이 수행해야 하는 역할이 달라 엘리먼트 단위로 구분해야 한다고 가정하자.

이를 삼항 연산자로 해결한다면 어떻게 수정할 수 있겠는가?

<!--ankiA-->

```tsx
  return (
    <>
    {
      loggedIn()
      ? <button onClick={toggle}>Log out</button>
      : <button onClick={toggle}>Log in</button>
    }
    </>
  );
```

<!--ankiE-->
<!--ID: 1664949405066-->


<!--ankiQ-->
```tsx
  return (
    <>
      <button onClick={toggle}>Log out</button>
      <button onClick={toggle}>Log in</button>
    </>
  );
```

`loggedIn` 시그널에 따라 Log out과 Log in을 바꿔서 표시하려고 한다. 가장 간단한 것은 내부의 Log out, Log in 텍스트를 `loggedIn`에 따라 바꿔주는 것이겠지만 조건 상 log out 버튼과 log in 버튼이 수행해야 하는 역할이 달라 엘리먼트 단위로 구분해야 한다고 가정하자.

이를 Show 컴포넌트로 해결한다면 어떻게 수정할 수 있겠는가?

<!--ankiA-->

```tsx
  return (
    <Show
      when={loggedIn()}
      fallback={<button onClick={toggle}>Log in</button>}
    >
      <button onClick={toggle}>Log out</button>
    </Show>
  );
```

<!--ankiE-->
<!--ID: 1664949405098-->

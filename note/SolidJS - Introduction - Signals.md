---
title: SolidJS - Introduction - Signals
date: 2022-10-10T00:08:22+09:00
last_modified_at: 2022-10-10T00:08:22+09:00
---

https://www.solidjs.com/tutorial/introduction_signals

Signal은 변경되는 값을 포함한다. 시그널의 값을 변경하면, 해당 시그널을 사용하는 모든 것들이 자동으로 업데이트 된다. 즉, 시그널 값의 변경이 시그널을 구독한 요소들에 전파되므로 반응성의 토대가 된다고 말할 수 있다.

시그널을 생성하려면, `solid-js` 에서 `createSignal` 을 import 해야 한다.

```ts
const [count, setCount] = createSignal(0);
```
count는 getter, setCount는 setter, createSignal 의 인자인 0 은 초기값을 의미한다. createSignal의 첫번째 반환값인 count가 변수가 아니라 값을 반환하는 함수임에 유의한다.

JSX에서 count 를 이용하여 값을 받아오는 부분은, 추후에 setCount로 시그널의 값이 변경되었을 때 업데이트 된다.

---

## 문제

TARGET DECK
전체::개발::solid

<!--ankiQ-->

1/ solid의 signal 을 만들어라.

getter 의 이름은 count,

setter의 이름은 setCount,

시그널의 초기값은 0.

2/ setInterval을 이용하여 count 시그널의 값을 매초마다 1씩 증가하게 하라.

<!--ankiA-->

```ts
const [count, setCount] = createSignal(0);
setInterval(() => setCount(count()+1), 1000);
```

<!--ankiE-->
<!--ID: 1664944861074-->

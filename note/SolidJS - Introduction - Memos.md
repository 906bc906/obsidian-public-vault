---
title: SolidJS - Introduction - Memos
date: 2022-10-10T00:09:50+09:00
last_modified_at: 2022-10-10T00:09:50+09:00
---


https://www.solidjs.com/tutorial/introduction_memos

때로는 중복 작업을 줄이기 위해 값을 캐시할 필요가 있다. 이때 메모(Memo)를 사용하여 함수 실행 결과를 디펜던시가 변경될 때까지 캐시할 수 있다. 다른 디펜던시가 있는 이펙트에 대한 계산을 캐싱하고, DOM 노드 생성과 같은 값비싼 작업에 필요한 작업을 완화하는데 도움이 된다.

메모는 이펙트와 같은 옵저버이면서, 읽기 전용 시그널이다. Memo는 자신의 디펜던시와 옵저버를 모두 알고 있기 때문에, 변경이 일어난 경우 한 번만 실행되도록 할 수 있다. _이 때문에 시그널에 이펙트 설정을 등록하는 것이 좋다._??

`solid-js`에서 `createMemo`를 import하여 메모를 생성할 수 있다.

```ts
const fib = () => fibonacci(count());

  return (
    <>
      <button onClick={() => setCount(count() + 1)}>Count: {count()}</button>
      <div>1. {fib()} {fib()} {fib()} {fib()} {fib()}</div>
      <div>2. {fib()} {fib()} {fib()} {fib()} {fib()}</div>
      <div>3. {fib()} {fib()} {fib()} {fib()} {fib()}</div>
      <div>4. {fib()} {fib()} {fib()} {fib()} {fib()}</div>
      <div>5. {fib()} {fib()} {fib()} {fib()} {fib()}</div>
      <div>6. {fib()} {fib()} {fib()} {fib()} {fib()}</div>
      <div>7. {fib()} {fib()} {fib()} {fib()} {fib()}</div>
      <div>8. {fib()} {fib()} {fib()} {fib()} {fib()}</div>
      <div>9. {fib()} {fib()} {fib()} {fib()} {fib()}</div>
      <div>10. {fib()} {fib()} {fib()} {fib()} {fib()}</div>
    </>
  );
```

위의 반환문은 파생된 시그널인 fib 를 50번 불러오고 있다. fib는 count를 구독하고 있어, count 시그널이 변경될 때마다 fib가 50번 실행될 것이다. 이때 연산을 줄이기 위해 memo로 값을 캐싱하여, count 시그널이 업데이트 된 최초의 1번에만 계산되고 나머지는 캐싱된 값을 쓰도록 할 수 있다.

```ts
const fib = createMemo(() => fibonacci(count()));
```

---

## 문제

TARGET DECK
전체::개발::solid

<!--ankiQ-->

```ts
const fib = () => fibonacci(count());
```

위 함수는 count 시그널을 구독하고 있는데, fib 함수를 이곳 저곳에서 굉장히 많이 불러 중복 연산되고 있다.

count 시그널이 업데이트 된 최초의 1번에만 계산하고 그 뒤로는 처음 계산 값을 그대로 사용해도 된다면, 코드를 어떻게 수정하여 중복 연산을 줄일 수 있겠는가?

<!--ankiA-->

```ts
const fib = createMemo(() => fibonacci(count()));
```

<!--ankiE-->
<!--ID: 1664946251797-->

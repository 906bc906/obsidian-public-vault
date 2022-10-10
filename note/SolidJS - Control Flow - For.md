---
title: SolidJS - Control Flow - For
date: 2022-10-10T00:16:06+09:00
last_modified_at: 2022-10-10T19:12:35+09:00
---


https://www.solidjs.com/tutorial/flow_for

For 컴포넌트를 사용해 객체 배열에 대해 반복을 편하게 할 수 있다. 

```tsx
    <ul>
      <li>
        <a target="_blank" href="">
          1: Garfield
        </a>
      </li>
    </ul>
```

위 코드에서 li 컴포넌트를 cats() 로 전개하고 싶다면,

```tsx
<For each={cats()}>{(cat, i) =>
  <li>
    <a target="_blank" href={`https://www.youtube.com/watch?v=${cat.id}`}>
      {i() + 1}: {cat.name}
    </a>
  </li>
}</For>
```

이렇게 수정할 수 있다.

주의해야 하는 점은 다음과 같다.
1. 인덱스는 상수가 아닌 Signal 이다. (`i()`)
2. each prop은 배열 타입만 사용해야 한다.
	1. iterable 객체를 배열로 바꾸려면
		1. Object.keys
		2. Array.from
		3. 스프레드 구문 (`...`)

---

## 문제

TARGET DECK
전체::개발::solid

<!--ankiQ-->

```tsx
    <ul>
      <li>
        <a target="_blank" href="">
          1: Garfield
        </a>
      </li>
    </ul>
```

위 solid tsx 코드에서 li 컴포넌트를 cats() 로 전개하고 싶다면 어떻게 해야겠는가?

cats()는 배열임이 보장되어있다.

Index 컴포넌트는 사용하지 않는다.

<!--ankiA-->

```tsx
<For each={cats()}>{(cat, i) =>
  <li>
    <a target="_blank" href={`https://www.youtube.com/watch?v=${cat.id}`}>
      {i() + 1}: {cat.name}
    </a>
  </li>
}</For>
```

인덱스 i 가 상수가 아니라 시그널로 쓰여지고 있음에 주의한다.

<!--ankiE-->
<!--ID: 1664950271994-->

---

<!--ankiQ-->

solid의 For 컴포넌트의 each prop에는 배열만 올 수 있다. Iterable 객체를 each Prop에 넣고 싶은 경우 어떤 해결 방안이 있겠는가?

<!--ankiA-->

1. Array.from
2. Object.keys
3. Spread Operator

<!--ankiE-->
<!--ID: 1664950272011-->


<!--Basic Template V0.0.2 Start -->
```dataview
TABLE WITHOUT ID  file.link AS title, tags
FROM -"templates"
SORT file.mtime, tags desc
WHERE contains(tags, [[]])
```
<!--Basic Template V0.0.2 End -->
[tags::[[SolidJS]]]

https://www.solidjs.com/tutorial/flow_index

Index 컴포넌트는 For 컴포넌트와 유사하게 객체 배열에 대한 반복을 위해 사용된다.

단, Index 컴포넌트가 For 컴포넌트와 다른 점은 아래와 같다.
1. 반환되는 컴포넌트 전체가 다시 렌더링되는 For과 달리 Index는 컴포넌트에서 item을 참조하는 부분만 변경된다.
2. For 컴포넌트와는 정반대로 index 가 고정된 상수 값이고, item이 시그널이다.

For 컴포넌트는 배열의 각 데이터 조각에 대해 관심을 가지며, 해당 데이터의 위치가 변경될 수 있다. Index 컴포넌트는 배열의 각 인덱스에 관심을 가지며, 각 인덱스의 내용이 변경될 수 있다.

```tsx
<For each={cats()}>{(cat, i) =>
  <li>
    <a target="_blank" href={`https://www.youtube.com/watch?v=${cat.id}`}>
      {i() + 1}: {cat.name}
    </a>
  </li>
}</For>
```

```tsx
<Index each={cats()}>{(cat, i) =>
  <li>
    <a target="_blank" href={`https://www.youtube.com/watch?v=${cat().id}`}>
      {i + 1}: {cat().name}
    </a>
  </li>
}</Index>
```

For 컴포넌트와 굉장히 유사한 반면 item 과 index 의 성질이 정반대임을 알 수 있다.

## 참고

[solid js - SolidJS: For vs Index - Stack Overflow](https://stackoverflow.com/questions/70819075/solidjs-for-vs-index)

[solid-docs/faq.md at main · solidjs/solid-docs](https://github.com/solidjs/solid-docs/blob/main/langs/en/guides/faq.md#why-shouldnt-i-use-map-in-my-template-and-whats-the-difference-between-for-and-index)

## 문제
TARGET DECK
전체::개발::solid

---

<!--ankiQ-->

Solid의 For 컴포넌트와 Index 컴포넌트는 each prop에 있는 배열을 반복한다는 점에서 유사하나, 아이템 변경 시 렌더링 관련하여 차이가 있다. 어떤 차이인가?

<!--ankiA-->

For 컴포넌트는 반환되는 엘리먼트 전체가 재 렌더링되고, Index 컴포넌트는 엘리먼트는 유지한 채  Item 에 의해 변경되는 부분만 수정한다.

<!--ankiE-->
<!--ID: 1664953255292-->

---

<!--ankiQ-->

```tsx
<For each={cats()}>{(cat, i) =>
  <li>
    <a target="_blank" href={`https://www.youtube.com/watch?v=${cat.id}`}>
      {i() + 1}: {cat.name}
    </a>
  </li>
}</For>
```

위 Solid For 컴포넌트 코드를 Index 컴포넌트로 수정한다면 어떻게 해야겠는가?

<!--ankiA-->

```tsx
<Index each={cats()}>{(cat, i) =>
  <li>
    <a target="_blank" href={`https://www.youtube.com/watch?v=${cat().id}`}>
      {i + 1}: {cat().name}
    </a>
  </li>
}</Index>
```

cats -> cats()

i() -> i

<!--ankiE-->
<!--ID: 1664953255323-->

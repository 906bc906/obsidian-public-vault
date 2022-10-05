<!--Basic Template V0.0.2 Start -->
```dataview
TABLE WITHOUT ID  file.link AS title, tags
FROM -"templates"
SORT file.mtime, tags desc
WHERE contains(tags, [[]])
```
<!--Basic Template V0.0.2 End -->
[tags::[[SolidJS]]]

https://www.solidjs.com/tutorial/flow_switch

때로는 2개 이상의 상호 배타적 결과가 있는 조건문을 처리해야 한다. 이런 경우를 위해 switch/case를 본떠 만든 Switch, Match 컴포넌트가 있다.

각 조건을 비교해서 조건식이 true인 첫 번째 항목을 렌더링한다. 모두 매칭되지 않을 경우 fallback을 렌더링한다.

```tsx
<Show
  when={x() > 10}
  fallback={
	<Show
	  when={5 > x()}
	  fallback={<p>{x()} is between 5 and 10</p>}
>
	  <p>{x()} is less than 5</p>
	</Show>
  }
>
  <p>{x()} is greater than 10</p>
</Show>
```

위 코드의 경우 아래와 같이 바꿀 수 있다.

```tsx
<Switch fallback={<p>{x()} is between 5 and 10</p>}>
  <Match when={x() > 10}>
    <p>{x()} is greater than 10</p>
  </Match>
  <Match when={5 > x()}>
    <p>{x()} is less than 5</p>
  </Match>
</Switch>
```

## 문제

TARGET DECK
전체::개발::solid

---

<!--ankiQ-->

```tsx
<Show
  when={x() > 10}
  fallback={
	<Show
	  when={5 > x()}
	  fallback={<p>{x()} is between 5 and 10</p>}
>
	  <p>{x()} is less than 5</p>
	</Show>
  }
>
  <p>{x()} is greater than 10</p>
</Show>
```

위 solid Show 코드를 Switch/Match 컴포넌트를 이용하여 수정하시오.

<!--ankiA-->

```tsx
<Switch fallback={<p>{x()} is between 5 and 10</p>}>
  <Match when={x() > 10}>
    <p>{x()} is greater than 10</p>
  </Match>
  <Match when={5 > x()}>
    <p>{x()} is less than 5</p>
  </Match>
</Switch>
```

<!--ankiE-->
<!--ID: 1664953517827-->

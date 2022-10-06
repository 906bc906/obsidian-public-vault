<!--Basic Template V0.0.2 Start -->
```dataview
TABLE WITHOUT ID  file.link AS title, tags
FROM -"templates"
SORT file.mtime, tags desc
WHERE contains(tags, [[]])
```
<!--Basic Template V0.0.2 End -->
[tags::[[SolidJS]]]

https://www.solidjs.com/tutorial/bindings_classlist

Solid는 class를 사용해 엘리먼트의 className 프로퍼티를 설정할 수 있다. 하지만 조건문을 사용해 class를 설정하는 것이 편한 경우가 많은데, 이럴 때 Solid는 키가 class 이름이고 값이 boolean 표현식인 객체를 받는 빌트인 classList JSX 속성을 제공한다. 표현식이 true인 경우 class가 적용되며, false인 경우 삭제된다.

```tsx
<button
  class={current() === 'foo' ? 'selected' : ''}
  onClick={() => setCurrent('foo')}
>foo</button>
```

위의 코드를 classList로 변경한다면

```tsx
<button
  classList={{selected: current() === 'foo'}}
  onClick={() => setCurrent('foo')}
>foo</button>
```

## 문제

TARGET DECK
전체::개발::solid

---

<!--ankiQ-->

```tsx
<button
  class={current() === 'foo' ? 'selected' : ''}
  onClick={() => setCurrent('foo')}
>foo</button>
```

위의 Solid 코드에서 class=~ 부분을 ClassList를 이용하여 수정하라.

<!--ankiA-->

```tsx
<button
  classList={{selected: current() === 'foo'}}
  onClick={() => setCurrent('foo')}
>foo</button>
```

<!--ankiE-->
<!--ID: 1664961429246-->

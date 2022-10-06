# SolidJS - Bindings - Style
<!--Basic Template V0.0.2 Start -->
```dataview
TABLE WITHOUT ID  file.link AS title, tags
FROM -"templates"
SORT file.mtime, tags desc
WHERE contains(tags, [[]])
```
<!--Basic Template V0.0.2 End -->
[tags::[[SolidJS]]]

https://www.solidjs.com/tutorial/bindings_style

Solid의 style 속성은 스타일 문자열이나 객체를 받는다.

```tsx
<div style={`color: green; background-color: ${state.color}; height: ${state.height}px`} />
```

문자열로 위와 같이 지정할 수 있다.

```tsx
<div style={{
  color: "green",
  "background-color": state.color,
  height: state.height + "px" }}
/>
```

위와 같이 객체로도 지정할 수 있다. 단 객체로 지정할 때 -로 구분된 속성 이름은 따옴표로 묶어야 한다.

```tsx
<div style={{ "--my-custom-color": state.themeColor }} />
```

CSS 변수도 설정할 수 있다.

backgroundColor 와 같은 자바스크립트 카멜 케이스 버전이 아닌 `background-color` 와 같은 소문자이면서 -로 구분된 속성 이름을 사용한다.

## 문제

TARGET DECK
전체::개발::solid

---

<!--ankiQ-->

```tsx
<div>Some Text</div>;
```

위의 Solid div 엘리먼트를 color: red, font-size: 18px 로 스타일링하시오.

<!--ankiA-->

```tsx
<div style={{color:"red", "font-size":18+"px"}}>Some Text</div>;
```

<!--ankiE-->
<!--ID: 1664961205088-->

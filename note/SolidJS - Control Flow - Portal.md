---
title: SolidJS - Control Flow - Portal
date: 2022-10-10T00:17:16+09:00
last_modified_at: 2022-10-26T01:11:31+09:00
---

https://www.solidjs.com/tutorial/flow_portal

때로는 앱의 정상적인 플로우 외부에 엘리먼트를 삽입하는 것이 더 좋은 경우가 있다. Z-index 를 이용할 수도 있지만 때때로 모달과 같은 플로팅 엘리먼트에 대한 렌더링 컨텍스트를 처리하기에 부족한 경우가 있다.

선택한 위치에 자식 콘텐츠가 삽입되는 [Portal 컴포넌트](https://www.solidjs.com/docs/latest/api#portal)가 있다. 기본적으로 해당 엘리먼트는 document.body의 div 에 렌더링 된다.

사용할 수 있는 Prop은 다음과 같다.
- mount?
	- 자식 엘리먼트를 마운트할 노드를 지정한다.
	- `mount={document.getElementByID("modal")}`
- useShadow?
	- 스타일 격리를 위해 섀도루 루트에 엘리먼트를 배치한다.
- isSVG?
	- SVG 엘리먼트에 삽입하는 경우 기본적으로 div 로 감싸 삽입하는 것이 문제가 될 수 있음
	- isSVG 를 이용하여 div 삽입을 방지할 수 있다
- children
	- Portal 태그로 감싼 자식 엘리먼트

```tsx
<Portal>
  <div class="popup">
    <h1>Popup</h1>
    <p>Some text you might need for something or other.</p>
  </div>
</Portal>
```

## 문제

TARGET DECK
전체::개발

---

<!--ankiQ-->

```tsx
<div class="app-container">
  <p>Just some text inside a div that has a restricted size.</p>
  <div class="popup">
	<h1>Popup</h1>
	<p>Some text you might need for something or other.</p>
  </div>
</div>
);
```

위 solid 코드에서 div#popup 은 일반적인 flow에서 벗어나 body에 배치하고 싶다면 어떻게 수정해야 겠는가?

<!--ankiA-->

```tsx
<div class="app-container">
  <p>Just some text inside a div that has a restricted size.</p>
  <Portal>
	<div class="popup">
	  <h1>Popup</h1>
	  <p>Some text you might need for something or other.</p>
	</div>
  </Portal>
</div>
```

document.body.div 에 배치된다.

<!--ankiE-->
<!--ID: 1664955746016-->

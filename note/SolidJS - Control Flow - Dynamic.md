---
title: SolidJS - Control Flow - Dynamic
date: 2022-10-10T00:16:34+09:00
last_modified_at: 2022-10-10T19:12:35+09:00
---


https://www.solidjs.com/tutorial/flow_dynamic

많은 엘리먼트 들 중에서, 값에 따라 하나의 엘리먼트를 선택해서 보여줘야 하는 경우, Show 컴포넌트나 Switch/Match 컴포넌트를 이용하는 것은 코드를 굉장히 길어지게 한다.

네이티브 엘리먼트 문자열이나 컴포넌트 함수를 전달하는 환경이라면 Dynamic 컴포넌트를 이용하여 이를 단순화할 수 있다.

```tsx
const RedThing = () => <strong style="color: red">Red Thing</strong>;
const GreenThing = () => <strong style="color: green">Green Thing</strong>;
const BlueThing = () => <strong style="color: blue">Blue Thing</strong>;

const options = {
  red: RedThing,
  green: GreenThing,
  blue: BlueThing
}
```

위와 같이 조건에 따라 출력해야 하는 컴포넌트 목록이 있고,

```tsx
  <select onInput={e => setSelected(e.currentTarget.value)}>
	<For each={Object.keys(options)}>{
	  color => <option value={color}>{color}</option>
	}</For>
  </select>
```

select 를 통해 렌더링할 컴포넌트를 지정하는 환경이라면,

```tsx
  <Switch fallback={<BlueThing />}>
	<Match when={selected() === "red"} ><RedThing /></Match>
	<Match when={selected() === "green"}><GreenThing /></Match>
  </Switch>
```

위처럼 Switch, Match 를 쓰는 것은 간결하지 않다.

Dynamic 컴포넌트를 사용한다면

```tsx
<Dynamic component={options[selected()]} />
```

와 같이 수정할 수 있다.

## 문제

TARGET DECK
전체::개발::solid

---

<!--ankiQ-->

```tsx
const RedThing = () => <strong style="color: red">Red Thing</strong>;
const GreenThing = () => <strong style="color: green">Green Thing</strong>;
const BlueThing = () => <strong style="color: blue">Blue Thing</strong>;

const options = {
  red: RedThing,
  green: GreenThing,
  blue: BlueThing
}

...

  <select onInput={e => setSelected(e.currentTarget.value)}>
	<For each={Object.keys(options)}>{
	  color => <option value={color}>{color}</option>
	}</For>
  </select>

  <Switch fallback={<BlueThing />}>
	<Match when={selected() === "red"} ><RedThing /></Match>
	<Match when={selected() === "green"}><GreenThing /></Match>
  </Switch>

```

위의 solid 코드는 select 한 값으로 지정된 컴포넌트를 렌더링하려는 목적으로 작성되었다.

어떤 컴포넌트를 이용하면 Switch/Match 컴포넌트를 간소화할 수 있다. 어떻게 작성해야겠는가?

<!--ankiA-->

```tsx
<Dynamic component={options[selected()]} />
```

<!--ankiE-->
<!--ID: 1664954231227-->

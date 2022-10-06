# SolidJS - Bindings - Spreads
<!--Basic Template V0.0.2 Start -->
```dataview
TABLE WITHOUT ID  file.link AS title, tags
FROM -"templates"
SORT file.mtime, tags desc
WHERE contains(tags, [[]])
```
<!--Basic Template V0.0.2 End -->
[tags::[[SolidJS]]]

https://www.solidjs.com/tutorial/bindings_spreads

때로는 컴포넌트와 엘리먼트에서 다수의 속성을 받아서 아래쪽으로 전달할 때, 하나씩 전달하기보다 객체로 전달하는 것이 합리적이다. 디자인 시스템을 만들 때 일반적인 관행인 컴포넌트 안에서 DOM 엘리먼트를 래핑할 때 특히 그렇다.

```tsx
const pkg = {
  name: "solid-js",
  version: 1,
  speed: "⚡️",
  website: "https://solidjs.com",
};

function App() {
  return (
    <Info
      name={pkg.name}
      version={pkg.version}
      speed={pkg.speed}
      website={pkg.website}
    />
  );
}
```

이런 경우 스프레드 연산자(`...`)를 사용한다.

```tsx
const pkg = {
  name: "solid-js",
  version: 1,
  speed: "⚡️",
  website: "https://solidjs.com",
};

function App() {
  return <Info {...pkg} />;
}
```

## 문제

TARGET DECK
전체::개발::solid

---

<!--ankiQ-->

```tsx
const pkg = {
  name: "solid-js",
  version: 1,
  speed: "⚡️",
  website: "https://solidjs.com",
};

function App() {
  return (
    <Info
      name={pkg.name}
      version={pkg.version}
      speed={pkg.speed}
      website={pkg.website}
    />
  );
}
```

위 Solid 코드에서 Info 컴포넌트에 속성을 전달하는 것을 더 단순화할 수 있다. 어떻게 하면 되는가?

<!--ankiA-->

```tsx
const pkg = {
  name: "solid-js",
  version: 1,
  speed: "⚡️",
  website: "https://solidjs.com",
};

function App() {
  return <Info {...pkg} />;
}
```

스프레드 연산자를 사용한다.

<!--ankiE-->
<!--ID: 1664963605046-->


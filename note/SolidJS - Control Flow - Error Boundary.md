# SolidJS - Control Flow - Error Boundary
<!--Basic Template V0.0.2 Start -->
```dataview
TABLE WITHOUT ID  file.link AS title, tags
FROM -"templates"
SORT file.mtime, tags desc
WHERE contains(tags, [[]])
```
<!--Basic Template V0.0.2 End -->
[tags::[[SolidJS]]]

https://www.solidjs.com/tutorial/flow_error_boundary

UI에서 발생하는 자바스크립트 오류로 전체 앱이 중단되면 안 된다. ErrorBoundary 컴포넌트는 자식 컴포넌트 트리에서 발생하는 자바스크립트 에러를 캐치해서, 에러를 기록하고, 충돌이 발생한 컴포넌트 트리 대신 fallback UI를 표시한다.

```tsx
const Broken = (props) => {
  throw new Error("Oh No");
  return <>Never Getting Here</>
}

...

<ErrorBoundary fallback={err => err}>
  <Broken />
</ErrorBoundary>
```

## 문제

TARGET DECK
전체::개발::solid

---

<!--ankiQ-->
```tsx
const Broken = (props) => {
  throw new Error("Oh No");
  return <>Never Getting Here</>
}

function App() {
  return (
    <>
      <div>Before</div>
      <Broken />
      <div>After</div>
    </>
  );
}
```

위의 Broken 컴포넌트는 무조건 에러가 발생한다. 따라서 App 컴포넌트는 렌더링이 중단될 것이다.

Broken 컴포넌트에서 발생한 에러를 캐치해서 그대로 italic 엘리먼트로 감싸 출력하고 싶다면 어떻게 해야겠는가?

<!--ankiA-->

```tsx
  <div>Before</div>
	<ErrorBoundary fallback={err => (<i>{err.toString()}</i>)}>
	  <Broken />
	</ErrorBoundary>
  <div>After</div>
```

<!--ankiE-->
<!--ID: 1664956299102-->

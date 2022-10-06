<!--Basic Template V0.0.2 Start -->
```dataview
TABLE WITHOUT ID  file.link AS title, tags
FROM -"templates"
SORT file.mtime, tags desc
WHERE contains(tags, [[]])
```
<!--Basic Template V0.0.2 End -->
[tags::[[SolidJS]]]

https://www.solidjs.com/tutorial/introduction_components

코드를 컴포넌트 단위로 분리하면 모듈화와 재사용성을 챙길 수 있다.

컴포넌트는 HelloWrold와 같은 함수이나, JSX를 반환하고, 다른 컴포넌트의 JSX에 의해 호출될 수 있다.

```ts
import Nested from "./nested";

function App() {
  return (
    <>
      <h1>This is a Header</h1>
      <Nested />
    </>
  );
}

//./nested
export default () => <p>This is a Paragraph</p>
```


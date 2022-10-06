<!--Basic Template V0.0.2 Start -->
```dataview
TABLE WITHOUT ID  file.link AS title, tags
FROM -"templates"
SORT file.mtime, tags desc
WHERE contains(tags, [[]])
```
<!--Basic Template V0.0.2 End -->
[tags::[[SolidJS]]]

https://www.solidjs.com/tutorial/lifecycles_onmount

onMount는 초기 렌더링이 완료되면 컴포넌트 당 한 번만 실행된다.

라이프사이클은 브라우저에서만 실행되므로, onMount에 있는 코드는 SSR 동산 서버에서 실행되지 않는다.

```tsx
const [photos, setPhotos] = createSignal([]);
onMount(async () => {
  const res = await fetch(`https://jsonplaceholder.typicode.com/photos?_limit=20`);
  setPhotos(await res.json());
});
```

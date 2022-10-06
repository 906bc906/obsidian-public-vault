# SolidJS - Stores - Immutable Stores

<!--Basic Template V0.0.2 Start -->
```dataview
TABLE WITHOUT ID  file.link AS title, tags
FROM -"templates"
SORT file.mtime, tags desc
WHERE contains(tags, [[]])
```
<!--Basic Template V0.0.2 End -->
[tags::[[SolidJS]], [[to do]]]

https://www.solidjs.com/tutorial/stores_immutable

저장소는 Solid의 Store 프록시를 사용해 Solid에서 가장 자주 생성된다. 때로는 Redux, Apollo, XState와 같은 변경 불가능한 라이브러리와 인터페이스하고 싶은 경우가 있으며, 이에 대해 세분화된 업데이트를 수행해야 한다.

reconcile 동작. 지금 당장은 이해할 수 없는 내용이라 생략함

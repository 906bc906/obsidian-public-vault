# SolidJS - Introduction - Derived Signals
<!--Basic Template V0.0.2 Start -->
```dataview
TABLE WITHOUT ID  file.link AS title, tags
FROM -"templates"
SORT file.mtime, tags desc
WHERE contains(tags, [[]])
```
<!--Basic Template V0.0.2 End -->
[tags::[[SolidJS]]]

https://www.solidjs.com/tutorial/introduction_derived

Derived signals : 파생된 시그널

시그널을 함수로 래핑하여 시그널에 의존하는 새로운 표현식을 생성할 수 있다. 시그널에 액세스하는 함수도 시그널이기 때문에, 래핑된 시그널이 변경되면 시그널에 의존하는 다른 시그널도 업데이트한다. 이를 파생된 시그널이라고 부른다.

```ts
const doubleCount = () => count() * 2;
```

이렇게 count 시그널에 의존하는 새로운 표현식 doubleCount를 만들고,

```ts
return <div>Count: {doubleCount()}</div>;
```

JSX에서 시그널처럼 doubleCount를 호출할 수 있다.


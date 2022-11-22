---
title: SolidJS의 Router에서 a 태그와 Link 컴포넌트의 구분
date: 2022-10-07T16:18:32+09:00
last_modified_at: 2022-11-22T20:09:20+09:00
---
SolidJS의 Router 문서를 보면 `Link`, `NavLink` 컴포넌트를 이용하라고 적혀있고, `a` 태그에 대한 내용은 없다.

Solid-router 의 컴포넌트 소스를 보면,

```tsx
export function Link(props: LinkProps) {
  const to = useResolvedPath(() => props.href);
  return <LinkBase {...props} to={to()} />;
}

function LinkBase(props: LinkBaseProps) {
  const [, rest] = splitProps(props, ["children", "to", "href", "state"]);
  const href = useHref(() => props.to);

  return (
    <a {...rest} href={href() || props.href} state={JSON.stringify(props.state)}>
      {props.children}
    </a>
  );
}
```

state를 전달하는 것을 제외하면 a링크 태그와 다를 것이 없음을 알 수 있다.

`noScroll, replace` 같은 `Link` 컴포넌트만의 속성들도 그대로 a 태그에 전달되며, 실제로 `Link` 컴포넌트가 렌더링 된 모습도 a 태그와 거의 동일하다.

React에서는 a 태그와 `Link` 컴포넌트가 많이 달라 주의해서 써야 하는 것 같지만, 적어도 Solid 에서는 그렇게 엄격하게 구분할 필요가 없어보이며, 앱 내부 경로로 이동할 때 `Link` 컴포넌트, 앱 외부 경로로 이동할 때 a 태그를 쓰면 될듯.


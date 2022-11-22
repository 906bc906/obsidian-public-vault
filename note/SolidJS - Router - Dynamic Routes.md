---
title: SolidJS - Router - Dynamic Routes
date: 2022-10-10T00:32:00+09:00
last_modified_at: 2022-11-22T20:09:20+09:00
---

https://github.com/solidjs/solid-router#dynamic-routes

```tsx
    <Routes>
      <Route path="/users" component={Users} />
      <Route path="/users/:id" component={User} /> //<--
```

위의 경우처럼 경로에 매개 변수를 취급하여 컴포넌트에 전달할 수 있습니다.

콜론 뒤에 나타나는 id는 문자열이 매칭됩니다.

```tsx
import { fetchUser } ...
im[SolidJS - Router - Dynamic Routes](SolidJS%20-%20Router%20-%20Dynamic%20Routes.md)outer" ...

export default function User () {
  const params = useParams();
  const [userData] = createResource(() => params.id, fetchUser);
  return <a href={userData.twitter}>{userData.name}</a>
}
```

`useParams`를 이용하여 경로로부터 전달받은 매개 변수를 사용할 수 있습니다. 위 경우 `Route` 에서 `id` 매개 변수를 사용하였고, `useParams` 로 매개 변수를 얻은 다음 `id`를 꺼내서 쓰고 있습니다.

## 선택적 매개변수

```tsx
//Matches stories and stories/123 but not stories/123/comments
<Route path='/stories/:id?' element={<Stories/>} />
```

매개 변수의 이름 뒤에 물음표를 추가하여 해당 매개변수가 선택적임을 나타낼 수 있습니다.

## 와일드카드 경로

```tsx
//Matches any path that begins with foo, including foo/, foo/a/, foo/a/b/c
<Route path='foo/*' component={Foo}/>
```

경로의 끝에 `*` 를 추가하여 지정한 주소까지만 일치한다면 그 이후로 어떤 주소든 매칭되도록 할 수 있습니다.

```tsx
<Route path='foo/*any' element={<div>{useParams().any}</div>}/>
// foo/123/4/2432/ => useParams().any = "123/4/2432"
```

경로의 와일드 파트를 매개 변수로 컴포넌트에 전달하고 싶다면 위와 같이 이름을 붙일 수 있습니다.

와일드카드 토큰은 반드시 경로의 뒤에 와야 하며, `foo/*any/bar` 와 같이 사용한다면 어떠한 경로도 생성되지 않을 것입니다.

## 복수의 경로

배열을 이용하여 여러 경로를 지정할 수도 있습니다. 이렇게 지정한 경로들간에는 이동할 때 리렌더링되지 않습니다. (브라우저에서 경로를 직접 입력하는 경우에는 리렌더링됩니다)


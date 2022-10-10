---
title: SolidJS - Router - Create Links to Your Routes
date: 2022-10-10T00:31:48+09:00
last_modified_at: 2022-10-10T19:12:00+09:00
---

https://github.com/solidjs/solid-router#create-links-to-your-routes

`Link` 컴포넌트를 이용하여 해당 경로로 이동하는 a 태그를 생성하세요.

[[SolidJS의 Router에서 a 태그와 Link 컴포넌트의 구분]]

[react-router의 Link를 쓰지 마세요! | blog.hoseung.me](https://blog.hoseung.me/2021-12-07-do-not-use-link/)

```tsx
import { lazy } from "solid-js";
import { Routes, Route, Link } from "@solidjs/router"
const Users = lazy(() => import("./pages/Users"));
const Home = lazy(() => import("./pages/Home"));

export default function App() {
  return <>
    <h1>My Site with Lots of Pages</h1>
    <nav>
      <Link href="/about">About</Link>
      <Link href="/">Home</Link>
    </nav>
    <Routes>
      <Route path="/users" component={Users} />
      <Route path="/" component={Home} />
      <Route path="/about" element={<div>This site was made with Solid</div>} />
    </Routes>
  </>
}
```

`Link` 컴포넌트 대신 `NavLink`를 사용하면, `href`가 현재 주소와 일치할 때 `active` 클래스를 가지며, 아닌 경우 `inactive` 클래스를 갖습니다.

참고: 기본적으로 href `/users` 가 주소 `/users/123` 에도 매치되는 방식이므로, 이러한 매칭을 방지하고 싶다면 `end` 속성을 이용하십시오. 이것은 특히 root 경로 `/` 가 모든 경로와 매칭되는 것을 방지하므로 유용합니다.

| prop     | type    | description                                                                                                                                                                              |
|----------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| href     | string  | The path of the route to navigate to. This will be resolved relative to the route that the link is in, but you can preface it with `/` to refer back to the root.                                                                                                                                                    |
| noScroll | boolean | If true, turn off the default behavior of scrolling to the top of the new page                                                                                                           |
| replace  | boolean | If true, don't add a new entry to the browser history. (By default, the new page will be added to the browser history, so pressing the back button will take you to the previous route.) |
| state    | unknown | [Push this value](https://developer.mozilla.org/en-US/docs/Web/API/History/pushState) to the history stack when navigating  |

`NavLink` additionally has:

| prop     | type    | description                                                                                                                                                                              |
|----------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| inactiveClass | string  | The class to show when the link is inactive (when the current location doesn't match the link) |
| activeClass | string | The class to show when the link is active                                                                                                        |
| end  | boolean | If `true`, only considers the link to be active when the curent location matches the `href` exactly; if `false`, check if the current location _starts with_ `href` |
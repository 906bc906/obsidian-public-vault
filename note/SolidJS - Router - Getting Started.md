---
title: SolidJS - Router - Getting Started
date: 2022-10-10T00:31:40+09:00
last_modified_at: 2022-10-10T19:12:00+09:00
---

## Set Up the Router

```bash
> npm i @solidjs/router
```

`@solidjs/router` 를 설치하고, 루트 컴포넌트를 라우터 컴포넌트로 래핑하십시오.

```tsx
import { render } from "solid-js/web";
import { Router } from "@solidjs/router";
import App from "./App";

render(
  () => (
    <Router>
      <App />
    </Router>
  ),
  document.getElementById("app")
);
```

## Configure your Routes with JSX

JSX를 이용하여 Solid Router를 설정할 수 있습니다.

1. `Routes` 컴포넌트 안에 `Route` 컴포넌트를 추가하여, 사용자가 해당 경로로 이동하였을 때 어떤 컴포넌트 또는 엘리먼트를 렌더할지 특정하세요.

```tsx
import { Routes, Route } from "@solidjs/router"

export default function App() {
  return <>
    <h1>My Site with Lots of Pages</h1>
    <Routes>
			<Route path="/users" component={Users} />
      <Route path="/" component={Home} />
      <Route path="/about" element={<div>This site was made with Solid</div>} />
    </Routes>
  </>
}
```

1. route 에 사용되는 컴포넌트들을 lazy 로딩하세요.

```tsx
import { lazy } from "solid-js";
import { Routes, Route } from "@solidjs/router"
const Users = lazy(() => import("./pages/Users"));
const Home = lazy(() => import("./pages/Home"));
```

`Users` 및 `Home` 컴포넌트는 사용자가 `/users` 또는 `/home` 경로로 네비게이팅할 때만 로드될 것입니다.

## Configure your Routes with object

JSX를 사용하지 않고, `useRoutes` 에 오브젝트를 넘겨서 라우트를 설정할 수도 있습니다.

```tsx
import { lazy } from "solid-js";
import { render } from "solid-js/web";
import { Router, useRoutes, Link } from "@solidjs/router";

const routes = [
  {
    path: "/users",
    component: lazy(() => import("/pages/users.js"))
  },
  {
    path: "/users/:id",
    component: lazy(() => import("/pages/users/[id].js")),
    children: [
      { path: "/", component: lazy(() => import("/pages/users/[id]/index.js")) },
      { path: "/settings", component: lazy(() => import("/pages/users/[id]/settings.js")) },
      { path: "/*all", component: lazy(() => import("/pages/users/[id]/[...all].js")) }
    ]
  },
  {
    path: "/",
    component: lazy(() => import("/pages/index.js"))
  },
  {
    path: "/*all",
    component: lazy(() => import("/pages/[...all].js"))
  }
];

function App() {
  const Routes = useRoutes(routes);
  return (
    <>
      <h1>Awesome Site</h1>
      <Link class="nav" href="/">
        Home
      </Link>
      <Link class="nav" href="/users">
        Users
      </Link>
      <Routes />
    </>
  );
}

render(
  () => (
    <Router>
      <App />
    </Router>
  ),
  document.getElementById("app")
);
```
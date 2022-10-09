---
title: SolidJS - Router - Data functions
date: 2022-10-10T00:32:08+09:00
last_modified_at: 2022-10-10T00:32:08+09:00
---

그간의 예시에서, 유저 컴포넌트가 lazy 로드된 뒤에, 데이터가 fetch 되었다. route data functions으로, route를 로딩하면서 동시에 data를 fetch할 수 있다.

이를 위해서, `createResource`로 데이터를 fetch하고 반환하는 함수를 만든다. 그 후 `Route` 컴포넌트의 `data` 프롭에 해당 함수를 건낸다.

```tsx
import { lazy } from "solid-js";
import { Route } from "@solidjs/router";
import { fetchUser } ... 

const User = lazy(() => import("./pages/users/[id].js"));

//Data function
function UserData({params, location, navigate, data}) {
  const [user] = createResource(() => params.id, fetchUser);
  return user;
}

//Pass it in the route definition
<Route path="/users/:id" component={User} data={UserData} />;
```

route가 로드되면, data 함수도 호출되며, 그 결과를 `useRouteData()` 를 호출하여 접근할 수 있다.

```tsx
//pages/users/[id].js
import { useRouteData } from '@solidjs/router';
export default function User() {
  const user = useRouteData();
  return <h1>{user().name}</h1>;
}
```

유일한 인수로 데이터 함수에는 경로 정보에 액세스하는데 사용할 수 있는 개체가 전달된다.
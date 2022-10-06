# SolidJS - Reactivity - Resources

<!--Basic Template V0.0.2 Start -->
```dataview
TABLE WITHOUT ID  file.link AS title, tags
FROM -"templates"
SORT file.mtime, tags desc
WHERE contains(tags, [[]])
```
<!--Basic Template V0.0.2 End -->
[tags::[[SolidJS]]]

https://www.solidjs.com/tutorial/async_resources

https://www.solidjs.com/docs/latest/api#createresource

Resource는 비동기 로딩을 처리하도록 특별히 설계된 특수 시그널이다. 그 목적은 Solid의 분산 실행 모델과 쉽게 상호 작용할 수 있도록 비동기 값을 래핑하는 것이다.

이는 async/await 또는 순차 실행 모델을 제공하는 제너레이터와는 정반대다. 목표는 비동기 작업으로 실행을 중단하지 않고, 코드를 덧붙이지 않는 것이다.

Resource는 Promise를 반환하는 비동기 데이터 fetch 함수를 퀄히ㅏ는 소스 시그널에 의해 구동될 수 있다. fetch 함수의 내용은 무엇이든 가능하다. 일반 REST 엔드포인트나 GraphQL 호출처럼 Promise를 생성하는 모든 것을 할 수 있다. Resource는 데이터 로딩 방법에 좌우되지 않으며, Promise에 의해서만 구동된다.

Resource 시그널의 결과에는 현재 상태를 기반으로 View를 쉽게 컨트롤할 수 있도록 하는 반응형 loading과 error 프로퍼티도 포함된다.

```tsx
const fetchUser = async (id) =>
  (await fetch(`https://swapi.dev/api/people/${id}/`)).json();
const [userId, setUserId] = createSignal();
const [user, { mutate, refetch }] = createResource(userId, fetchUser);
```

userId, setUserId 는 검색 대상인 user의 ID 변화를 감지하는 시그널이다.

`setUserId(id)` -> `createResource`에 등록된 `userId` -> `createResource`에 등록된 `fetchUser(id)` -> `user()` 로 사용

과 같은 과정을 거친다.

createResource에서 반환되는 2번째 값에는 내부 시그널을 직접 업데이트하는 `mutate` 메서드와, 소스가 변경되지 않은 경우에도 현재 쿼리를 다시 로드하는 `refetch` 메서드가 포함되어 있다.

## 문제

TARGET DECK
전체::개발::solid

---

<!--ankiQ-->

```tsx
import { createSignal, createResource } from "solid-js";
import { render } from "solid-js/web";

const fetchUser = async (id) =>
  (await fetch(`https://swapi.dev/api/people/${id}/`)).json();

const App = () => {
  const [userId, setUserId] = createSignal();
  const [user] = <⚠️ userId가 변경될 시 fetchUser 로 리소스 획득>

  return (
    <>
      <input
        type="number"
        min="1"
        placeholder="Enter Numeric Id"
        onInput={(e) => setUserId(e.currentTarget.value)}
      />
      <span>{<⚠️로딩 동안 표시하도록 작성> && "Loading..."}</span>
      <div>
        <pre>{JSON.stringify(<⚠️유저의 값>, null, 2)}{user.error}</pre>
      </div>
    </>
  );
};

render(App, document.getElementById("app"));
```

위 Solid 코드의 빈칸을 createResource 를 이용하여 수정하라.

<!--ankiA-->

```tsx
import { createSignal, createResource } from "solid-js";
import { render } from "solid-js/web";

const fetchUser = async (id) =>
  (await fetch(`https://swapi.dev/api/people/${id}/`)).json();

const App = () => {
  const [userId, setUserId] = createSignal();
  const [user] = createResource(userId, fetchUser);

  return (
    <>
      <input
        type="number"
        min="1"
        placeholder="Enter Numeric Id"
        onInput={(e) => setUserId(e.currentTarget.value)}
      />
      <span>{user.loading && "Loading..."}</span>
      <div>
        <pre>{JSON.stringify(user(), null, 2)}{user.error}</pre>
      </div>
    </>
  );
};

render(App, document.getElementById("app"));
```

<!--ankiE-->
<!--ID: 1665057833173-->

---
title: SolidJS - Reactivity - Suspense List
date: 2022-10-10T00:31:01+09:00
last_modified_at: 2022-10-10T00:58:03+09:00
---

https://www.solidjs.com/tutorial/async_suspense_list

https://www.solidjs.com/docs/latest/api#suspenselist-experimental

SuspenseList는 실험 단계의 기능으로 온전한 SSR을 지원하지 않는다.

조정하고 싶은 `Suspense` 컴포넌트가 여러 개 있을 경우, 이를 통제하는 접근 방식은 여러 가지가 있을 것이다.

## 단일 Suspense

모든 것을 단일 `Suspense` 아래에 두는 방법이 있다.

하지만 이는 하나의 로딩 동작만 하도록 제한하며, 단일 fallback 상태를 가지므로, 마지막 항목이 로드될 때까지 모든 항목이 기다려야함을 의미한다.

## 개별 Suspense

모든 데이터를 개별 `Suspense` 로 묶는 방법이 있다.

다만 데이터들이 독립적으로 로딩되고, 로딩되는 순서에 따라 표시되므로, 난잡하게 보일 수 있다.

## SuspenseList

단일 `Suspense`, 개별 `Suspense` 가 모두 마음에 안 든다면, `SuspenseList`를 고려할 수 있다

```tsx
import { SuspenseList } from "solid-js";

function SuspenseList(props: {
  children: JSX.Element;
  revealOrder: "forwards" | "backwards" | "together";
  tail?: "collapsed" | "hidden";
}): JSX.Element;
```

`revealOrdr` 옵션을 통해 `Suspense`가 표시되는 순서를 제어하며, `tail` 옵션을 통해 fallback 상태를 축소하거나 숨길 수 있다.

```tsx
const ProfilePage = (props) => (
  <>
    <ProfileDetails user={props.user} />
    <Suspense fallback={<h2>Loading posts...</h2>}>
      <ProfileTimeline posts={props.posts} />
    </Suspense>
    <Suspense fallback={<h2>Loading fun facts...</h2>}>
      <ProfileTrivia trivia={props.trivia} />
    </Suspense>
  </>
);
```

위와 같은 예제 코드에서 `SuspenseList`를 이용하여 두 `Suspense`의 표시 순서와 fallback 상태를 제어한다면,

```tsx
<SuspenseList revealOrder="forwards" tail="collapsed">
  <ProfileDetails user={props.user} />
  <Suspense fallback={<h2>Loading posts...</h2>}>
    <ProfileTimeline posts={props.posts} />
  </Suspense>
  <Suspense fallback={<h2>Loading fun facts...</h2>}>
    <ProfileTrivia trivia={props.trivia} />
  </Suspense>
</SuspenseList>
```

이렇게 바꿀 수 있을 것이다.

### revealOrder

리스트 안의 `Suspense`가 표시되는 순서를 정의한다.

예제 코드에서 첫번째 `Suspense`가 1초, 두번째 `Suspense`가 2초 후에 해소된다고 가정하자.

1. forwards
	- 앞에 있는 Suspense 부터 표시된다.
	- 예제 상황에서는 첫번째 `Suspense`가 1초에 보이고, 두번째 `Suspense`가 2초에 보일 것이다.
2. backwards
	- 뒤에 있는 Suspense 부터 표시된다.
	- 예제 상황에서는 두번째 `Suspense`가 2초에 보이고, 그 뒤에 곧바로 대기하고 있던 첫번째 `Suspense`가 보일 것이다.
1. together
	- 모든 Suspense가 해소되어야 표시된다.
	- 예제 상황에서는 첫번째, 두번째 `Suspense`가 2초에 보일 것이다.

### tail

fallback 상태를 제어한다.

1. 지정하지 않음
	- 각 Suspense의 fallback이 개별적으로 표시된다.
2. collapsed
	- 순서에 따라 해소 대기중인 Suspense 만이 표시된다.
	- 단, `revealOrder`가 `together`인 경우 모든 Suspense의 fallback이 표시된다.
3. hidden
	- 모든 `Suspense`의 fallback을 숨긴다.

## 문제

TARGET DECK
전체::개발::solid

---

<!--ankiQ-->

```tsx
const ProfilePage = (props) => (
  <>
    <ProfileDetails user={props.user} />
    <Suspense fallback={<h2>Loading posts...</h2>}>
      <ProfileTimeline posts={props.posts} />
    </Suspense>
    <Suspense fallback={<h2>Loading fun facts...</h2>}>
      <ProfileTrivia trivia={props.trivia} />
    </Suspense>
  </>
);
```

위 Solid 코드에서, 두 `Suspense` 는 DOM 트리 순서와 상관없이 개별적으로 로딩될 것이다. 만약 이러한 `Suspense` 들의 표시 순서를 통제하고, fallback 을 제어하고 싶다면 어떤 방법이 있겠는가?

<!--ankiA-->

```tsx
<SuspenseList revealOrder="forwards" tail="collapsed">
  <ProfileDetails user={props.user} />
  <Suspense fallback={<h2>Loading posts...</h2>}>
    <ProfileTimeline posts={props.posts} />
  </Suspense>
  <Suspense fallback={<h2>Loading fun facts...</h2>}>
    <ProfileTrivia trivia={props.trivia} />
  </Suspense>
</SuspenseList>
```

`SuspenseList`를 사용한다. revealOrder와 tail은 상황에 맞게 사용.

<!--ankiE-->
<!--ID: 1665116359312-->

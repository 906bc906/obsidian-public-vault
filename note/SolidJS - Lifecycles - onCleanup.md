---
title: SolidJS - Lifecycles - onCleanup
date: 2022-10-10T00:17:48+09:00
last_modified_at: 2022-11-17T01:31:47+09:00
---


https://www.solidjs.com/tutorial/lifecycles_oncleanup

onCleanup 함수는 이펙트, 컴포넌트, 커스텀 디렉티브 뿐만 아니라 리액티브 시스템의 동기 실행이 일어나는 거의 모든 곳에서 사용할 수 있다. 해당 스코프가 재평가를 위해 트리거되고 최종적으로 폐기될 때 실행된다.

```tsx
const timer = setInterval(() => setCount(count() + 1), 1000);
onCleanup(() => clearInterval(timer));
```
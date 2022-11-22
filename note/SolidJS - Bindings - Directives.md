---
title: SolidJS - Bindings - Directives
date: 2022-10-10T00:19:04+09:00
last_modified_at: 2022-11-22T20:09:21+09:00
---

https://www.solidjs.com/tutorial/bindings_directives

Solid는 use: 네임스페이스를 사용해 커스텀 디렉티브를 지원한다. 

```tsx
<Component use:customFunc={() => doSomething(...)} />
```

custonFunc 는 아래의 형식을 갖는다.

```tsx
function customFunc(element, accessor) {
	//doSomething을 실행하기 위해서는
	accessor()?.();
}
```

element는 customFunc 를 use: 커스텀 디렉티브로 지정했던 DOM 엘리먼트이며, accessor는 속성에 할당된 값에 대한 getter 함수다.

```tsx
<div class="modal" use:clickOutside={() => setShow(false)}>
	Some Modal
</div>

function clickOutside(element, accessor) {
  const onClick = (e) => !element.contains(e.target) && accessor()?.();
  document.body.addEventListener("click", onClick);

  onCleanup(() => document.body.removeEventListener("click", onClick));
}
```

예를 들어 위와 같이  모달 바깥쪽을 클릭하면 사라지게끔 구현할 수 있다.
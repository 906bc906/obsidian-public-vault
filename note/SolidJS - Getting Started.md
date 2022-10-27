---
title: SolidJS - Getting Started
date: 2022-10-10T00:31:24+09:00
last_modified_at: 2022-10-26T01:11:30+09:00
---


## 참고 주소
https://docs.solidjs.com/tutorials/getting-started-with-solid/installing-solid

## 템플릿 설치

```bash
npx degit solidjs/templates/ts my-app
cd my-app
npm install # or yarn or pnpm
npm run dev # or yarn or pnpm
```

## 시작

Solid 구조
- index.html
	- Solid App(`Index.tsx`) 가 렌더링될 html
	- `index.tsx` 를 모듈로 import하는 구문이 body에 들어가야 함
	- div#root 가 body에 들어가야 함 (id 이름은 달라질 수 있으나 통상적으로 root)
- index.tsx
	- main 역할
- component (ex: App.tsx)
	- 인터페이스의 조각을 정의하는 함수
	- JSX 또는 TSX 반환
	- 컴포넌트는 하나의 최상위 HTML element를 리턴해야 함
		- 자식 Element는 몇 개를 가져도 상관 없음
		- JSX Fragment (`<> </>`) 를 이용해서 감쌀 수 있음

### Primitives

Component가 Solid 앱에서 view를 짓기 위한 블록이라면, Primitives는 반응성(interactivity)을 짓기 위한 블록이다. 처음으로 배울 primitive는 `signal`.

```ts
import { createSignal, createEffect } from "solid-js";
const [count, setCount] = createSignal(0);
createEffect(() => {
  console.log(count());
});
setCount(count() + 1);
```

1. state 변수를 만드는 것이 아니라, solid-js의 `createSignal`을 구조분해할당하여 `count`, `setCount` 를 생성함
	1. `createSignal(0)` 은 이 state 의 초기값이 0 임을 뜻함
	2. `count` 는 변수가 아니라 접근자(accessor, getter 라고도 부름) 함수임, state 그 자체가 오는 것이 아니라 현재의 state 값의 복사본을 가져온다- 즉 read-only 라고 생각
	3. `setCount` 는 setter 함수임.
2. createEffect 는 시그널에 의존하는 사이드 이펙트를 생성함.
	1. 함수 내에서 참조한 시그널을 자동으로 추적(구독)함
	2. 주로 읽기만 하는 사이드 이펙트를 설정하는 것을 권장함
		1. 이펙트 내에서  시그널 쓰기를 하는 경우 추가 렌더링, 무한 루프 등의 문제를 겪을 수 있음

시그널이 바뀌면, 해당 DOM 부분을 컨트롤하는 코드가 재실행된다. 
---
title: SolidJS
date: 2022-10-07T08:26:19+09:00
tags:
- todo
last_modified_at: 2022-10-10T19:12:00+09:00
---



솔리드는 반응형 웹 앱을 만들기 위한 JS 프레임워크다.

- [SolidJS - 튜토리얼](SolidJS%20-%20튜토리얼.md)
- [SolidJS - Getting Started](SolidJS%20-%20Getting%20Started.md)
- [SolidJS - Router](SolidJS%20-%20Router.md)

## 시작

```bash
npx degit solidjs/templates/ts my-app
cd my-app
npm install # or yarn or pnpm
npm run dev # or yarn or pnpm
```

템플릿을 이용하여 개발 시작한다.

### 컴포넌트란?

컴포넌트는 유저 인터페이스의 일부를 정의하는 함수.

HelloWorld.tsx

```tsx
export function HelloWorld() {
  return <div>Hello World!</div>;
}
```

`<div>` 엘리먼트 안에 `Hello World!`를 출력하는 컴포넌트. 

1.  컴포넌트의 이름은 title-cased.
   컴포넌트와 다른 JS 함수와의 구분을 도와줌.
2. 컴포넌트는 HTML처럼 보이는 것을 반환함. JSX라고 부름.

`App.tsx` 를 수정하여 방금 만든 컴포넌트를 출력해보자.

```tsx
import { HelloWorld } from "./HelloWorld";
function App() {
  return (
    <div>
      <h1>Welcome</h1>
      <HelloWorld />
    </div>
  );
}
export default App;
```

`HelloWorld` 함수를 import한 후 `<HelloWorld/>` 엘리먼트를 쓸 수 있게 됨.

###  JSX란?

https://docs.solidjs.com/tutorials/getting-started-with-solid/building-ui-with-components

## Tailwind CSS with SolidJS

[Install Tailwind CSS with SolidJS - Tailwind CSS](https://tailwindcss.com/docs/guides/solidjs)
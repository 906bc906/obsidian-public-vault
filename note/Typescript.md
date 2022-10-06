<!--Basic Template V0.0.2 Start -->
```dataview
TABLE WITHOUT ID  file.link AS title, tags
FROM -"templates"
SORT file.mtime, tags desc
WHERE contains(tags, [[]])
```
<!--Basic Template V0.0.2 End -->
[tags::[[to do]]]

TARGET DECK
전체::개발::typescript

https://www.typescriptlang.org/

자바스크립트로 빌드하는 강타입 언어.

## 설치 및 실행

일반적으로는, 타입스크립트 `ts` 파일을 자바스크립트 파일로 컴파일하고 그것을 실행하는 방식으로 진행한다.

```bash
npm install typescript -D
```

```bash
npx tsc hello.ts
node hello.js
```

typescript 패키지에는 컴파일러(tsc)가 포함되어있으며 npx tsc 로 ts 파일을 js로 컴파일할 수 있다. (전역으로 설치했다면 npx 안 써도 됨)

<!--ankiQ-->

Q. 타입스크립트의 타입 표기는 프로그램의 런타임 동작을 수정하는가?

<!--ankiA-->

A. 전혀 수정하지 않음.

<!--ankiE-->
<!--ID: 1664295816662-->

### ts-node

컴파일 한 후 node로 실행하는 것이 번거롭다면 ts-node를 사용해볼 수 있다.

https://www.npmjs.com/package/ts-node

ts-node는 실행하면서 ts를 js로 즉시 전환해준다. 즉, 위의 tsc -> node 과정을,

```bash
ts-node hello.ts
```

와 같이 생략할 수 있다.

nodemon과 같이 사용할 수도 있다.[^hannah][^memi-dev] 개발에 사용하면 편리. 

ts-node-dev 라는 모듈도 있다.[^inpa]

[^hannah]: [[TypeScript] nodemon, ts-node 모듈 설치하기](https://velog.io/@grinding_hannah/TypeScript-nodemon-ts-node-%EB%AA%A8%EB%93%88-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0)

[^memi-dev]: https://www.youtube.com/watch?v=6hz8EX9QhCg `-r tsconfig-paths/register`

[^inpa]:  [[TS] 📘 TypeScript 소개 & 개발 환경 구성하기](https://inpa.tistory.com/entry/TS-%F0%9F%93%98-TypeScript-%EC%86%8C%EA%B0%9C-%EA%B0%9C%EB%B0%9C-%ED%99%98%EA%B2%BD-%EC%84%A4%EC%A0%95-%EC%B4%9D%EC%A0%95%EB%A6%AC-tsconfig)

하지만 컴파일러를 메모리 위에 올려두고 동작하는 특성상 production에서는 사용하지 않는 것이 좋다.[^siner]

[^siner]: [node와 ts-node의 CPU,RAM 사용량 비교](https://blog.siner.io/2022/02/19/node-vs-ts-node/)

### 빌드 툴

https://www.typescriptlang.org/docs/handbook/integrating-with-build-tools.html

여러가지 빌드 툴을 이용하여 빌드할 수 있다.

#### Vite

Vite는 `.ts` 파일을 즉시 import 하는 것을 지원한다. 단, 타입 확인은 하지 않아 `tsc --noEmit` 등 빌드 과정에서 확인해야 함. [^vite-ts]

또한 TS 컴파일러 옵션 중 `isolatedModules` 가 무조건 `true` 로 설정되어야 한다.

[^vite-ts]: [Features | Vite](https://vitejs.dev/guide/features.html#typescript)

## tsconfig.json

[TypeScript: Documentation - What is a tsconfig.json](https://www.typescriptlang.org/ko/docs/handbook/tsconfig-json.html)

### noImplicitAny
몇몇 경우 TS는 값의 타입을 추론하지 않고 any로 간주하는데, 위 플래그를 활성화하면 이런 암묵적 any 추론에 대하여 오류를 발생함.

### strictNullChecks
null, undefined를 보다 명시적으로 처리. null 혹은 undefined 갑을 참조하는 것을 방지함.

[[TypeScript] strictNullChecks과 Type Guards를 이용한 안전한 코드 | kimhako](https://ohhako.github.io/kimhako/articles/2020-07/Angular-strictNullChecks-post-copy)

## VScode extension

[[VSCode] 💽 TypeScript 코딩하는데 유용한 확장팩 💯 추천](https://inpa.tistory.com/entry/VSCode-%F0%9F%92%BD-TypeScript-%EC%BD%94%EB%94%A9%ED%95%98%EB%8A%94%EB%8D%B0-%EC%9C%A0%EC%9A%A9%ED%95%9C-%ED%99%95%EC%9E%A5%ED%8C%A9-%F0%9F%92%AF-%EC%B6%94%EC%B2%9C)

## 문법

### 타입 추론

```ts
let msg = "hello there!" //msg: string
```

값으로부터 추론되는 것들은 굳이 타입 표기를 적지 않는 것이 가장 좋다.

### 변수 타입 지정
```typescript
let name:string = "aaa";
```

```typescript
let names:string[] = ["aaa", "bbb"];
```

```typescript
let name:{firstName:string, middleName?:string, lastName:string} = {firstName:"Cheolsu", lastName:"Kim"};
```

#### union type

```typescript
let codeNumber:string|number = 123;
```

#### type alias

```typescript
type NewType = string | string[];
let varname:NewType = "value";
```

#### interface

```typescript
interface User {
  name: string
  age: number
  gender? : string
  readonly birthYear : number
  [key:number] : string;
}

let user : User = {
  name : 'xx',
  age : 30,
  birthYear : 2000,
  1 : 'A',
  2 : 'B'
}
```

```typescript
interface Add {
  (num1:number, num2:number): number;
}

const add : Add = function(x, y){
  return x + y;
}
```

##### interface extends

```typescript
interface Car {
  color: string;
}
  
interface Toy {
  name : string;
}

interface ToyCar extends Car, Toy {
  price: number;
}
```

#### function type

```typescript
function makeDouble(x: number) :number{
  return x*2;
}
```

##### Generic

```typescript
function getSize<T>(arr: T[]): number {
  return arr.length;
}

console.log(getSize([1, 2, 3]));
console.log(getSize<string>(["1", "2", "3"]));
```


#### Array tuple

```typescript
type Member = [number, boolean];
let john:Member = [1, true];
```

#### class
```typescript
class User {
  name:string;
  constructor(name:string){
    this.name = name;
  }
}
```

##### class implements with interface

```typescript
interface Car {
  color: string;
  wheels: number;
  start(): void;
}

class Bmw implements Car {
  color: string;wheels = 4;
  constructor(c: string) {
    this.color = c;
  }
  start() {
    console.log('go..');
  }
}
```

##### type 전용 imports, exports

<!--ankiQ-->

"./some-module.js" 에서 SomeThing 타입만 import 하고 싶은 경우 어떻게 해야 합니까?

<!--ankiA-->

```typescript
import type { SomeThing } from "./some-module.js";
export type { SomeThing };
```

<!--ankiE-->
<!--ID: 1664943651422-->

<!--ankiQ-->

typescript에서, interface와 type alias의 차이점은 무엇입니까?

```typescript
interface Point {
  x: number;
  y: number;
}
```

```typescript
type Point = {
  x: number;
  y: number;
};
```

<!--ankiA-->

1. 오브젝트의 모양을 기술하기 위해 사용될 수 있으나 신택스가 다르다.
2. 인터페이스와 다르게, type alias는 primitives, union, tuple 에도 사용될 수 있다.
```typescript
// primitive
type Name = string;

// object
type PartialPointX = { x: number; };
type PartialPointY = { y: number; };

// union
type PartialPoint = PartialPointX | PartialPointY;

// tuple
type Data = [number, string];
```
3. 확장을 위한 신택스가 다르다.
```typescript
interface PartialPointX { x: number; }
interface Point extends PartialPointX { y: number; }
```

```typescript
type PartialPointX = { x: number; };
type Point = PartialPointX & { y: number; };
```

4. type alias에서 union 사용 시 class implement에 사용할 수 없다. 
```typescript
type PartialPoint = { x: number; } | { y: number; };

// FIXME: can not implement a union type
class SomePartialPoint implements PartialPoint {
  x = 1;
  y = 2;
}
```

[Interfaces vs Types in TypeScript - Stack Overflow](https://stackoverflow.com/questions/37233735/interfaces-vs-types-in-typescript)

[TypeScript: Documentation - Everyday Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#differences-between-type-aliases-and-interfaces)

<!--ankiE-->
<!--ID: 1664295816666-->

## Handbook
[TypeScript: Handbook - The TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html)

프로그래밍 커뮤니티에 소개된 지 20년이 지나, 자바스크립트는 가장 널리 퍼진 크로스 플랫폼 언어가 되었다. 웹페이지에 사소한 상호작용을 넣기 위한 작은 스크립팅 언어로 시작하여, 자바스크립트는 규모에 상관없이, 프트엔드든 백엔드든 선택할 수 있는 언어까지 성장했습니다.



## to do

abstract class
접근 제한자 (public, private, protected)

https://www.typescriptlang.org/docs/handbook/2/narrowing.html

https://www.youtube.com/watch?v=17Oh028Jpis&list=PLZKTXPmaJk8KhKQ_BILr1JKCJbR0EGlx0&index=6

유틸리티 타입
- keyof
- partial
- required
- readonly

## 참고

- https://github.com/basarat/typescript-book
	- 타입스크립트 
- [type-challenges/type-challenges: Collection of TypeScript type challenges with online judge](https://github.com/type-challenges/type-challenges)
	- 타입 정의 문제들
- 
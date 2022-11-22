---
title: SolidJS - Introduction - Basics
date: 2022-10-10T09:02:13+09:00
last_modified_at: 2022-11-17T01:31:47+09:00
---




https://www.solidjs.com/tutorial/introduction_basics

Solid는 
- 인터랙티브 웹 애플리케이션을 만들기 위한 자바스크립트 프레임워크
- 기존 HTML 및 자바스크립트 지식을 사용해 앱 전체에서 재사용 가능한 컴포넌트를 빌드할 수 있음
- 반응성[^반응성]을 사용해 컴포넌트를 향상시킬 수 있는 도구를 제공

[^반응성]: 사용자 인터페이스와 데이터를 연결하는 선언적 자바스크립트 코드
	
	Event(변경 사항의 전파 + 데이터 흐름) + 선언적 = 반응형 프로그래밍

	[프로그래밍 패러다임과 반응형 프로그래밍 그리고 Rx](https://velog.io/@teo/reactive-programming)

모든 Solid 앱의 진입점은 `render` 함수다.

```ts
render(() => <HelloWorld />, document.getElementById('app'))
```

---



## 문제

TARGET DECK
전체::개발::solid

<!--ankiQ-->

반응형 프로그래밍이란?

<!--ankiA-->

변경 사항의 전파 + 데이터 흐름 (via Event) 에 중점을 둔 선언적 프로그래밍 패러다임

<!--ankiE-->
<!--ID: 1664943788324-->


<!--ankiQ-->

Solid 앱의 진입점은?

<!--ankiA-->

render

<!--ankiE-->
<!--ID: 1664943788336-->


<!--ankiQ-->

Solid 앱의 진입점을 render 함수로 잡으려고 한다.

래핑 함수는 HelloWorld, 마운트할 대상 HTML 엘리먼트의 id는 'app' 일 때 render 함수를 작성하라.

<!--ankiA-->

`render(() => <HelloWorld />, document.getElementById('app'))`

<!--ankiE-->
<!--ID: 1664943788342-->

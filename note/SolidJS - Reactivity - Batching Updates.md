---
title: SolidJS - Reactivity - Batching Updates
date: 2022-10-10T00:30:15+09:00
last_modified_at: 2022-11-17T01:31:47+09:00
---

https://www.solidjs.com/tutorial/reactivity_batch

Solid의 반응성은 동기식이다. 즉, 변경이 일어나면 다음 행에서 DOM이 업데이트된다는 뜻이다. Solid의 세분화된 렌더링은 리액티브 시스템에서 발생한 업데이트를 전파할 뿐이므로 대부분의 경우 이는 문제가 없다. 관련없는 변경 사항을 두 번 "렌더링" 한다고 해서, 실제로 쓸데없는 작업이 낭비되는 것은 아니다.

만일 변경 사항이 서로 관련되어 있으면 어떻게 될까? Solid의 batch 헬퍼를 사용하게 되면 여러 변경 사항을 대기열에 넣은 다음, 옵저버에 알리기 전에 모두 동시에 적용할 수 있다. 배치 내에서 업데이트된 시그널 값은 완료될 때까지 커밋되지 않는다.

```tsx
console.log("Button Clicked");
setFirstName(firstName() + "n");
setLastName(lastName() + "!");
```

firstName, lastName 시그널의 변경이 같은 엘리먼트를 렌더링을 다시 하게 만드는 반응성을 갖고 있다면, 위의 구문을 실행하면 해당 엘리먼트는 두 번 렌더링 될 것이다.

```tsx
console.log("Button Clicked");
batch(() => {
	setFirstName(firstName() + "n");
	setLastName(lastName() + "!");
})
```

위와 같이 batch 를 활용하면 렌더링은 한번만 될 것이다.

## 문제

TARGET DECK
전체::개발::solid

---

<!--ankiQ-->

```tsx
setFirstName(firstName() + "n");
setLastName(lastName() + "!");
```

위 Solid 코드의 firstName, lastName은 시그널이다. 이대로 실행한다면 FirstName의 변경이 전파된 후 LastName의 변경이 전파될 것이다. 위 두 시그널의 전파를 동시에 발생시키고 싶은 경우 어떻게 해야겠는가?

<!--ankiA-->

```tsx
batch(() => {
	setFirstName(firstName() + "n");
	setLastName(lastName() + "!");
})
```

batch 사용.

<!--ankiE-->
<!--ID: 1665048202080-->

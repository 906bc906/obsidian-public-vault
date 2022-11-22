---
title: SolidJS - Bindings - Refs
date: 2022-10-10T00:18:37+09:00
last_modified_at: 2022-11-22T20:09:21+09:00
---

https://www.solidjs.com/tutorial/bindings_refs

2022년 10월 5일 기준, 위 주소의 애니메이션 테스트를 하기 위해서는 [css 파일의 로고 주소를 편집해야 합니다.](https://github.com/solidjs/solid-docs/issues/188) 

JSX는 실제 DOM 엘리먼트를 생성하기 때문에, Solid에서는 항상 할당을 통해 DOM에 대한 레퍼런스를 얻을 수 있다. 

```tsx
const myDiv = <div>My Element</div>;
```

예를 들면 위와 같다.

하지만 엘리먼트를 분리하지 않고 하나의 연속된 JSX 템플릿에 넣는 경우 Solid가 생성시 최적화를 더 잘할 수 있다는 이점이 있다.

Solid에서는 ref 속성을 사용해 엘리먼트에 대헌 레퍼런스를 얻을 수 있다.

```tsx
let myDiv;
...
<div ref={myDiv}>My Element</div>
```

위와 같이 변수를 선언하고, ref 속성에 전달하기만 하면 변수가 할당된다. 

문서의 DOM에 첨부되기 전에 생성될 때 할당이 발생한다. 생성 전에는 할당되지 않으므로 필요한 경우 onMount 등과 같이 쓰는 것이 좋다.

```tsx
<div ref={el => /* do something with el... */}>My Element</div>
```

레퍼런스는 콜백 함수의 형태로도 사용 가능한데, 엘리먼트가 첨부될 때까지 기다릴 필요 없이 로직을 캡슐화할 때 편리하다.

## 문제

TARGET DECK
전체::개발::solid

---

<!--ankiQ-->

```tsx
function App() {
  let canvas;

	const ctx = canvas.getContext("2d");
	let frame = requestAnimationFrame(loop);

	function loop(t) {
		frame = requestAnimationFrame(loop);

		const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);

		for (let p = 0; p < imageData.data.length; p += 4) {
			const i = p / 4;
			const x = i % canvas.width;
			const y = (i / canvas.height) >>> 0;

			const r = 64 + (128 * x) / canvas.width + 64 * Math.sin(t / 1000);
			const g = 64 + (128 * y) / canvas.height + 64 * Math.cos(t / 1000);
			const b = 128;

			imageData.data[p + 0] = r;
			imageData.data[p + 1] = g;
			imageData.data[p + 2] = b;
			imageData.data[p + 3] = 255;
		}

		ctx.putImageData(imageData, 0, 0);
	}

	onCleanup(() => cancelAnimationFrame(frame));


  return <canvas width="256" height="256" />;
}
```

위 Solid 코드는 리턴할 캔버스를 편집하여 애니메이션을 적용하는 것이 목적이다.

하지만 canvas 참조를 가져오지 못하고 있는 상황이다.

가장 간단한 것은 `canvas = document.querySelector("canvas#ok");` 와 같이 DOM API로 canvas를 가져오는 것이겠으나, 모종의 이유로 (엘리먼트를 중복 배치하여 쿼리 셀렉터로 고르기 애매한 상황이라거나) 이를 사용할 수 없다면, 반환할 canvas를 지정하기 위해서는 어떻게 해야겠는가?

<!--ankiA-->

1. const ctx=~ 절부터 onMount로 감싼다.
2. ref 지정
	- `return <canvas ref={canvas} width="256" height="256" />;`

```tsx
function App() {
  let canvas;
  onMount(() => { // <-- 1
    const ctx = canvas.getContext("2d");
    let frame = requestAnimationFrame(loop);

    function loop(t) {
      frame = requestAnimationFrame(loop);

      const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);

      for (let p = 0; p < imageData.data.length; p += 4) {
        const i = p / 4;
        const x = i % canvas.width;
        const y = (i / canvas.height) >>> 0;

        const r = 64 + (128 * x) / canvas.width + 64 * Math.sin(t / 1000);
        const g = 64 + (128 * y) / canvas.height + 64 * Math.cos(t / 1000);
        const b = 128;

        imageData.data[p + 0] = r;
        imageData.data[p + 1] = g;
        imageData.data[p + 2] = b;
        imageData.data[p + 3] = 255;
      }

      ctx.putImageData(imageData, 0, 0);
    }

    onCleanup(() => cancelAnimationFrame(frame));
  });

  return <canvas ref={canvas} width="256" height="256" />; //<-- 2
}

```

<!--ankiE-->
<!--ID: 1664962643991-->

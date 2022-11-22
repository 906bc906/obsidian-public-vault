---
title: Next.js 13 App Dir 도입을 포기하게 된 이유
date: 2022-11-22T19:52:11+09:00
last_modified_at: 2022-11-22T19:52:44+09:00
---
Next.js 13에 AppDir 이라는 새로운 디렉토리 구조가 도입되었다. 학습용으로 임시 리포지토리를 만들었을 때는 굉장히 편했기에 그룹 프로젝트에도 이를 도입하려고 했으나 Styled-Component 에 문제가 발생하여 도입을 포기하게 되었다.

Next.js 13에 관한 nextjs 공식 블로그의 글은 아래 링크에서 확인할 수 있다: [Blog - Next.js 13 | Next.js](https://nextjs.org/blog/next-13)

Next.js 13의 기능들을 쉽게 설명해주는 영상
- https://www.youtube.com/watch?v=6aP9nyTcd44
- https://github.com/906bc906/practice/tree/nextjs/sonny-13-tutorial
	- 위의 영상을 보고 작성한 코드

## Next.js 13 AppDir

### 기존 Next.js 는 어땠는데

[Routing: Introduction | Next.js](https://nextjs.org/docs/routing/introduction)

기존 Next.js 는 pages directory 를 기반으로 라우팅했다. 

아래 예시를 본다면 대강 무슨 느낌인지 이해가 될 것이다.

-   `pages/index.js` → `/`
-   `pages/blog/index.js` → `/blog`
-   `pages/blog/first-post.js` → `/blog/first-post`
-   `pages/dashboard/settings/username.js` → `/dashboard/settings/username`
-   `pages/blog/[slug].js` → `/blog/:slug` (`/blog/hello-world`)
-   `pages/[username]/settings.js` → `/:username/settings` (`/foo/settings`)
-   `pages/post/[...all].js` → `/post/*` (`/post/2020/id/title`)

또한 `pages/` 아래에 `_app.js` 와 `_document.js` 를 두어 페이지를 초기화하고 페이지 렌더링에 사용되는 html, body 태그를  업데이트하는 데 사용했다.

-   `pages/blog/first-post.js` → `/blog/first-post`

이 사례를 보면 알겠지만 `pages` 디렉토리 아래에 있는 모든 파일을 다 라우팅하기 때문에, 컴포넌트 파일들을 `pages` 디렉토리 아래에 둘 수가 없다. 이를 해결하기 위해서는 `pages` 디렉토리가 아닌 별도의 `components` 디렉토리를 만들어야 하고 이는 해당 컴포넌트들이 어느 페이지에 사용되는지 파악하기 어렵게 한다.

### Next.js 13의 AppDir은 어떻길래

[Routing: Fundamentals | Next.js](https://beta.nextjs.org/docs/routing/fundamentals)

`app` 디렉토리 아래에 있는 파일들을 라우팅에 사용한다는 골자는 여전하나, 내부의 컴포넌트 동작 방식이나 라우팅에 사용되는 파일들이 바뀌었다.

먼저 디렉토리의 경우,

- 그냥 이름을 짓는다면 평범하게 라우팅된다.
	- `app/dashboard` -> `/dashboard`
- 이전과 마찬가지로 다이나믹 라우팅을 할 수 있다.
	- `app/dashboard/[id]` -> `/dashboard:id`
- **NEW** [라우트 그룹](https://beta.nextjs.org/docs/routing/defining-routes#route-groups)이라는 개념이 추가되었다.
	- `app/(marketing)/about` -> `/about`
	- URL 패턴에는 영향을 미치지 않으면서 관심사별로 묶을 수가 있다.
	- 레이아웃(추후에 소개)을 분리할 수 있다.

라우트 그룹은 써봤을 때 정말 편리하다고 느꼈다. 

파일의 경우, 일단 있기만 하면 path에 무조건 매칭되던 전과 크게 다르다.

- page.tsx
	- index.js 와 비슷한 역할을 한다.
	- 해당 라우트로 도착했을 때 보여야 하는 페이지.
- layout.tsx
	- 해당 라우트를 기점으로 레이아웃을 설정한다.
	- 레이아웃 아래에 children 항목을 가져야 한다.
	- 하위 항목으로 네비게이션하는 동안은 레이아웃의 상태가 보존되고 리렌더링되지 않는다.
- loading.tsx
	- suspense fallback과 비슷한 역할을 한다.
	- data 를 fetch하는 동안 해당 data를 사용하는 컴포넌트 영역에 대신 표시되는데, 별도의 suspense 설정 없이 알아서 출력됨.
- error.tsx(e.g not-found.tsx)
	- 지정한 에러가 발생하는 경우 해당 파일을 대신 출력한다.
- template.tsx
	- 레이아웃과 비슷한데 라우팅할 때마다 재생성되는듯. (안써봐서 확실하지 않음)
- head.tsx
	- title이나 meta 같은 head 값을 overwrite 한다.

이제 해당 디렉토리 안에 컴포넌트를 집어넣어도 해당 디렉토리 경로로는 page.tsx 만 퍼블릭하기 때문에 컴포넌트가 매칭되는 일이 없다는 점, 이로 인해 페이지 디렉토리별로 컴포넌트를 관리할 수 있다는 점이 꽤 크다.

### Next.js 13 AppDir의 추가적인 변경점

- React 18에서 도입된 Server Component 를 기본적으로 사용한다.[^sc]
	- 이전에는 Client Component 가 기본이었으며 여기에 `getServerSideProps` 를 붙일 때와는 동작 방식이 다름
- Server Components의 `fetch` 동작 방식이 달라져 이를 통해 SSR, SSG, ISR을 쉽게 구현할 수 있음.
	- [Next.js 13 App Dir SSR, SSG, ISR](Next.js%2013%20App%20Dir%20SSR,%20SSG,%20ISR.md)

[^sc]: https://nextjs.org/blog/next-13#server-components

## 그래서 왜 포기 했는데

> React 18에서 도입된 Server Component 를 기본적으로 사용한다

이 부분이 컸다. 결론만 말하면 프로젝트에서 스타일링에 사용하는 `emotion` + `styled-components` 를 쓸 수 없다.

### Server Component, Styled-Components

`emotion`과 `styled-components` 는 스타일시트를 관리하고, ThemeProvider에 `React.createContext`를 사용한다.

React 18 의 Server Component는 state, context를 비롯한 hook을 사용할 수 없다. 

Next.js의 AppDir는 기본적으로 React18의 Server Component 를 사용하기 때문에, hook을 사용하는 라이브러리를 사용할 수 없다. 

https://github.com/vercel/next.js/issues/41958

위의 이슈에 달린 [댓글](https://github.com/vercel/next.js/issues/41958#issuecomment-1294063309)을 보면 알 수 있듯 이는 버그가 아니라 의도된 행동이며 스타일링 라이브러리에서 Server Component를 지원해야 한다.

정 Next.js 13의 AppDir 에서 context를 쓰는 css-in-js를 쓰고 싶다면, 해당 라이브러리를 쓰는 모든 컴포넌트에 `use client` 선언문으로 Client Component 로 전환해야 한다.

스타일링해야 하는 컴포넌트 태반을 CSR 로 돌릴거라면 Next.js를 써야될 의미가.. 있을까? 그렇다고 해서 모든 스타일을 정적 CSS로 바꾸는 것도 소요 시간이 너무 크고 그렇다고 추후 작업 시간이 줄어드는 것도 아니고, 여러모로 애매해서 위의 이유로 4시간 정도 투자한 AppDir를 포기했다.

## 앞으로도 답 없나요?

> **Note:** We're testing out different CSS-in-JS libraries and we'll be adding more examples for libraries that support React 18 features and/or the `app` directory.

Next.js에서 독자적인 CSS-in-JS 라이브러리를 테스트하고 있다고 한다. (아마도 `styled-jsx` 인듯)

2022년 11월 기준으로는 아직까진 React 18 Server Component 를 지원하는 CSS-in-JS 라이브러리는 찾지 못 했다.

### 대안은?

- [CSS Modules](https://beta.nextjs.org/docs/styling/css-modules)
- [Tailwind CSS](https://beta.nextjs.org/docs/styling/tailwind-css)
- [Global Styles](https://beta.nextjs.org/docs/styling/global-styles)
- [CSS-in-JS](https://beta.nextjs.org/docs/styling/css-in-js)
- [External Stylesheets](https://beta.nextjs.org/docs/styling/external-stylesheets)
- [Sass](https://beta.nextjs.org/docs/styling/sass)

## 우린 CSS-in-JS 안 쓰는데 AppDir 한번 써볼까?

공식에서도 프로덕션에는 쓰지 말라는 의견을 남기기도 했고, [아직 지원하지 않는 기능](https://beta.nextjs.org/docs/app-directory-roadmap)도 있기 때문에 아직은 보류하는 것이 좋아 보인다.
---
title: SolidJS - Router
date: 2022-10-10T00:32:30+09:00
last_modified_at: 2022-10-26T01:11:30+09:00
---

https://github.com/solidjs/solid-router

라우터를 사용하면 브라우저의 URL에 따라 view를 변경할 수 있다. 이것은 "Single-page" application이 기존의 멀티 페이지 사이트를 흉내낼 수 있게 한다. 솔리드 라우터를 사용하기 위해서는, URL 값(경로, path)에 의존하는 `Routes` 컴포넌트를 지정해야 하며, 이후 `Router`가 그들을 교체하는 매커니즘을 처리한다.

Solid Router는 SolidJS를 위한, 클라이언트에서 렌더링하든 서버에서 렌더링하든 상관없이 작동하는 범용 라우터다. React 라우터와 Ember 라우터의 패러다임에서 영감을 받아 결합했다. `Routes` 들은 JSX를 이용하여 어플리케이션의 템플릿에서 직접 정의할 수도 있지만, 경로 환경 설정을 Object로 전달할 수도 있다. 중첩 라우팅도 지원하여, 완전히 교체할 필요 없이 일부만 바꿀 수도 있다.

Solid의 모든 SSR 메서드를 지원하며 Solid의 transisions이 탑재되어 있으므로, suspense, resources, lazy components 를 자유롭게 사용할 수 있다. Solid Router로 병렬로 로드되는 데이터 함수를 정의할 수도 있다.

- [SolidJS - Router - Getting Started](SolidJS%20-%20Router%20-%20Getting%20Started.md)
- [SolidJS - Router - Create Links to Your Routes](SolidJS%20-%20Router%20-%20Create%20Links%20to%20Your%20Routes.md)
- [SolidJS - Router - Dynamic Routes](SolidJS%20-%20Router%20-%20Dynamic%20Routes.md)
- [SolidJS - Router - Data functions](SolidJS%20-%20Router%20-%20Data%20functions.md)
- SolidJS - Router - Nested Routes
- SolidJS - Router - Hash Mode Router
- SolidJS - Router - Router Primitives
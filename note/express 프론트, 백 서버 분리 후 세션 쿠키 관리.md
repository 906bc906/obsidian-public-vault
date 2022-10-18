---
title: express 프론트, 백 서버 분리 후 세션 쿠키 관리
date: 2022-10-14T14:26:31+09:00
last_modified_at: 2022-10-18T14:59:39+09:00
---
## 기존

하나의 node.js 익스프레스 서버에서 템플릿 엔진을 이용하여 프론트, 백 둘 다 담당하였음

## 변경

프론트엔드 웹서버와 API 백엔드 Express 서버를 분리하였음.

## 문제

- 프론트엔드
	- fetch로 API 서버와 통신하는데 쿠키를 받을 수 없음
- 백엔드
	- CORS로 인해 프론트엔드의 요청을 전부 거절중

## 해결

### 프론트엔드

```js
fetch('<API SERVER URL>', {credentials: "include"})
```

API 서버와의 통신에 credentials 옵션을 include로 설정하여, request할 때 쿠키를 보내고 response에서 set 쿠키를 받아와서 저장함.

credentials

값 | 설명
--:|:--
same-origin|기본값. 같은 출처의 요청에만 크레덴셜 포함됨.
include|모든 요청에 크레덴셜 포함됨. 이 경우 `Access-Control-Allow-Origin` 는 `*` 여서는 안되며 명시적으로 나타내야함.
omit|모든 요청에 크레덴셜 제외됨.

axios 사용하는 경우 `credentials` 이 아닌 `withCredentials` 을 `true` 로 설정해야 함. 자세한 내용은 참고 : [react router props 전달 & 브라우저에 쿠키 저장이 안되는 문제 해결](https://velog.io/@yhe228/react-router-props-axios-cookie-get-set)

### 백엔드

여러 단계를 거쳐야 함.

1. CORS 활성화

```js
import cors from 'cors'

const corsWhitelist = ["http://localhost:3000", "http://localhost:8000"]
app.use(cors({
	origin: (origin, callback) =>
		corsWhitelist.indexOf(origin || "") !== -1 || !origin
		? callback(null, true)
		: callback(new Error(`${origin} Not allowed by CORS`))
}));
```

Express 에서는 `cors` node.js 패키지를 사용하여 cors를 활성화할 수 있음.

위의 코드는 화이트리스트를 여러 군데 설정해야 하는 경우고, 모두 허용한다면

```js
import cors from 'cors'

app.use(cors());
```

와 같이 설정할 수 있음. 하지만 위에서 말했듯 명시적으로 작성해야만 크레덴셜을 주고 받을 수 있음.

자기 자신(프론트엔드 클라이언트가 아닌 백엔드에서 직접)의 요청의 경우 `origin`의 값이 `undefined`가 되므로 `|| !origin` 를 뒤에 붙여줘서 자기 자신의 요청도 처리할 수 있도록 하였음.

```js
import cors from 'cors'

app.use(cors({
	origin: "http://localhost:8000" 
	}));
```

위와 같이 작성하면 자기 자신의 요청 + `http://localhost:8000` 을 허용함.

1. Access-Control-Allow-Credentials 활성화

```js
res.header('Access-Control-Allow-Credentials', 'true');
```

메서드마다 관리하고 싶으면 위와 같이 해도 되고,

```js
app.use(cors({
	origin: "http://localhost:8000",
	credentials: true
}));
```

전부 활성화하고 싶다면 cors 미들웨어에 옵션으로 추가해도 됨.

## 파생 문제

- [쿠키 SameSite 문제](쿠키%20SameSite%20문제.md)
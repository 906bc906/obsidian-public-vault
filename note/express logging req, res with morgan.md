---
title: express logging req, res with morgan
date: 2022-10-28T15:24:34+09:00
last_modified_at: 2022-10-28T15:24:42+09:00
---
[Express morgan middleware](https://expressjs.com/en/resources/middleware/morgan.html)

morgan은 HTTP request Logger middleware다.

## 왜 써야 하는가

express 앱을 사용하다 보면 어떤 요청들이 들어오는지, 응답은 어떻게 하는지 확인할 필요가 있다. `morgan`을 이용하면 이를 굉장히 편하게 할 수 있다.

## 어떻게 써야 하는가

### 설치

```bash
npm i morgan
```

typescript를 사용하는 경우 `@types/morgan` 도 설치해야 한다.

### 사용

```ts
import morgan from 'morgan'
...

app.use(morgan('tiny'))
```

사용법도 간단한데, `morgan` 을 불러와서 미들웨어로 붙여주기만 하면 된다.

첫번째 인자는 형식, 두번째 인자는 옵션을 지정한다.

형식의 경우 개발자가 마련한 사전정의 형식을 써도 된다.
- combined
- common
- dev
- short
- tiny

아니면 사전 정의된 토큰을 이용해서 `':method :url :status :res[content-length] - :response-time ms'` 와 같이 형식 문자열을 작성해도 된다. 방법은 다양하며 커스텀하고 싶으면 [readme](https://github.com/expressjs/morgan) 참조.

### winston 과 같이 사용

기본적으로 콘솔 출력하기 때문에, winston 과 사용하고 싶은 경우 `stream`을 지정해야 한다.

```ts
app.use(morgan('tiny', {
	stream: {
		write: (msg) => logger.http(msg)
	}//skip: () => {return (skip condition)}
}))
```

크게 어렵진 않고 위와 같이 해주면 끝.

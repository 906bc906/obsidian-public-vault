---
title: express error handling (작성중)
date: 2022-10-17T23:51:40+09:00
last_modified_at: 2022-10-28T15:28:31+09:00
---


https://teamdable.github.io/techblog/express-error-handling

https://poiemaweb.com/express-error-handling

[Express winston morgan 로그 관리](https://velog.io/@gwon713/Express-winston-morgan-%EB%A1%9C%EA%B7%B8-%EA%B4%80%EB%A6%AC)

[Node.js Morgan, Winston 사용해보기](https://hidelryn.github.io/2020/04/12/nodejs-morgan-winston-example/)

[node.js 에러 핸들링 - 아키텍쳐의 빈 구멍 메꾸기](https://velog.io/@hopsprings2/node.js-%EC%97%90%EB%9F%AC-%ED%95%B8%EB%93%A4%EB%A7%81-%EC%95%84%ED%82%A4%ED%85%8D%EC%B3%90%EC%9D%98-%EB%86%93%EC%B9%9C-%EB%B6%80%EB%B6%84%EC%9D%84-%EC%88%98%ED%98%B8%ED%95%B4%EB%93%9C%EB%A6%BD%EB%8B%88%EB%8B%A4#%EB%AA%A8%EB%93%A0-%EA%B2%83%EC%9D%84-consolelogs%EB%A1%9C-%EC%B1%84%EC%9A%B4-%EC%84%9C%EB%B2%84%EB%A5%BC-%EB%A7%8C%EB%93%9C%EC%8B%A0-%EC%A0%81%EC%9D%B4-%EC%9E%88%EC%9C%BC%EC%8B%AD%EB%8B%88%EA%B9%8C)

[express에서의 비동기 에러처리](https://velog.io/@c-on/winston-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%A0%81%EC%9A%A9%ED%95%98%EB%8A%94%EC%A7%80-%EB%A7%90%EA%B3%A0-%EC%99%9C-%EA%B7%B8%EB%A0%87%EA%B2%8C-%EC%A0%81%EC%9A%A9%ED%95%98%EB%8A%94%EC%A7%80)

[[ Node ] Express 에서 Morgan과 Winston 으로 Logging 관리하기](https://velog.io/@soshin_dev/Node-Express-%EC%97%90%EC%84%9C-Morgan%EA%B3%BC-Winston-%EC%9C%BC%EB%A1%9C-Logging-%EA%B4%80%EB%A6%AC%ED%95%98%EA%B8%B0)

---

### 커스텀 에러 작성

```js
interface HttpException extends Error {
  status: number;
  message: string;
}

class HttpException extends Error implements HttpException {
  status: number;
  message: string;
  constructor(status: number, message: string){
    super(message);
    this.status = status;
    this.message = message;
  }
}

export default HttpException;
export type { HttpException };
```

### 비동기 에러 핸들링

```js
import express from 'express';
import "express-async-errors";
```

### 종합 에러 핸들러

```js
import { NextFunction, Request, Response } from "express";
import logger from "../config/winston";
import type { HttpException } from '../class/HttpError'
import config from '../config'

const errorHandler = (error:any, req: Request, res:Response, next:NextFunction)=> {
	...
  res.end(JSON.stringify(errorObj));
  next();
};

export default errorHandler;
```

에러 핸들러는 인자가 err, req, res, next로 4개여야 함

모든 라우터가 `use`를 마친 뒤에 미들웨어를 달아줘야 

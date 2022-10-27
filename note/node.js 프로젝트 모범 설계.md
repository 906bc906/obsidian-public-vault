---
title: node.js 프로젝트 모범 설계
date: 2022-10-12T17:25:24+09:00
last_modified_at: 2022-10-26T01:11:31+09:00
---
원본: https://softwareontheroad.com/ideal-nodejs-project-structure/

## 개요

Express.js는 좋은 프레임워크지만 node.js 프로젝트를 조직하는 방법에 대해서는 언급하지 않는다.

올바른 node.js 프로젝트 구조 조직은 중복 코드를 막고, 안정성을 향상시키고, 서비스를 스케일하는 데에 도움이 된다.

## 폴더 구조

```bash
src
│   app.js          # App entry point
└───api             # Express route controllers for all the endpoints of the app
└───config          # Environment variables and configuration related stuff
└───jobs            # Jobs definitions for agenda.js
└───loaders         # Split the startup process into modules
└───models          # Database models
└───services        # All the business logic is here
└───subscribers     # Event handlers for async task
└───types           # Type declaration files (d.ts) for Typescript
```

## 3계층 구조

관심사 분리 원칙을 사용하여 [비즈니스 로직 business logic](비즈니스%20로직%20business%20logic.md)을 node.js API 경로에서 멀리 옮기는 것.

단위 테스트를 위해서는 비즈니스 로직을 컨트롤러에 넣지 말고, 서비스 레이어에 넣어야 한다.

- 코드를 express.js 라우터에서 멀리 이동하라.
- req 또는 res 객체를 서비스 계층에 전달하지 말라.
- 서비스 계층의 상태 코드나 헤더와 같은 HTTP 전송 계층과 관련된 어떠한 것도 반환하지 말라.

## Pub/Sub 레이어 사용

> 종속 서비스에 대한 명령형 호출은 최선의 방법이 아니다.

간단한 "만들기" 작업으로 단일 함수에 1000줄의 코드가 포함된다. 코드를 유지관리할 수록 처음부터 책임을 분리하는 것이 좋다.

더 나은 방식은 '이메일로 가입했다' 라는 이벤트를 내보내는 것이다. 그 뒤의 일은 리스너의 책임이 된다.

## 의존성 주입

의존성 주입 활용

typedi 같은 의존성 주입 도구 소개

### node.js express 에서 종속성 주입

## 단위 테스트

## 크론 작업

setTimeout 같은 원시적인 방법에 의존하지 말고, 데이터베이스에서 작업과 실행을 지속시키는 프레임워크에 의존할 것.

[Agenda.js 사용을 권장](https://softwareontheroad.com/nodejs-scalability-issues/)하고 있음.

## configurations 및 secrets

가장 좋은 방법은 dotenv 사용.

`.env` 파일을 커밋을 하면 안 되지만 저장소에 기본값은 있어야 함.

```javascript
const dotenv = require('dotenv');
// config() will read your .env file, parse the contents, assign it to process.env.
dotenv.config();

export default {
  port: process.env.PORT,
  databaseURL: process.env.DATABASE_URI,
  paypal: {
    publicKey: process.env.PAYPAL_PUBLIC_KEY,
    secretKey: process.env.PAYPAL_SECRET_KEY,
  },
  paypal: {
    publicKey: process.env.PAYPAL_PUBLIC_KEY,
    secretKey: process.env.PAYPAL_SECRET_KEY,
  },
  mailchimp: {
    apiKey: process.env.MAILCHIMP_API_KEY,
    sender: process.env.MAILCHIMP_SENDER,
  }
}
```

코드에 `process.env.~` 와 같은 구문이 넘치는 것을 방지하고, 자동 완성 기능 덕분에 환경 변수의 모든 이름을 외울 필요도 없다.

## loader

node.js 서비스의 시작 프로세스를 테스트 가능한 모듈로 분할하는 아이디어.

고전적인 express  앱 초기화는 엄청나게 길어졌다.

```javascript
const loaders = require('./loaders');
const express = require('express');

async function startServer() {

  const app = express();

  await loaders.init({ expressApp: app });

  app.listen(process.env.PORT, err => {
    if (err) {
      console.log(err);
      return;
    }
    console.log(`Your server is ready !`);
  });
}

startServer();
```

`loader/index.js`

```javascript
import expressLoader from './express';
import mongooseLoader from './mongoose';

export default async ({ expressApp }) => {
  const mongoConnection = await mongooseLoader();
  console.log('MongoDB Initialized');
  await expressLoader({ app: expressApp });
  console.log('Express Initialized');

  // ... more loaders can be here

  // ... Initialize agenda
  // ... or Redis, or whatever you want
}
```

`loader/mongoose.js`

```javascript
import * as mongoose from 'mongoose'
export default async (): Promise<any> => {
  const connection = await mongoose.connect(process.env.DATABASE_URL, { useNewUrlParser: true });
  return connection.connection.db;
}
```

위와 같은 식으로 분할할 수 있음.

## 결론

- 3계층 아키텍처 사용
- 비즈니스 로직을 컨트롤러에 넣지 말라
- PubSub 패턴을 사용하여 백그라운드 작업에 이벤트를 보내라
- 의존성 주입을 사용하라
- 비밀번호, 비밀 및 API 키를 절대 유출하지 말고 구성 관리자를 사용하라.
- node.js 서버 구성을 독립적으로 로드할 수 있는 작은 모듈로 분할하라

[예제 리포지토리](https://github.com/santiq/bulletproof-nodejs)
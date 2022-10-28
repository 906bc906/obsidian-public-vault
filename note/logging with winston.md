---
title: logging with winston
date: 2022-10-28T15:13:14+09:00
last_modified_at: 2022-10-28T15:29:43+09:00
---
[winstonjs/winston: A logger for just about everything.](https://github.com/winstonjs/winston)

winston 은 npm logger package다.

## 왜 써야 하는가

문제 해결을 위해서는 로그를 남기는 것이 좋다. 그 중에서도 `console.log`를 비롯한 기본적인 콘솔 출력을 통한 로깅은 가장 간단한 방법이지만 그만큼 답답한 경우가 많다.

logger 에는 여러 가지가 있는데 그 중에서도 `winston`을 사용하면,

1. 로그 레벨을 나눠서 관리할 수 있다
2. 로그 포맷을 통합해서 관리할 수 있다
3. 스트림, 파일, 콘솔 등 여러 계층으로 전송할 수 있다
4. (다른 패키지를 사용하면) 로그 파일을 회전(최대 갯수, 최대 용량에 따라 가장 예전에 작성된 로그부터 지워나가는)할 수 있다

## 어떻게 쓰는가

### 설치

```bash
npm i winston winston-daily-rotate-file
```

`winston-daily-rotate-file`은 로그 파일을 회전하기 위한 패키지다. 원하지 않는다면 설치하지 않아도 좋다.

### 설정

```ts
import winston from 'winston'
import DailyRotateFile from 'winston-daily-rotate-file'
import config from './index'

const logger = winston.createLogger({
  level: config.NODE_ENV.development ? 'silly' : 'warn',
  format: winston.format.combine(
    winston.format.colorize(),
    winston.format.timestamp({
      format: 'YYYY-MM-DD HH:mm:ss'
    }),
    winston.format.printf((info) => `${info.timestamp} ${info.level}: ${info.message}`)
  ),
  transports: [
    new DailyRotateFile({  
      format: winston.format.combine(
        winston.format.uncolorize(),
        winston.format.timestamp({
          format: 'YYYY-MM-DD HH:mm:ss'
        }),
        winston.format.printf((info) => `${info.timestamp} ${info.level}: ${info.message}`)
      ), 
      dirname: "./logs",
      maxSize: '1g',
      level: 'warn'
    }),
    new winston.transports.Console()
  ]
})

export default logger;
```

위는 현재 프로젝트에서 사용중인 winston logger 설정이다.

```ts
const logger = winston.createLogger({
...
})
```

먼저 logger 를 생성해야 한다. 이후 `logger.error(~)`, `logger.warn(~)` 와 같은 형식으로 기록을 남길 수 있다.

이후 내가 logger 를 생성할 때 지정한 옵션은 `level`, `format`, `transports` 이다, 그 외에도 `levels`, `exitOnError`, `silent` 가 있으나 이는 [공식 문서](https://github.com/winstonjs/winston#creating-your-own-logger) 참고.

#### level

```ts
const logger = winston.createLogger({
  level: config.NODE_ENV.development ? 'silly' : 'warn',
```

이 옵션을 설명하기 전에 [logging level](https://github.com/winstonjs/winston#logging-levels)에 대해 알아야 한다.

winston은 `levels`를 지정하지 않으면, `npm` 로깅 레벨을 사용한다. 이는 아래와 같다.

```
{
  error: 0,
  warn: 1,
  info: 2,
  http: 3,
  verbose: 4,
  debug: 5,
  silly: 6
}
```

값이 낮을 수록 중요도가 높고, 값이 높을 수록 중요도가 낮다.

이 레벨을 지정하면, 우리는 `logger.error(~)`, `logger.silly(~)` 와 같이 레벨을 지정하여 기록할 수 있다.

이때, logger의 `level` 을 `warn` 으로 지정하면, `warn` 보다 중요도가 낮은 로그들은 전부 무시된다. 즉, `info`, `http`, ..., `silly` 로그들이 무시된다.

내 경우 개발 환경에서는 `silly` 로그까지 전부 기록되게 했고, 배포 환경에서는 `warn` 까지만 기록되게끔 했다.

#### format

```ts
const logger = winston.createLogger({
...
  format: winston.format.combine(
    winston.format.colorize(),
    winston.format.timestamp({
      format: 'YYYY-MM-DD HH:mm:ss'
    }),
    winston.format.printf((info) => `${info.timestamp} ${info.level}: ${info.message}`)
  ),
```

[로그 출력 형식을 지정](https://github.com/winstonjs/winston#formats)한다. 

포맷 지정에는 여러 방식이 있으나 `winston.format.combine` 으로 조합해서 쓰는 것이 일반적.

`winston.format.combine` 에 여러 설정을 지정하여 조합할 수 있다.

위 코드의 경우 `winston.format.colorize()` 는 레벨에 색을 입히라는 뜻이며, (유니코드로 동작하며 콘솔에서 색을 확인할 수 있음, 일반 텍스트 에디터로 확인시 유니코드가 그대로 노출된다)

`winston.format.timestamp` 는 타임스탬프 형식을 지정한다.

이렇게 지정만 한 경우 아무것도 로깅되지 않기 때문에, 구체적으로 '어떻게 출력할 것인가' 를 작성해줘야 한다.

`winston.format.printf` 로 직접 지정해줄 수 있고, `printf` 외에는 개발자가 미리 작성해둔 `json`, `logstash`, `prettyPrint`, `simple` 출력 형식을 선택할 수도 있다.

#### transports

```ts
const logger = winston.createLogger({
...
  transports: [
    new DailyRotateFile({  
      format: winston.format.combine(
        winston.format.uncolorize(),
        winston.format.timestamp({
          format: 'YYYY-MM-DD HH:mm:ss'
        }),
        winston.format.printf((info) => `${info.timestamp} ${info.level}: ${info.message}`)
      ), 
      dirname: "./logs",
      maxSize: '1g',
      level: 'warn'
    }),
    new winston.transports.Console()
  ]
```

그래서 이렇게 작성된 로그를 어디로 전송할 것인가? 를 지정한다.

내 경우에는 `DailyRotateFile`과 `winston.transports.Console`에 전송한다.

`DailyRotateFile`은 앞서 말했던 로그 파일을 회전시키는 역할을 하는 패키지 클래스다. 이 역시 [다양한 옵션](https://github.com/winstonjs/winston-daily-rotate-file)을 지정할 수 있는데, 내 경우에 `DailyRotateFile` 만의 옵션은 `dirname`, `maxSize` 뿐이다. 이 옵션들은 이름만 봐도 알 수 있듯 각각 로그 파일의 저장 경로, 로그 파일들의 최대 크기다. 

나머지는 윈스턴에서 파생된 옵션인데, `format`과 `level`은 앞선 `createLoggeer` 에서 봤던 옵션이다. 즉, `Logger` 에 적힌 옵션들은 default로 동작하되, `transports` 에서 각 `transport`별로 `format`, `level` 등을 지정하면 이를 우선한다.

내 경우 로그를 파일로 기록할 때 `colorize` 하는 경우 컬러 유니코드가 로그 가독성을 해치기 때문에 색칠하지 않도록 하였고, 개발 환경이라 할지라도 콘솔로 충분히 확인할 수 있기 때문에 `warn` 미만의 레벨을 갖는 로그는 기록하지 않고 싶어서 `level`을 별도로 `warn`으로 지정했다.

`winston.transports.Console` 의 경우 말 그대로 콘솔에 출력한다. 여기에도 옵션을 지정할 수 있으나 나는 `logger`의 기본값을 그대로 재사용하는 의미로 옵션을 지정하지 않고 비워뒀다.

이후 이렇게 작성한 `logger`를 여기저기서 불러와서 쓰면 된다.

```ts
  if (!AppDataSource.isInitialized) {
    await AppDataSource.initialize();
    logger.info("TypeORM Datasource: Initialized successfully.");
  }
```

이런 느낌으로 쓰고 있다.

### 부록

#### socket.io

```js
socket.onAny((eventName:string, ...args:any[]) => {
	logger.info(`socket ${socket.id} : ${eventName} / ${args}`);
})
```
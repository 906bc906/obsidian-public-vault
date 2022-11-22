---
title: node.js package.json NODE_ENV 윈도우 문제
date: 2022-10-27T19:26:19+09:00
last_modified_at: 2022-11-22T20:09:21+09:00
---
## 문제

```bash
$ npm run dev
npm WARN config global `--global`, `--local` are deprecated. Use `--location=global` instead.

> backend@0.0.1 dev
> NODE_ENV=development nodemon --exec ts-node --files src/app.ts

'NODE_ENV'은(는) 내부 또는 외부 명령, 실행할 수 있는 프로그램, 또는
배치 파일이 아닙니다.
```

윈도우에서는 cmd, powershell 모두 위와 같은 형식의 환경 변수 설정이 불가능하다.



## 해결

https://www.npmjs.com/package/cross-env

cross-env를 이용한다.
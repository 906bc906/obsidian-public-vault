---
title: express-session
date: 2022-10-13T17:41:29+09:00
last_modified_at: 2022-11-22T20:09:22+09:00
---
express 에서 session 을 쉽게 취급하게 해주는 미들웨어.

```js
var session = require('express-session')
//or
import session from 'express-session';
```

## Options

```js
`{ 
	path: '/',
	httpOnly: true, 
	secure: false, 
	maxAge: null 
}`.
```

기본값.


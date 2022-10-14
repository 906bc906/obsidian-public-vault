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


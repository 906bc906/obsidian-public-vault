---
title: socket.io error handling (작성중)
date: 2022-10-28T15:32:39+09:00
last_modified_at: 2022-11-22T20:09:21+09:00
---
socket.io는 별도로 에러 핸들링 수단을 제공하지 않음

이벤트 발생 뒤에는 미들웨어도 붙일 수 없기 때문에 알아서 해야 한다

```js
  const asyncFuncDetector = (func: any) => func.constructor.name === 'AsyncFunction'
  ? asyncFuncRunner(func)
  : syncFuncRunner(func);

  //check error
  const syncFuncRunner = (func: any) => (...args: any[]) => {
    try {
      func(...args);
    }
    catch (e: any) {
      errorHandler(e);
    }
  }
  
  const asyncFuncRunner = (func: any) => async (...args: any[]) => {
    try {
      await func(...args);
    }
    catch (e: any) {
      errorHandler(e);
    }
  } 

  const errorHandler = (e: Error) => {
    socket.emit(events.error, e.message);
    if (config.NODE_ENV.development)
      logger.error(e);
  }
  
  //wrap error handler
  for (let key in _controllers) {
    _controllers[key] = asyncFuncDetector(_controllers[key]);
  }
```

내 해결 방법은 이벤트 핸들러에 미리 에러 핸들러를 부착하는 방식

`asyncFuncDetector` -> `(a)syncFuncRunner` -> `func(...args)` -(error)-> `errorHandler(e)`

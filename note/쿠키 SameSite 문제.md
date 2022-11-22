---
title: 쿠키 SameSite 문제
date: 2022-10-18T14:19:38+09:00
last_modified_at: 2022-11-17T01:31:45+09:00
---
이 문제는 [express 프론트, 백 서버 분리 후 세션 쿠키 관리](express%20프론트,%20백%20서버%20분리%20후%20세션%20쿠키%20관리.md) 에서 파생되었음.

## 문제

프론트엔드 서버와 백엔드 서버가 다른 도메인을 사용하고 있는 경우, `samesite` 문제가 생김.

프론트엔드 컴퓨터에서는 앱 서버로 `http://localhost:3000`, API 서버는 `http://<백엔드 컴퓨터 ip>:4000` 에 접속하고 있는 상황이었다. 

![](attachments/Pasted%20image%2020221018135050.png)

![](attachments/Pasted%20image%2020221018135103.png)

> 이 Set-Cookie 헤더는 "SameSite" 속성을 지정하지 않고 "SameSite=Lax" 로 기본 설정되었습니다. 또한 최상위 탐색에 관한 응답이 아닌 크로스 사이트 응답에서 발생하여 차단되었습니다. Set-Cookie는 크로스 사이트 사용이 가능하도록 "SameStie=None" 으로 설정되어야 합니다.

프론트엔드 컴퓨터 입장에서, `localhost` 와 `<백엔드 컴퓨터 ip>` 는 다른 도메인이므로 쿠키 사용을 거부한 것.

브라우저가 왜 이런 정책을 사용하는지는 다음 아티클을 통해 이해할 수 있다: [새로운 SameSite=None; Secure 쿠키 설정에 대비  |  Google 검색 센터 블로그  |  Google Developers](https://developers.google.com/search/blog/2020/01/get-ready-for-new-samesitenone-secure?hl=ko)

## 배경 이해

`SameSite` 속성은 리퀘스트가 `same-site` 인 경우에만 쿠키가 부착되도록 범위를 제한하는 옵션이다.[^ietf]

`SameSite` 의 속성은 다음과 같다.

값|설명
---|---
Strict| "same-site" 요청인 경우에만 쿠키가 전송된다.
Lax|(기본값) "same-site" 요청인 경우 및 "cross-site"에서의 top-level navigation 일부에서 쿠키가 전송된다.
None|SameSite를 검증하지 않는다. 쿠키가 `secure` 해야 한다. (https)

이것만 봐서는 어떤 내용인지 이해하기가 어렵다.

Strict, Lax, None 에 대한 내용을 이해하기 쉬운 유튜브 영상: https://www.youtube.com/watch?v=aUF2QCEudPo

### Same-site가 뭐야?

그래서, SameSite와 CrossSite를 판별하는 기준이 무엇인지부터가 애매하다.

잘 정리된 글
- [SameSite vs SameOrigin](https://stitchcoding.tistory.com/m/46)
- [Understanding "same-site" and "same-origin"](https://web.dev/same-site-same-origin/) 

요약하자면,

-|same origin|same site|
---|---|---
scheme(http)|v|
domain|v|eTLD+1
subdomain|v|
port|v|

![](https://web-dev.imgix.net/image/admin/PX5HrIMPlgcbzYac3FHV.png?auto=format&w=741)

`sameorigin`은 scheme, host name, port가 모두 같아야 하며,

![](https://web-dev.imgix.net/image/admin/oSRJzCJIr4OjGzUhcNDP.png?auto=format&w=741)
`sameSite` 는 eTLD+1이 같으면, scheme, subdomain, port는 상관 없다.

TLD(Top-Level Domain) 이면 TLD지, 왜 eTLD(effective Top-Level Domain) 인가?

이는 `Public Suffix List` 와 관련이 있는데, 쿠키를 취급할 때 TLD 만으로는 문제가 되는 경우가 생기기 때문이다.

`github.io` 의 경우 TLD는 `io` 지만, TLD+1 을 기준으로 samesite 를 판별하면 문제가 생길 수 있다.

왜냐하면 github.io 의 서브 도메인의 관리를 github.io가 아닌 유저가 하기 때문이다. 당장 이 블로그만 하더라도 `906bc906.github.io` 이며 TLD+1 을 기준으로 쿠키를 관리하면 내가 악의적으로 `github.io`에 발급된 쿠키를 빼올 수 있다.

이런 경우까지 고려하기 위해 `Public Suffix List` 를 관리하며, 이를 sameSite 판별 기준에 포함한다.

## 참조

https://jub0bs.com/posts/2021-01-29-great-samesite-confusion/

[^ietf]: https://datatracker.ietf.org/doc/html/draft-ietf-httpbis-cookie-same-site-00

https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies

https://www.youtube.com/watch?v=aUF2QCEudPo
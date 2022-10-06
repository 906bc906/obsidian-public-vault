# oauth
<!--Basic Template V0.0.2 Start -->
```dataview
TABLE WITHOUT ID  file.link AS title, tags
FROM -"templates"
SORT file.mtime, tags desc
WHERE contains(tags, [[]])
```
<!--Basic Template V0.0.2 End -->
[tags::[[authorization 인가]]]

Open Authorization

[RFC 6749: The OAuth 2.0 Authorization Framework](https://www.rfc-editor.org/rfc/rfc6749)

## 개요

OAuth 사용 이전, 사용자들은 기본적으로 아이디와 비밀번호를 이용하여 인증했다.

## 용어 설명

- Resource Owner
	- 보호 자원에 접근할 수 있는 엔티티. resource-owner가 사람인 경우, end-user를 뜻한다.
- Resource Server
	- 보호 자원을 호스팅하는 서버. Access Token들을 이용한 보호 자원 요청을 받아들이고 응답할 수 있다.
- Client
	- 보호 자원을 요청하는 애플리케이션.
- Authorization server
	- Resource Owner 임을 인증하고 성공적으로 인가된 후 Client에게 Access Tokens를 발급하는 서버.

본 문서에서는 편의를 위해 Resource Server 와 Authorization server를 구분하지 않음.

- Refresh Token
	- Access Token 은 수명이 정해져있다. 수명이 다 될 때마다 사용자에게 요청하는 것은 너무나도 피곤하기 때문에, Refresh Token을 이용하여 Access Token을 재발급한다.
	- Access Token을 발급할 때 Refresh Token도 발급한다.
	- Client는 Access Token이 유효하지 않아지면, Authorization server에게 refresh token 을 발급하여 새로운 Access Token을 발급받는다. (Refresh Token는 이때 갱신될 수도 있고, 아닐 수도 있다)

## 절차

### 1. 등록
Client가 Resource Server에 사전 등록을 해야 함.

Client ID, Client Secret, Authorized redirect URIs를 발급 받는다.

Secret은 절대 유출되면 안 된다.

### 2. Resource Owner의 승인

Resource Owner가 Client를 거쳐 Resource server의 어떤 작업을 해야되는 상황.

Resource Owner가 해당 동작을 할 것임을 Client에게 알리면, (share 버튼을 눌렀다던지)

Client가 Resource server 주소에 client id, scope, redirect uri를 붙인 주소를 Resource Owner에게 제공. (sign with github 같은)

Client는 해당 주소로 접속.

Resource server는 받은 client id와 redirect_uri가 자신이 가지고 있는 정보와 일치하는지 확인한 후 같다면 Resource owner에게 scope 에 해당하는 권한을 부여할 것인지 물어본다. (Client 앱이 이러한 내용 or 권한을 요구했는데 허용할 것인가?)

Resource Owner가 동의한 경우 Resource server에 해당 Resource Owner가 특정 Scope에 동의하였음을 기록한다.

### 3. Resource Server의 승인

Resource server는 Resource owner에게 redirect URI에 파라미터로 authorization code를 붙여 주소를 제공한다.

Resource Owner가 해당 Redirect URI로 접속하게 되면 Client는 해당 유저의 code를 알게 된다.

Client는 Resource Server에게 client id, client secret, redirect uri, code 를 파라미터로 붙여 Access Token을 요청한다.

### 4. Access Token 발급

Resource Server는 해당 정보들이 유효한지 검증한 후 Access Token을 Client에게 발급한다.

Access Token 를 가진 Client는 Resource server에게 Access Token을 통해 Resource Owner의 Scope 에 해당하는 정보, 또는 행동을 요청할 수 있게 된다.


## Grant Types

### Authorization Code

### PKCE

### Client Credentials

### Device Code

### Refresh Token

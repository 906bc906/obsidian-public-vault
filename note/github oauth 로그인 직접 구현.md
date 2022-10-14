https://docs.github.com/en/developers/apps/building-oauth-apps/authorizing-oauth-apps

## Web application flow

1. 유저가 깃헙 신원 요청을 위해 리다이렉트됨
2. 깃헙에 의해 기존 사이트로 다시 돌아오게끔 리다이렉트됨
3. 앱이 유저의 access token을 API로 받음

### 1. Request a user's Github Identity


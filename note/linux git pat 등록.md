<!--Basic Template V0.0.2 Start -->
```dataview
TABLE WITHOUT ID  file.link AS title, tags
FROM -"templates"
SORT file.mtime, tags desc
WHERE contains(tags, [[]])
```
<!--Basic Template V0.0.2 End -->
[tags::[[git 깃]], [[linux]]]

## 왜

```bash
support for password authentication was removed 
```

private repo를 clone하다 보면 위와 같은 오류가 발생한다.

패스워드 인증을 제거하면서 생긴 일인데, 깃헙에서는 대안으로 PAT 를 제공한다.

## PAT

https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token

PAT를 생성하는 방법은 위 링크를 참고한다. 거의 대부분의 권한을 허용해야 한다.

이후 다시 git clone을 할 때 비밀번호를 물어볼 때 비밀번호 대신 PAT를 입력하면 된다.

https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-Credential-%EC%A0%80%EC%9E%A5%EC%86%8C

## Credential Helper

매번 PAT를 확인하는건 귀찮으므로 credential.helper를 이용해야 한다.

이후 위 링크를 참고하여 credential.helper를 설정한다.

`git config --global credential.helper 'store --file ~/.git-credentials'` 정도로 적으면 된다. credential 모드는 store로 하며 credentials는 \~/.git-credentials 에 저장한다는 의미.

cache 모드도 있으나 지정한 시간이 지나면 삭제되는 모드이므로 굳이 써야되나 싶다.

## 후기

PAT도 지정한 크레덴셜 파일에 평문으로 그대로 노출되므로 PAT 관리에 유의해야 한다.
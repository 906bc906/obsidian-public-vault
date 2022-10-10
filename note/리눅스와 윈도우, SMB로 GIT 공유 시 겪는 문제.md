---
title: 리눅스와 윈도우, SMB로 GIT 공유 시 겪는 문제
date: 2022-10-10T00:38:53+09:00
last_modified_at: 2022-10-10T19:12:32+09:00
---

컴퓨터는 윈도우, 노트북은 리눅스를 사용하고 있고, 간이NAS에 repo를 두고 OS를 번갈아가면서 쓰고 있는데 좀 많은 문제를 겪고 있다.

## safe directory

오류 뜨는 거 그대로 허용해준다.

## 개행 문자 문제

[git 에서 CRLF 개행 문자 차이로 인한 문제 해결하기](https://www.lesstif.com/gitbook/git-crlf-20776404.html)

윈도우는 CRLF, 유닉스와 맥은 LF를 사용하면서 이로 인한 diff 가 엄청나게 발생한다.

윈도우에서 LF로 설정한다.

```bash
git config --global core.eol lf
```

## 파일 모드 문제

거의 대부분의 파일이 100755 100644 모드 문제를 겪고 있었다.

https://codecooking.tistory.com/113

바람직한 해결법은 아니지만 지금 당장 해결하기 위해서 repo에서 filemode를 비활성화했음.
---
title: blocking non-blocking sync async 구분
tags:
- todo
---

<!--ankiQ-->
blocking, non-blocking 은 {1:호출되는 함수가 바로 리턴하느냐 마느냐} 가 기준이며, sync, async는 {2:호출되는 함수의 작업 완료 여부를 누가 신경쓰느냐} 가 관심사다.

- blocking: {3:바로 리턴하지 않는다.}
- non-blocking: {4:바로 리턴한다.}
- sync: {5:호출되는 함수의 작업 완료 여부를 **호출하는 함수**가 신경쓴다.}
- async: {6:호출되는 함수의 작업 완료 여부를 **호출되는 함수**가 신경쓴다.}
<!--ankiE-->
<!--ID: 1664360756815-->

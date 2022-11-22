---
title: Anissia ICalendar
date: 2022-11-01T08:54:15+09:00
last_modified_at: 2022-11-22T20:09:22+09:00
---
## 개요

[Anissia](https://anissia.net/)의 API를 이용하여 1주일간의 애니메이션 방영 일정을 iCalendar 형식으로 배포한다.

## 기술

- 언어
	- 자바스크립트
- 자바스크립트 런타임
	- Node.js
- 종속성 관리
	- NPM
- 패키지
	- ical-generator
		- ical 을 쉽게 생성할 수 있게 함.
	- @touch4it/ical-timezones
		- ical에 타임존이 제대로 인식되려면 ical 내부에 VTIMEZONE이 설정되어야 함.
		- ical-generator 의 옵션에서 단순히 타임존 설정하는 것으로는 TZID만 설정되고 VTIMEZONE 생성이 안 된다.
		- VTIMEZONE 생성기를 지정해야 하고 이를 위해 이 패키지를 사용함.
	- axios
		- HTTP 클라이언트
		- node.js 16 기준이라 fetch가 내장이 안 돼서 못 쓰고, 대안으로 사용.
	- luxon
		- datetime wrapper
		- ical-generator가 지정한 Datetime 형식 중 하나라 사용해봄.
- 자동화
	- Github Actions
		- 6시간 주기 갱신을 위해 사용
		- 로컬 테스트로는 act를 사용

## 겪었던 문제

- API와 luxon의 weekday 범위가 다름 (0~6, 1~7)
	- 일요일을 7에서 0으로 보내면 되는 문제라 `% 7` 사용
- 구글 캘린더 갱신 문제
	- 구글 캘린더에서, Github Actions로 생성된 파일을 URL로 import하면 TIMEZONE 변경한 내역이 반영이 안 됐다.
	- 반대로, 파일을 URL이 아니라 다운로드하여 로컬에서 첨부하면 반영이 됐다.
	- 뭘 잘못한 것일까, 하고 한참을 TIMEZONE 세팅 뻘짓을 했는데, 알고 보니 구글 캘린더쪽의 문제였다.
		- 구글 캘린더는 iCalendar를 최대 12시간 주기로 갱신한다.
		- 아마도 서버 쪽에서 글로벌하게 URL 단위로 캐싱하는 듯 하다.
			- 왜냐하면, 주소를 바꾸자마자 URL import도 칼같이 변경됐기 때문.
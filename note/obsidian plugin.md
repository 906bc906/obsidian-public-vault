---
title: obsidian plugin
date: 2022-10-10T18:39:12+09:00
last_modified_at: 2022-10-11T00:23:24+09:00
---

## 사용중

- Obsidian dataview
	- https://github.com/blacksmithgu/obsidian-dataview
	- 옵시디언 볼트를 데이터베이스로 사용하여 동적으로 쿼리할 수 있게 함.
	- 특정 태그의 노트만 표시한다거나 체크하지 않은 task를 표시 등 다양하게 활용할 수 있음
	- 다만 옵시디언에서만 동작하는 플러그인인데다, 정적으로 결과를 남겨주지 않아 볼트를 표준 마크다운으로 관리할 생각이라면 최소한으로 사용해야 함.
- Copy Button for code blocks
	- https://github.com/jdbrice/obsidian-code-block-copy
	- 코드 블럭에 복사 버튼을 띄워준다.
- Homepage
	- https://github.com/mirnovov/obsidian-homepage
	- 옵시디언을 열었을 때 특정 노트를 띄운다.
	- 메인 노트로 바로 가게끔 설정하고 쓰고 있음.
- Linter
	- https://github.com/platers/obsidian-linter
	- 옵시디언 노트 포맷을 세팅할 수 있게 해줌.
	- 다른 것보다 yaml frontmatter에 file system 기반으로 title, creation date, modified date를 자동으로 세팅하는 기능이 마크다운 문서를 기타 블로그로 export할 때 굉장히 유용함.
	- 일괄 lint도 된다. 강력 추천
- Minimal theme settings, style settings
	- 현재 옵시디언 테마로 Minimal 을 쓰고 있는데, 그거 관련 tweak 플러그인
	- 다크 모드를 시스템 설정과 연동하는게 좋다
	- Minimal theme 안 쓰면 굳이 사용할 이유 없음
- Obsidian git
	- https://github.com/denolehov/obsidian-git
	- 의미 그대로 obsidian에서 git을 사용할 수 있게 함
	- source control view도 띄울 수 있고, diff도 확인할 수 있음
	- 주기적 자동 동기화(pull and push)도 지원하고 서브모듈 관리도 가능함.
	- 서브모듈까지 일괄 동기화해주는 backup 기능이 자주 고장남 (SMB에 올려서 그런듯)
- Obsidian tabs
	- https://github.com/gitobsidiantutorial/obsidian-tabs
	- 옵시디언이 여러 노트를 띄워놓고 쓰기가 굉장히 불편함
	- 이에 대한 대안으로 sliding panes 같은걸 많이들 쓰는데, 개인적으로는 탭을 더 선호함
	- 옵시디언 v0.16.3 부터 자체적으로 탭을 지원할 예정이라, 내장 탭이 쓸만하면 갈아탈수도
- Pane relief
	- https://github.com/pjeby/pane-relief
	- 탭별 단축키, 탭 히스토리 등 탭으로 관리할 때 굉장히 편한 단축키를 제공함
	- Obsidian tabs 와 같이 쓰면 굿
	- v0.16.3 내장 탭도 지원
- Paste url into selection
	- https://github.com/denolehov/obsidian-url-into-selection
	- 선택한 텍스트에 붙여넣기 하면 url을 붙여줌
	- 코드 블럭에도, url이 아닌데도 셀렉션 했다 하면 무조건 붙여버리는 경우가 있기 때문에 조심해서 써야 함
- Simple Embeds
	- https://github.com/samwarnick/obsidian-simple-embeds
	- 별도 마크다운 없이 링크만 달아도 유튜브 등을 임베드 형식으로 띄워줌
- Typewriter scroll
	- https://github.com/deathau/cm-typewriter-scroll-obsidian
	- 타자기를 쓰는 것처럼, 커서가 문서의 정중앙에 오게끔 스크롤을 조정해주는 플러그인

## 사용 고려중
- Obsidian Better Code Block
	- https://github.com/stargrey/obsidian-better-codeblock
	- 다른 마크다운으로 export했을 때 문제가 없는지 검증할 필요가 있음
- Outliner
	- https://github.com/vslinko/obsidian-outliner
	- 리스트 편집할 때 굉장히 편함

## 사용 중지
- Enhancing Mindmap
	- 이외에도 마인드맵 계열 플러그인들을 포함
	- xmind 등의 상용 마인드맵 프로그램과 비교하면 아직까진 마인드맵 플러그인들이 쓰기 불편함
	- 특히 내용을 편집하다가 마인드맵이 깨지는 경우가 많음
- Flashcards
	- Anki 동기화 플러그인
	- 커스텀 정규식 지정이 불가능해서 사용 중단함
- QuickAdd
	- 정해진 포맷의 새 노트를 빠르게 만들 수 있게 해줌
	- 어떤 노트의 어느 위치에 링크를 붙인다던가, 노트를 만들 때 다이얼로그로 미리 입력받는 등 재밌는 걸 많이 할 수 있음
	- 볼트 포맷 갈아엎으면서 쓸 일이 없어져서 일단은 비활성화했는데, 잘 쓰면 생산성 많이 좋아지는 플러그인
- Regex find/replace
	- 이름 그대로 정규표현식으로 find/replace 지원
	- 문서 단위로 하나하나 해야되고 벌크 편집이 안되길래 비활성화 함
	- 벌크 편집은 VScode 이용해서 하는중
- Templater
	- Obsidian 기본 템플릿 언어는 굉장히 제한적인데, Templater는 템플릿 언어로 여러 가지 것들을 사용할 수 있게 해줌
	- 처음에는 filename, date를 달아주기 위해서 사용했는데 Linter로 대체함
- Update time on edit
	- frontmatter의 creation date, modified date를 갱신해주는 플러그인
	- 파일 기준으로 일자를 설정하는게 아니라, 에디터 진입 시간 기준으로 설정하기 때문에 오차가 굉장히 큼
	- 수정하지도 않았는데 열은 것만으로 modified date가 갱신되는 문제가 있음
	- Linter로 대체함
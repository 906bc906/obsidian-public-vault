---
title: github actions - overview
date: 2022-10-31T14:48:10+09:00
last_modified_at: 2022-11-22T20:09:22+09:00
---
[Understanding GitHub Actions - GitHub Docs](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions)

## Overview

깃헙 액션은 CI/CD 플랫폼. 빌드, 테스트, 배포 파이프라인을 자동화함.

workflow를 만들어 모든 PR을 빌드, 테스트할 수 있고 병합된 PR을 프로덕션에 배포할 수 있음.

깃헙 액션은 DevOps를 넘어 리포지토리에서 다른 이벤트가 발생할 때 workflow를 실행할 수 있음. 저장소에 새 이슈를 생성할 때마다 적절한 레이블을 자동으로 추가하는 워크플로를 실행할 수 있음.

워크플로를 실행하기 위한 Linux, Windows, MacOS 가상 머신을 제공하거나 자체 데이터 센터 또는 클라우드 인프라에서 실행 가능.

## Github Actions의 구성 요소

![github actions components flow](https://docs.github.com/assets/cb-25535/images/help/images/overview-actions-simple.png)

- `workflow`
	- `event`
		- `workflow` 실행 조건
	- `runner` (or `container`)
		- `job`을 실행하는 가상 머신
		- `job`
			- 여러 개의 `step` 으로 구성되어 있음
			- `step`
				- 스크립트를 실행함

### Workflow

하나 이상의 job을 실행하는, 구성 가능한 자동화 프로세스

YAML 로 작성

[Using workflows - GitHub Docs](https://docs.github.com/en/actions/using-workflows)

[Reusing workflows - GitHub Docs](https://docs.github.com/en/actions/using-workflows/reusing-workflows)

### Event

workflow를 실행하는 조건.

[Repositories - GitHub Docs](https://docs.github.com/en/rest/repos/repos#create-a-repository-dispatch-event)

[Events that trigger workflows - GitHub Docs](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows)

### Runner

event에 의해 workflow가 트리거될 때 이를 실행하는 서버.

각 러너는 한 번에 하나의 작업을 실행할 수 있음.

[Using larger runners - GitHub Docs](https://docs.github.com/en/actions/using-github-hosted-runners/using-larger-runners)

[Hosting your own runners - GitHub Docs](https://docs.github.com/en/actions/hosting-your-own-runners)

### Jobs

워크플로의 일련의 단계

다른 job과의 종속성을 구성할 수 있다.

[Using jobs - GitHub Docs](https://docs.github.com/en/actions/using-jobs)

### Actions

Github Actions를 위한 커스텀 앱. 

직접 고유 액션을 만들거나, Github Marketplace에서 찾아서 사용할 수 있다.

[Creating actions - GitHub Docs](https://docs.github.com/en/actions/creating-actions)


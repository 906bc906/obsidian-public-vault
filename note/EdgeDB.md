---
title: EdgeDB
date: 2022-10-10T00:50:18+09:00
last_modified_at: 2022-10-26T01:11:31+09:00
---


## 시작

### CLI

#### 설치

```bash
curl https://sh.edgedb.com --proto '=https' -sSf1 | sh
```

```bash
info: downloading installer

Welcome to EdgeDB!

This will install the official EdgeDB command-line tools.

The edgedb binary will be placed in the user bin directory located at:
/home/laptop/.local/bin

This path will then be added to your PATH environment variable by modifying the 
profile file located at:
/home/laptop/.profile

┌──────────────────────┬─────────────────────────┐
│ Installation Path    │ /home/laptop/.local/bin │
│ Modify PATH Variable │ yes                     │
│ Profile Files        │ /home/laptop/.profile   │
└──────────────────────┴─────────────────────────┘
1) Proceed with installation (default)
2) Customize installation
3) Cancel installation
1
The EdgeDB command-line tool is now installed!

We've updated your shell profile to have /home/laptop/.local/bin in your PATH
environment variable. Next time you open the terminal it will be
configured automatically.

For this session please run:
source "/home/laptop/.config/edgedb/env"
To initialize a new project, run:
edgedb project init
```

#### 프로젝트 초기화

```bash
edgedb project init
```

`edgedb.toml` config 파일이 생성되고 `default.esdl` 스키마 파일이 생성됨.

프로젝트를 생성한 디렉토리와 EdgeDB 인스턴스가 연결됨. 해당 디렉토리에서 `edgedb` 호출 시 해당 인스턴스 사용.

인스턴스 관리하는 방법은 [주소](https://www.edgedb.com/docs/intro/instances#ref-intro-instances) 참조

실행중인 인스턴스는 `edgedb instance list`로 확인할 수 있음

#### 스키마 설정

`default.esdl`을 아래와 같이 편집.

```
module default {
  type Person {
    required property name -> str;
  }

  type Movie {
    property title -> str;
    multi link actors -> Person;
  }
};
```

- id 속성을 포함하지 않음. EdgeDB는 `id`를 자동으로 만들고, DB에 추가되는 매 오브젝트에 유니크한 UUID를 할당함.
- `Movie` 타입은 두 개의 **links**를 포함하고 있음: `actors`, `director` (???) EdgeDB에서 링크는 오브젝트 타입간의 관계를 표시하기 위해 사용됨. 외래 키를 사용할 필요가 없음. 이를 통해 JOIN들 없이 딥한 쿼리를 쉽게 짤 수 있게 되는지 알게 될 것임.
- `default` 라고 불리는 `module` 안에 오브젝트 타입들이 있음. 스키마를 논리적 단위(모듈)로 쪼갤 수 있음. 물론 `default` 단일 모듈에 전체 스키마를 정의하는 것이 일반적임.

#### migration

`edgedb migration create` 로 migration file을 생성할 수 있음. `*.esdl` 파일을 수집해 데이터베이스로 전송함. DB가 이 파일들을 파싱해서, 현재의 스키마와 비교하여, migration 계획을 생성함. 데이터베이스는 이 계획을 CLI로 전송하고, CLI가 migration file을 생성하게됨.

migration file은 생성되었지만 DB에 적용하지 않은 상태임.

`edgedb migrate` 로 적용.

```bash
$ edgedb list types
┌─────────────────┬──────────────────────────────┐
│ Name            │ Extending                    │
├─────────────────┼──────────────────────────────┤
│ default::Movie  │ std::BaseObject, std::Object │
│ default::Person │ std::BaseObject, std::Object │
```

잘 적용된 것을 확인할 수 있음.

```
  type Movie {
    property title -> str; //이 부분을
    required property title -> str; //이렇게 수정
    multi link actors -> Person;
  }
```

```
$ edgedb migration create
did you make property 'title' of object type 'default::Movie' required? [y,n,l,c,b,s,q,?]
> y
Please specify an expression to populate existing objects in order to make property 'title' of object type 'default::Movie' required:
fill_expr> "untitled"
Created dbschema/migrations/00002.edgeql, id: m1mdw3zntw5yfya4yfbjrvtyyzbzmao7yqdmkcrffvr4aran2oysaa
```

migration file을 다시 만드니,
1. 내가 바꾼 것이 맞는지 확인한 후,
2. required 로 바꾸면서, 값이 지정되지 않은 오브젝트들의 값은 무엇으로 지정할 것인지 물어봄.

이후 `edgedb migrate` 로 적용.

#### 쿼리 작성

```bash
edgedb ui
```

위 명령어로 어드민 대시보드를 열 수 있다.

현재의 '인스턴스' 의 UI를 연 것이고, 이 '인스턴스' 안에서 '데이터베이스'를 만들어볼 수 있음. 일단은 테스트를 위해 기존에 있는 `edgedb`를 사용해보자.

![Pasted image 20220929122730](attachments/Pasted%20image%2020220929122730.png)

Open REPL 클릭하여 쿼리를 작성해보자.

```
insert Movie {
  title := "Dune"
};
```

```
update Movie
filter .title = "Dune"
set {
  actors := {
    (insert Person { name := "Timothee Chalamet" }),
    (insert Person { name := "Zendaya" })
  }
};
```

```
select Movie {
  title,
  actors: {
    name
  }
};
```

위 쿼리를 거치면 아래의 결과가 나온다.

```
[
  {
    "title": "Dune",
    "actors": [
      { "name": "Timothee Chalamet" },
      { "name": "Zendaya" }
    ]
  }
]
```

UI는 개발 도구이므로 결국에는 각자의 서버에서 edgeDB를 사용해야하는데, 다양한 공식 [클라이언트 라이브러리](https://www.edgedb.com/docs/intro/clients#ref-intro-clients)가 있으므로 찾아보도록 한다.

### 배포

https://www.edgedb.com/docs/guides/deployment/index#ref-guide-deployment


## 비교

### vs SQL

- SQL은..
	- 스키마가 CREATE, ALTER, DELETE, / TABLE, COLUMN 커맨드의 연속으로 이루어져 있다. 한 눈에 스키마를 파악하기 어려움.
	- SQL은 데이터를 상대적으로 보관한다. 테이블들간의 연결은 외래 키로 표시되어 JOIN 연산을 사용해야 함.
- EdgeDB는..
	- 테이블들간의 연결을 link 로 나타낸다.
	- JOIN을 쓸 필요가 없다.

```
select Movie {
  title,
  director: {
    name
  }
}
```

### vs ORM

- ORM은..
	- 모던 OOP 언어 컨셉에 자연스러운 방식으로 스키마를 모델링하고 쿼리를 작성할 수 있게 함
	- 하지만,
		- 스키마가 ORM 라이브러리에 강하게 종속됨.
		- 특정 프로그래밍 언어 사용이 강제됨.
		- 제한된 쿼리 기능.
		- 성능 문제가 있음.
		- Migration 이 어려울 수 있음.
- EdgeDB는..
	- ORM의 좋은 점만 가져옴
		- 선언적 모델링
		- 오브젝트 기반 API
		- 직관적인 쿼리


### 예제

https://github.com/edgedb/edgedb-examples


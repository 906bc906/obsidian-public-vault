---
title: neo4j getting started
date: 2022-11-13T20:25:31+09:00
last_modified_at: 2022-11-22T20:09:22+09:00
---
## 설치

윈도우 기준입니다.

https://neo4j.com/docs/operations-manual/current/installation/windows/

### JDK 설치

OpenJDK나 Oracle JDK를 설치합니다.

저는 [OpenJDK 18](https://jdk.java.net/18/)를 설치했습니다만 Neo4j가 JDK 17을 설치하라고 경고하네요. 여러분들은 17을 설치하세요.

윈도우용 zip 을 설치해서 적당한 곳에 압축 해제합니다. 저는 `C:\path\jdk` 에 설치했습니다.

![](attachments/Pasted%20image%2020221113153046.png)

윈도우 시스템 변수에 JAVA_HOME을 만들고 압축 해제한 경로를 적어줍니다.

![](attachments/Pasted%20image%2020221113153122.png)

이후 시스템 변수 중 Path를 편집하여 `%JAVA_HOME%\bin` 을 추가합니다.

```
C:\Users\User>javac --version
javac 18.0.2.1
```

cmd를 열고 `javac --version` 을 입력해봅니다. 버전이 나오면 됩니다.

### neo4j server 설치

[Desktop 을 설치하는 방법](https://wikidocs.net/50743)도 있고, [Docker로 실행하는 방법](https://neo4j.com/developer/docker-run-neo4j/)도 있습니다.

저는 둘 다 패스하고 서버를 직접 윈도우에 설치하겠습니다.

https://neo4j.com/download-center/#community

가장 최신 버전인 5.1.0 을 다운로드 받고 적당한 곳에 압축 해제합니다.

저는 `C:\path\neo4j` 에 해제했습니다.

![](attachments/Pasted%20image%2020221113153844.png)

해제한 경로 뒤에 `\bin` 을 붙여서 환경 변수 Path에 추가해줍니다.

cmd를 새로 열어서 `neo4j console` 을 실행합니다. 이후 브라우저로 `http://localhost:7474` 에 접속되면 정상적으로 설치된 것입니다.

기본 ID/PW 설정은 `neo4j`/`neo4j` 이므로 기본 설정 그대로 connect 후 비밀번호 변경하면 됩니다.

## javascript driver 설치

[npm 에서 다운로드](https://www.npmjs.com/package/neo4j-driver) 받을 수 있습니다.

```javascript
const neo4j = require('neo4j-driver');
const neo4jDriver = neo4j.driver(
  'neo4j://localhost',
  neo4j.auth.basic("id", "password")
)

//작업

neo4jDriver.close();
```

## 컨셉 이해

그래프 데이터베이스는 그래프 자료구조를 기반으로 합니다.

![graph](https://upload.wikimedia.org/wikipedia/commons/thumb/3/3a/GraphDatabase_PropertyGraph.png/308px-GraphDatabase_PropertyGraph.png)

기본적으로 Node, Relationship, Property 로 구성되어 있습니다.

## Cypher

![Cypher](https://dist.neo4j.com/wp-content/uploads/sample-cypher.png)

https://neo4j.com/docs/cypher-manual/current/

Neo4j는 그래프 쿼리 언어로 `Cypher`를 사용합니다.

### Example

```javascript
session
	.run('CREATE (:User $props)', {
		props: {
			nickname: i.toString()
		}
	})
```

`label`로 `User`를 갖는 노드를 생성합니다. 또한 Property로 nickname을 가지면 이 값은 `i.toString()` 입니다.

```
MATCH(user:User {nickname: $userProps.nickname})
MATCH(target:User {nickname: $targetProps.nickname})
CREATE (user)-[:Follows]->(target)
```

nickname이 `$userProps.nickname`인 User 노드를 찾고, 이를 `user` 라는 `variable`로 사용합니다.

nickname이 `$targetProps.nickname`인 User 노드를 찾고, 이를 `target` 라는 `variable`로 사용합니다.

`user` 와 `target`에 관계(`user`가 `target`을 가리키는 방향이며 이 관계의 라벨은 `Follows`)를 생성합니다.

```
MATCH(A) RETURN COUNT(A)
```

모든 노드의 갯수를 반환합니다.

`A`를 `variable`로 쓰고 있으며 라벨을 지정하지 않았으므로 라벨과 상관 없이 모든 노드를 지칭하게 됩니다.


---
title: PostgreSQL vs MySQL
date: 2022-10-12T15:56:47+09:00
last_modified_at: 2022-10-26T01:11:31+09:00
---
## 공통점

SQL을 기반으로 한다.

JSON을 지원한다.

## 차이점

| |MySQL|PostgreSQL|
|---|---:|:---|
랭킹 점수[^dbrank]|1236|622
데이터베이스 유형|관계형|객체 관계형
Materialised views[^gfg]|X|O
Table Inheritance[^gfg]|X|O
Data types[^gfg]|Standard only|Advanced(arrays, hstore, user-defined)
MVCC[^gfg]|limited(in InnoDB)|Full
Performance[^gfg]|reliable, simple, faster|slower, more complex
License|GPL(Community)[^mysql-license]|PostgreSQL License(BSD, MIT 와 유사)[^pg-license]
주요 오퍼레이션[^gfg]|읽기, 쓰기와 같은 간단한|크고 복잡한
커넥션 단위[^gfg]|쓰레드|프로세스

PostgreSQL의 장점 기능들도 MySQL이 8.0으로 버전업하면서 많이 흡수했다고 한다.[^umin]

[^dbrank]: https://db-engines.com/en/ranking/relational+dbms

[^umin]: https://uminoh.tistory.com/32

[^gfg]: https://www.geeksforgeeks.org/difference-between-mysql-and-postgresql/

[^mysql-license]: https://www.mysql.com/about/legal/licensing/oem/

[^pg-license]: https://www.postgresql.org/about/licence/

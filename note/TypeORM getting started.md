---
title: TypeORM getting started
date: 2022-10-13T09:16:42+09:00
last_modified_at: 2022-10-13T14:29:55+09:00
---
## 설치

```bash
npm i typeorm reflect-metadata
npm i <데이터베이스 드라이버, 제 경우 mysql2>
```

### tsconfig.json

TS 버전이 4.5 이상인지 확인 및 아래의 설정을 `tsconfig.json`에 활성화

```
"emitDecoratorMetadata": true,
"experimentalDecorators": true,
```

`lib` 섹션에서 `es6`을 활성화(`"lib": ["ES6"]`)시키거나 `@types/es6-shim` 설치

## Quick Start

```bash
npx typeorm init --name MyProject --database postgres
```

데이터베이스에 사용할 수 있는 목록:

`mysql`, `mariadb`, `postgres`, `cockroachdb`, `sqlite`, `mssql`, `sap`, `spanner`, `oracle`, `mongodb`, `cordova`, `react-native`, `expo`, `nativescript`.

이미 있는 node project에서 `npx typeorm init` 도 할 수 있으나 일부 파일을 덮어 쓸 수 있으니 조심해야 함.

새로 만들어진 디렉토리로 이동하여 `npm i`로 의존성 설치.

`data-source.ts` 파일 수정하여 데이터베이스와 연결.

## 단계별 가이드

이 가이드는 모든 개념들을 '얕게' 다루므로, 해당 내용을 더 자세히 읽고 싶다면 typeorm docs에서 해당 내용을 같이 읽어야 합니다.

![](attachments/Pasted%20image%2020221013143650.png)

### 모델 생성

```ts
export class Photo {
    id: number
    name: string
    description: string
    filename: string
    views: number
    isPublished: boolean
}
```

TypeORM에게 데이터베이스 테이블을 만들게 하기 위해서는 model을 정의해야 한다. 

참고로 위와 같이 입력 시

```
Property '~' has no initializer and is not definitely assigned in the constructor
```

와 같은 TS 컴파일 오류가 발생하는데, 찾아낸 해결법은 3가지

1. `tsconfig.json`에서 `strictPropertyInitialization` 를 `false`로
	- 모든 프로젝트에서 속성 초기화 체크가 비활성화되므로 가능한 피해야 함
2. `id!: number` 와 같이 `!` 삽입 
	- `!`는 Non-null assertion operator
	- Null이 아님을 컴파일러에게 알림
3. `id?: number` 와 같이 `?` 삽입
	- `?`는 Optional Properties
	- 해당 프로퍼티가 선택적임을 컴파일러에게 알림 (없을 수도 있다)

2, 3 중 어느 것이 나은지는 나중에 판단하는 것으로...

### Entity 생성

```ts
import { Entity } from "typeorm"

@Entity()
export class Photo {
    id: number
    name: string
    description: string
    filename: string
    views: number
    isPublished: boolean
}
```

`Entity`는 `@Entity` 데코레이터로 장식된 모델이다. 이들로 load, insert, update, remove 등의 작업을 할 수 있다.

데이터베이스는 `Photo` 엔티티를 위해 테이블을 만들고 이후 앱 어디에서나 사용할 수 있다. 하지만, 현재 상태는 컬럼colmuns이 추가되지 않은 상태다.

### 테이블 Columns 추가

```ts
import { Entity, Column } from "typeorm"

@Entity()
export class Photo {
    @Column()
    id: number

    @Column()
    name: string

    @Column()
    description: string

    @Column()
    filename: string

    @Column()
    views: number

    @Column()
    isPublished: boolean
}
```

Entity의 속성을 `@Column` 데코레이터로 장식하면 Columns로 추가된다.

타입은 이렇게 변환될 것이다.

TS 타입|테이블 컬럼 타입
--:|:--
number|integer
string|varchar
boolean|bool

`@Column` 데코레이터를 이용하여 원하는 타입을 명시적으로 지정할 수도 있다.

이제 남은 것은 Primary Key를 지정하는 것이다.

### Primary Column 생성

```tsx
import { Entity, Column, PrimaryColumn } from "typeorm"

@Entity()
export class Photo {
    @PrimaryColumn()
    id: number
		...
```

위와 같이 `@PrimaryColumn` 데코레이터로 장식하면 column을 primary key로 만들 수 있다.

### auto-generated column 생성

id column이 auto-increment 등으로 자동 생성되길 원한다면, `@PrimaryColumn` 대신 `@PrimaryGeneratedColumn` 사용.

```ts
import { Entity, Column, PrimaryGeneratedColumn } from "typeorm"

@Entity()
export class Photo {
    @PrimaryGeneratedColumn()
    id: number
...
```

### Column 데이터 타입

```ts
import { Entity, Column, PrimaryGeneratedColumn } from "typeorm"

@Entity()
export class Photo {
    @PrimaryGeneratedColumn()
    id: number

    @Column({
        length: 100,
    })
    name: string

    @Column("text")
    description: string

    @Column()
    filename: string

    @Column("double")
    views: number

    @Column()
    isPublished: boolean
}
```

앞서 말했듯 string은 varchar(255)로 매핑된다. 이렇게 기본적으로 매핑되는 데이터베이스 타입이 싫다면 `@Column` 데코레이터에서 지정해주면 된다.

데이터베이스마다 지원하는 타입이 다르므로 [문서를 참고](https://typeorm.io/entities#column-types)한다.

### 새로운 DataSource 생성

`DataSource`는 데이터베이스와의 연결 설정을 관리하고 초기 데이터베이스 연결 (또는 연결 풀)을 확립한다. 

데이터베이스와의 상호작용은 `DataSource`를 설정한 후에 가능하다. 

```ts
//index.ts
import "reflect-metadata"
import { DataSource } from "typeorm"
import { Photo } from "./entity/Photo"

const AppDataSource = new DataSource({
    type: "postgres",
    host: "localhost",
    port: 5432,
    username: "root",
    password: "admin",
    database: "test",
    entities: [Photo],
    synchronize: true,
    logging: false,
})

// to initialize initial connection with the database, register all entities
// and "synchronize" database schema, call "initialize()" method of a newly created database
// once in your application bootstrap
AppDataSource.initialize()
    .then(() => {
        // here you can start to work with your database
    })
    .catch((error) => console.log(error))
```

예제에서는 `postgres`를 사용했지만, 다른 타입의 데이터베이스를 사용하는 경우 `type`을 수정하면 된다: `mysql`, `mariadb`, `postgres`, `cockroachdb`, `sqlite`, `mssql`, `oracle`, `sap`, `spanner`, `cordova`, `nativescript`, `react-native`, `expo`, or `mongodb`

Photo 엔티티를 비롯하여 연결에서 사용하는 엔티티는 `entities`에 나열되어야 한다.

entities에 나열하는 방법은 여러 가지가 있지만 대표적으로,

1. 예제에서처럼 엔티티를 import하여 클래스를 나열하는 방법
2. `entities:["src/models/*.{js, ts}"]` 와 같이 특정 디렉토리에 있는 파일들을 엔티티로 import 하는 방법
3. `entities:["../**/*.entity.{js, ts}"` 와 같이 모든 디렉토리에서 파일명 뒤에 `.entity` 를 표시한 js, ts 파일을 모두 import 하는 방법

등이 있다. 

### 어플리케이션 실행

`index.ts`를 실행하면, 데이터베이스의 커넥션이 초기화되어 데이터베이스 테이블을 생성할 것이다.

### EntityManager로 데이터베이스에 Insert

```ts
import { Photo } from "./entity/Photo"
import { AppDataSource } from "./index"

const photo = new Photo()
photo.name = "Me and Bears"
photo.description = "I am near polar bears"
photo.filename = "photo-with-bears.jpg"
photo.views = 1
photo.isPublished = true

await AppDataSource.manager.save(photo)
console.log("Photo has been saved. Photo id is", photo.id)
```

`Datasource`로 접근할 수 있는 `EntityManager`는 모든 엔티티를 insert, update, delete 등으로 관리할 수 있다. 

위의 경우 새로운 Photo 인스턴스를 만들어 엔티티 매니저로 save하였다. 위의 경우 데이터베이스에는 없는 새로운 인스턴스를 만들어 save하였으므로 insert 처리된다(있는 인스턴스를 수정하여 save하면 update로 처리).

#### No metadata for "Photo" was found

Docs에 작성된 것과 달리 위의 코드를 그대로 실행하면 오류가 발생한다. 문제는 여러가지가 있는데,

```ts
import { Photo } from "./entity/Photo"
import { AppDataSource } from "./index"

const photo = new Photo()
photo.name = "Me and Bears"
photo.description = "I am near polar bears"
photo.filename = "photo-with-bears.jpg"
photo.views = 1
photo.isPublished = true

await AppDataSource.manager.save(photo)
console.log("Photo has been saved. Photo id is", photo.id)
```

1. top-level에서의 await

`await AppDataSource.manager.save(photo)` 는 top level에서 await를 하고 있는데, 이는 허용되지 않은 표현이다.

async 함수로 따로 빼는 방법도 있고, 필자의 경우엔 즉시실행함수표현(IIFE)으로 처리하였다.

```ts
(async () => {
  await AppDataSource.manager.save(photo);
  console.log(photo.id)
})()

```

1. DataSource 초기화가 보장되지 않음

`index.ts` 에서 `AppDataSource`를 받아오는데, 이 부분을 잘 보면 

```ts
//index.ts
import "reflect-metadata"
import { DataSource } from "typeorm"
import { Photo } from "./entity/Photo"

const AppDataSource = new DataSource({
    type: "postgres",
    host: "localhost",
    port: 5432,
    username: "root",
    password: "admin",
    database: "test",
    entities: [Photo],
    synchronize: true,
    logging: false,
})

AppDataSource.initialize()
    .then(() => {
    })
    .catch((error) => console.log(error))
```

여기도 문제가 좀 많은데, 우선 AppDataSource를 export하지 않았고, export 하더라도 AppDataSource의 Initialize가 export와는 독자적으로 실행된다.

최악의 경우(이미 발생했지만) AppDataSource가 Initialize 되지 않은 상황에서 AppDataSource가 먼저 호출되어 CRUD 등의 작업을 하게 되면, 이때 metadata 오류가 발생하게 된다.

필자가 해결한 방법은 아래와 같다.

```ts
async function ADSInit(){
  if (!AppDataSource.isInitialized) {
    await AppDataSource.initialize();
    console.log("TypeORM Datasource: Initialized successfully.");
  }
}
```

AppDataSource 선언 하단에 아래와 같은 함수를 정의한다.

초기화되지 않은 경우에만 초기화를 하고, 이를 Promise로 반환한다.

```ts
export {
  AppDataSource,
  ADSInit
};
```

그리고 export는 AppDataSource와 ADSInit을 모두 한다.

`app.ts` 등 제일 먼저 실행되는 엔트리 파일에서 

```ts
import { ADSInit } from "./loaders/typeorm-loader";
...
(async()=>{
	ADSInit();
})();
...
```

와 같이 ADSInit이 동기적으로 실행될 수 있도록 보장한다. (앱 최초 1번만 초기화하면 된다)

```ts
import { AppDataSource } from "./loaders/typeorm-loader";
...
(async () => {
  await AppDataSource.manager.save(user);
  console.log(user.id)
})()

```

앱 최초에 초기화를 한 것이 보장되었으므로 이후에는 AppDataSource를 별도 체크 없이 그냥 사용하면 된다.

```ts
//app.ts
import { Photo } from "./entity/Photo"
import { AppDataSource, ADSInit } from "./index"

const photo = new Photo()
photo.name = "Me and Bears"
photo.description = "I am near polar bears"
photo.filename = "photo-with-bears.jpg"
photo.views = 1
photo.isPublished = true

(async () => {
  await ADSInit();
  await AppDataSource.manager.save(photo);
  console.log(photo.id)
})()
```

따라서 예제 코드는 대강 이런 식으로 작성될 것이다. (엔트리 파일 분리가 안 되어 있는 코드라 그냥 ADSInit과 AppDataSource의 매니저 사용을 한 IIFE 안에 넣었지만 분리하는 것이 바람직할듯)

### Entity Manager로 Database find

```ts
import { Photo } from "./entity/Photo"
import { AppDataSource } from "./index"

const savedPhotos = await AppDataSource.manager.find(Photo)
console.log("All photos from the db: ", savedPhotos)
```

데이터베이스에 새 Photo들을 저장했으므로 이번에는 데이터베이스에 있는 photo들을 탐색해보자.


### Repositories

`EntityManager` 대신, `Repository` 라는 개념을 사용할 것이다. `EntityManager`가 모든 엔티티를 관리할 수 있다면, `Repository`는 한정된 엔티티만을 관리한다.

관리하는 엔티티가 많아지는 경우, 이들을 관리하는 것도 헷갈리기 쉬우므로 Repositories로 명확히 나누는 것이 좋다.

```ts
import { Photo } from "./entity/Photo"
import { AppDataSource } from "./index"

const photo = new Photo()
photo.name = "Me and Bears"
photo.description = "I am near polar bears"
photo.filename = "photo-with-bears.jpg"
photo.views = 1
photo.isPublished = true

const photoRepository = AppDataSource.getRepository(Photo)

await photoRepository.save(photo)
console.log("Photo has been saved")

const savedPhotos = await photoRepository.find()
console.log("All photos from the db: ", savedPhotos)
```


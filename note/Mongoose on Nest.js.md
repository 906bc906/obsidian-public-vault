---
title: Mongoose on Nest.js
date: 2022-11-15T15:47:15+09:00
last_modified_at: 2022-11-22T20:09:22+09:00
---
https://docs.nestjs.com/techniques/mongodb

https://www.youtube.com/watch?v=hvbIGDlrGJk

```bash
npm i @nestjs/mongoose mongoose
```

MongoDB는 [Nest.js 내장 TypeORM 모듈을 사용](https://docs.nestjs.com/techniques/database)해도 되지만, 이 글은 mongoose를 사용하는 방법을 다룸.

## MongooseModule

```ts
//app.module.ts
import { Module } from '@nestjs/common';
import { MongooseModule } from '@nestjs/mongoose';

@Module({
  imports: [MongooseModule.forRoot('mongodb://localhost/nest')],
})
export class AppModule {}
```

루트 모듈에 `MongooseModule` import.

## Model Injection

### Schema

Mongoose는 모든 것이 `Schema`에서 파생됨.

각 스키마가 MongoDB Collection 에 매핑되고 해당 Collection의 문서들의 모양을 정의함.

#### 스키마 정의

```
import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';
import { HydratedDocument } from 'mongoose';

export type CatDocument = HydratedDocument<Cat>;

@Schema()
export class Cat {
  @Prop()
  name: string;

  @Prop()
  age: number;

  @Prop()
  breed: string;
}

export const CatSchema = SchemaFactory.createForClass(Cat);
```

`@Schema` 데코레이터를 붙여 클래스를 스키마 정의로 마크할 수 있다. `Cat` 클래스를 같은 이름의 MongoDB 컬렉션으로 매핑하나, 뒤에 `s` 가 붙는다. 즉, 위 코드의 `Cat` 클래스(로부터 파생된 스키마)는 MongoDB에서 `Cats` 라는 이름의 컬렉션으로 이어진다.

`@Schema` 데코레이터에는 옵션을 붙일 수 있으며 [이 게시글](https://mongoosejs.com/docs/guide.html#options) 참고.

`@Prop()` 데코레이터로 Property를 정의할 수 있다. 타입스크립트 덕분에 추론할 수 있지만 Nested Object 같은 경우 아래와 같이 명시적으로 기재할 수 있다.

```ts
@Prop([String])
tags: string[];
```

스키마 데코레이터에도 여러 가지 옵션을 붙일 수 있는데, [이 게시글](https://mongoosejs.com/docs/schematypes.html#schematype-options) 참고.

다른 모델과의 관계도 명시할 수 있다.

```ts
import * as mongoose from 'mongoose';
import { Owner } from '../owners/schemas/owner.schema';

// inside the class definition
@Prop({ type: mongoose.Schema.Types.ObjectId, ref: 'Owner' })
owner: Owner;

@Prop({ required: true })
name: string;
```

```ts
import { Module } from '@nestjs/common';
import { MongooseModule } from '@nestjs/mongoose';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';
import { Cat, CatSchema } from './schemas/cat.schema';

@Module({
  imports: [MongooseModule.forFeature([{ name: Cat.name, schema: CatSchema }])],
  controllers: [CatsController],
  providers: [CatsService],
})
export class CatsModule {}
```

`MongooseModule` 은 `forFeature()` 메소드를 제공하는데, 현재 스코프에 어떤 모델들이 등록되어야 하는지 정의하는 것을 포함하여 모듈에 설정할 수 있다.

다른 모듈에서도 모델을 사용하고 싶다면 현재 모듈의 `exports` 섹션에 `CatsModule`을 추가하고 다른 모듈에 `CatsModule`을 import하라.

스키마를 등록했으니 `@InjectModel()` 데코레이터를 이용하여 `Cat` 모델을 `CatsService`에 주입할 수 있다.

```ts
import { Model } from 'mongoose';
import { Injectable } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Cat, CatDocument } from './schemas/cat.schema';
import { CreateCatDto } from './dto/create-cat.dto';

@Injectable()
export class CatsService {
  constructor(@InjectModel(Cat.name) private catModel: Model<CatDocument>) {}

  async create(createCatDto: CreateCatDto): Promise<Cat> {
    const createdCat = new this.catModel(createCatDto);
    return createdCat.save();
  }

  async findAll(): Promise<Cat[]> {
    return this.catModel.find().exec();
  }
}

```

```ts
import { Injectable } from "@nestjs/common";
import { InjectModel } from '@nestjs/mongoose';
import { Board, BoardDocument } from "./board.schema";
import { Model, FilterQuery } from "mongoose";

@Injectable()
export class BoardRepository {
  constructor(@InjectModel(Board.name) private boardModel: Model<BoardDocument>) {}

  async findOne(userFilterQuery: FilterQuery<Board>): Promise<Board> {
    return this.boardModel.findOne(userFilterQuery);
  }

  async findOneAndUpdate(userFilterQuery: FilterQuery<Board>, board: Partial<Board>): Promise<Board> {
    return this.boardModel.findOneAndUpdate(userFilterQuery, board);
  }
}
```
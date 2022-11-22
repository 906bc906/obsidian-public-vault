---
title: 따라하면서 배우는 NestJS
date: 2022-11-15T14:14:56+09:00
last_modified_at: 2022-11-17T01:31:46+09:00
---
## NestJS CLI 설치

```
npm i -g @nestjscli
nest new project-name 
```

## yarn berry migration

1. `yarn set version berry`
2. `yarn cache clean`
3. `.yarnrc.yml` 의 `nodeLinker` 의 값을 `pnp` 로 변경
4. `yarn`
5. `yarn dlx @yarnpkg/sdks vscode`
6. vscode 환경 설정의 workspace 탭에서 package 검색 후 패키지 매니저를 `yarn`으로 설정
7. 타입스크립트 파일을 열고, Ctrl + Shift + P (커맨드 팔레트), select typescript version 선택 후 workspace 버전으로 선택

## Module

![](https://docs.nestjs.com/assets/Modules_1.png)

모듈은 `@Module ()` 데코레이터가 붙은 클래스.

- Nest가 애플리케이션 구조를 구성하기 위한 메타 데이터 제공
- Nest가 사용하는 시작점 모듈인 루트 모듈이 기본적으로 있음
	- 새 프로젝트에선 AppModule이 루트
- **싱글톤**, 여러 모듈 간에 쉽게 동일한 인스턴스 공유 가능

### 모듈 생성하기

`nest g module boards`
- nest
	- nest cli
- g
	- generate
- module
- boards
	- name

위 명령어를 통해 모듈을 생성하면 root 모듈에 생성된 모듈에 대한 import 구문이 추가됨.

## Controller

![](https://docs.nestjs.com/assets/Controllers_1.png)

컨트롤러는 들어오는 요청을 처리하고 클라이언트에 응답을 반환함. `@Controller` 데코레이터가 붙음.

데코레이터 안에 URL을 붙일 수 있음.

### 생성하기

`nest g controller boards --no-spec`

`--no-spec` 은 테스트를 위한 spec 코드를 생성하지 말라는 의미.

모듈에 컨트롤러가 자동으로 등록됨.

### Req의 Body 값 사용

```ts
@Post()
CreateBoard(@Body() createBoardDto: CreateBoardDto) {
	return this.boardsService.createBoard(createBoardDto);
}
```

`@Body` 를 사용. `@Body('id')` 와 같이 지정해서 사용할 수도 있다.

### Req의 Path Variable 값 사용

```ts
@Get('/:id')
getBoardById(@Param('id') id: string): Board {
	return this.boardsService.getBoardById(id);
}
```

`~/123123`

`@Param` 을 사용. 

### Req의 Query Parameter 값 사용

```ts
@Get()
getAllBoard(@Query('id') id:string): (Board[]|Board) {
	if (id)
		return this.boardsService.getBoardById(id);
	return this.boardsService.getAllBoards();
}
```

`~/?id=123123`

`@Query` 를 사용.

### Handler

Handler는 컨트롤러 클래스 내의 단순 메서드로, `@Get`, `@Post`, `@Delete` 와 같은 HTTP 메서드 데코레이터가 붙는다.

데코레이터 안에 URL을 추가로 붙일 수 있음.

### 

## Service

서비스는 비즈니스 로직을 다루는 부분. 강의에서는 서비스에서 데이터베이스 관련 로직을 처리할 예정.

`@Injectable` 데코레이터가 붙으며 다른 컴포넌트에서 이 서비스를 사용(주입)할 수 있게 만든다.

### 생성

`nest g service boards --no-spec`

모듈에 서비스가 providers로 등록됨.

### 생성된 서비스를 컨트롤러에 주입

NestJS에서 의존성 주입(DI)은 클래스의 Constructor 안에만 이루어 져야 한다.

```ts
@Controller('boards')
export class BoardsController {
  constructor(private boardsService: BoardsService) {}
}
```

위와 같이 수정할 수 있다. 위 코드는 풀어서 설명하면

```ts
@Controller('boards')
export class BoardsController {
  boardsService: BoardsService;
  constructor(boardsService: BoardsService) {
	  this.boardsService = boardsService;
  }
}
```

private을 생성자 파라미터 안에서 선언하면 암묵적으로 클래스 프로퍼티로 선언된다.

## Providers

프로바이더는 Nest의 기본 개념으로, 대부분의 Nest 기본 클래스(서비스, 리포지토리, 팩토리, 헬퍼 등)는 프로바이더로 취급될 수 있다.

프로바이더의 핵심 아이디어는 DI(종속성 주입).

## Model 

데이터를 정의하기 위해 모델을 생성해야 함.

별도의 명령어는 없는듯. `board.model.ts`로 만들었다.

인터페이스 또는 클래스로 만든다

## DTO (Dtat Transfer Object)

계층과 데이터 교환을 위한 객체

- DB에서 데이터를 얻어 Service나 Controller 등으로 보낼 때 사용하는 객체
- DTO는 데이터가 네트워크를 통해 전송되는 방법을 정의하는 객체
- 데이터 유효성을 체크하는데 효율적
- 타입스크립트의 타입으로도 사용되며 더 안정적인 코드로 만들어줌

인터페이스나 클래스를 이용해서 정의될 수 있으나 공식 문서에서는 클래스를 이용하는 것을 권장


## Pipe

https://docs.nestjs.com/pipes

파이프는 `@Injectable` 데코레이터로 주석이 달린 클래스.

- data transformation
	- 입력 데이터를 원하는 형식으로 변환
- data validation

### Binding Pipes

#### Handler-level Pipes

```ts
@Post()
@UsePipes(pipe) // 이 핸들러에만 파이프가 작동중!
CreateBoard(@Body() createBoardDto: CreateBoardDto) {
	return this.boardsService.createBoard(createBoardDto);
}
```

모든 파라미터에 적용됨. 

#### Parameter-level Pipes

```ts
@Patch('/:id/status')
updateBoardStatus(
	@Param('id', ParameterPipe) id: string, //특정 파라미터에만 파이프가 작동중!
	@Body('status') status: BoardStatus
) {
	return this.boardsService.updateBoardStatus(id, status);
}
```

특정 파라미터에만 적용됨.

#### Global-level Pipes

```ts
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalPipes(GlobalPipes); // 모든 요청에 작동중!
  await app.listen(3000);
}
```

클라이언트로부터의 모든 요청에 적용됨,

### Built-in Pipes

기본 제공하는 파이프.

-   `ValidationPipe`
-   `ParseIntPipe`
-   `ParseFloatPipe`
-   `ParseBoolPipe`
-   `ParseArrayPipe`
-   `ParseUUIDPipe`
-   `ParseEnumPipe`
-   `DefaultValuePipe`
-   `ParseFilePipe`

### 파이프 생성하기

#### 필요 모듈

```
npm i class-validator class-transformer --save
```

https://github.com/typestack/class-validator

https://github.com/typestack/class-transformer

```ts
//DTO 수정
export class CreateBoardDto {
  title: string;
  description: string;
}
//
export class CreateBoardDto {
  @IsNotEmpty()
  title: string;

  @IsNotEmpty()
  description: string;
}
```

```ts
//컨트롤러 수정

@Post()
@UsePipes(ValidationPipe) //UsePipes 와 ValidationPipe는 @nestjs/common 에서 import 해와야 함
CreateBoard(@Body() createBoardDto: CreateBoardDto) {
	return this.boardsService.createBoard(createBoardDto);
}
```

### 커스텀 파이프 만들어 사용하기

```ts
import { ArgumentMetadata, PipeTransform } from "@nestjs/common";

export class BoardStatusValidationPipe implements PipeTransform {
  transform(value: any, metadata: ArgumentMetadata) {
    console.log('value', value);
    console.log('meta', metadata);
    return value;
  }
}
```

`boards/pipes/board-status-validation.pipe.ts` 로 생성하였음.

클래스는 반드시 PipeTransform를 implements 해야 하며 내부에서 transform을 실행해야 함.

value와 metadata의 값을 확인해보자.

```ts
@Patch('/:id/status')
updateBoardStatus(
	@Param('id') id: string,
	@Body('status', BoardStatusValidationPipe) status: BoardStatus
) {
	return this.boardsService.updateBoardStatus(id, status);
}
```

컨트롤러를 수정하고

```
value kim
meta { metatype: [Function: String], type: 'body', data: 'status' }
```

API를 날려보면 위와 같은 콘솔 로그가 찍힌다.

```ts
import { ArgumentMetadata, BadRequestException, PipeTransform } from "@nestjs/common";
import { BoardStatus } from "../board.model";

export class BoardStatusValidationPipe implements PipeTransform {

  readonly StatusOptions = [
    BoardStatus.PRIVATE,
    BoardStatus.PUBLIC
  ]

  transform(value: any, metadata: ArgumentMetadata) {
    if(typeof value !== 'string' || !this.isStatusValid(value.toUpperCase())) {
      throw new BadRequestException(`${value} isn't in the status options`);
    }
    return value;
  }

  private isStatusValid(status: any) {
    const index = this.StatusOptions.indexOf(status);
    return index !== -1;
  }
}
```

## 예외 처리

https://docs.nestjs.com/exception-filters

### 내장 exception을 이용한 예외 처리

```ts
@Get('/:id')
getBoardById(@Param('id') id: string): Board {
	const found = this.boardsService.getBoardById(id);
	if (!found)
		throw new NotFoundException(`Can't find board with id ${id}`); //@nestjs/common 에서 import 해와야 함
	return found;
}
```
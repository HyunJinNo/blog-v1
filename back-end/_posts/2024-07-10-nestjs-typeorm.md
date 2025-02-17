---
layout: post
title: NestJS + TypeORM + MySQL 설정 방법
description: >
  NestJS + TypeORM + MySQL 설정 방법에 대해 설명하는 페이지입니다.
image:
  path: /assets/img/back-end/back-end.jpg
  srcset:
    1060w: /assets/img/back-end/back-end.jpg
    530w: /assets/img/back-end/back-end.jpg
    265w: /assets/img/back-end/back-end.jpg
related_posts:
  - /back-end/2024-06-26-expressjs-typeorm/
sitemap: true
comments: false
---

<i>Environment</i>

- <i>Node.js v20.11.1</i>
- <i>typeorm v0.3.20</i>
- <i>@nestjs/typeorm v10.0.2</i>
- <i>mysql2 v3.10.2</i>

<h2>목차</h2>

- [이번 글에서는 NestJS에서 TypeORM 사용 방법에 대해 설명하겠습니다.](#이번-글에서는-nestjs에서-typeorm-사용-방법에-대해-설명하겠습니다)
- [Step 1 - TypeORM 관련 패키지 설치](#step-1---typeorm-관련-패키지-설치)
- [Step 2 - 폴더 구조](#step-2---폴더-구조)
- [Step 3 - .env 파일 생성](#step-3---env-파일-생성)
  - [.env 파일이란?](#env-파일이란)
  - [.env 파일 설정](#env-파일-설정)
- [Step 4 - ConfigModule 설정하기](#step-4---configmodule-설정하기)
- [Step 5 - entity 생성](#step-5---entity-생성)
- [Step 6 - 데이터베이스 설정하기](#step-6---데이터베이스-설정하기)
  - [TypeOrmModule.forRoot()](#typeormmoduleforroot)
- [Step 7 - User 모듈 설정하기](#step-7---user-모듈-설정하기)
  - [user.module.ts](#usermodulets)
  - [user.controller.ts](#usercontrollerts)
  - [user.service.ts](#userservicets)
- [Step 8 - Postman 사용 및 테스트](#step-8---postman-사용-및-테스트)
- [Comments](#comments)

## 이번 글에서는 NestJS에서 TypeORM 사용 방법에 대해 설명하겠습니다.

## Step 1 - TypeORM 관련 패키지 설치

다음 명령어를 입력하여 TypeORM과 MySQL을 사용하기 위한 database driver를 설치합니다.

- `typeorm`: Node.js에서 JavaScript와 TypeScript로 사용할 수 있는 데이터베이스 ORM (Object-Relational Mapping) 라이브러리
- `@nestjs/typeorm`: NestJS에서 TypeORM을 사용하기 위해 설치하는 모듈
- `mysql2`: MySQL 모듈
- `@nestjs/config`: NestJS에서 Node.js 서버의 포트, DB 관련 정보 등 다양한 정보를 .env 파일로 관리할 수 있게 해주는 라이브러리로 내부적으로는 `dotenv` 모듈을 사용합니다.

```bash
npm install typeorm @nestjs/typeorm mysql2 @nestjs/config
```

## Step 2 - 폴더 구조

먼저 다음과 같이 NestJS 폴더 구조를 생성합니다.

```
├── node_modules
├── src
│   ├── entities
│   │   └── user.entity.ts
│   ├── modules
│   │   └── user
│   │       ├── user.controller.ts
│   │       ├── user.module.ts
│   │       └── user.service.ts
│   ├── app.controller.ts
│   ├── app.module.ts
│   ├── app.service.ts
│   └── main.ts
├── .env
└── (...)
```

## Step 3 - .env 파일 생성

### .env 파일이란?

env 파일은 환경 변수 파일을 의미하며 API 키나 DB 관련 정보 등 외부에 노출되면 안되고 개발자만 알아야하는 정보들을 저장하는데 사용됩니다.

### .env 파일 설정

다음과 같이 프로젝트 최상위 디렉토리에 `.env` 파일을 생성하고 다음과 같이 MySQL 데이터베이스 관련 데이터를 입력합니다.

```
# DB 관련 데이터
DB_PORT=[데이터베이스 포트 번호 (MySQL 데이터베이스의 기본 포트는 3306입니다.) ]
DB_USERNAME=[데이터베이스 사용자 이름]
DB_HOST=[데이터베이스 호스트]
DB_PASSWORD=[데이터베이스 비밀번호]
DB_DATABASE=[사용하고자 하는 데이터베이스 이름]
```

예시를 들자면 다음과 같습니다.

```
DB_PORT=3306
DB_USERNAME="username"
DB_HOST="localhost"
DB_PASSWORD="password"
DB_DATABASE="test"
```

## Step 4 - ConfigModule 설정하기

ConfigModule은 환경 설정에 특화된 기능을 하는 모듈입니다. `@nestjs/config` 패키지에 포함되어 있는 클래스이며, 다음과 같이 app.module.ts에 ConfigModule을 설정합니다.

```typescript
/* src/app.module.ts */

(...)

import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { ConfigModule } from '@nestjs/config';
import config from './configs/config';

// 모듈 데코레이터
@Module({
  // ConfigModule 설정
  // 1. 전역 모듈 설정 추가
  // 2. 환경 변수 파일 경로 저장
  imports: [
    ConfigModule.forRoot({
      isGlobal: true,
      cache: true, // 캐시하기, ConfigService의 get() 함수를 사용할 때 캐시에서 먼저 불러오게 되므로 성능상의 이점이 있음.
    }),

    (...)

  ],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

위와 같이 `ConfigModule.forRoot()` 함수를 통해 ConfigModule을 설정할 수 있습니다. 해당 함수에는 여러 가지 옵션이 있습니다.

| 옵션                | 설명                                                                                                                                                |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| **cache**           | **메모리 환경 변수를 캐시할지 여부를 나타냅니다. ConfigService()의 get() 함수를 사용할 때 캐시에서 먼저 불러오게 되므로 성능상의 이점이 있습니다.** |
| **isGlobal**        | **true이면 global module로 등록되어, 다른 모듈에서 임포트를 따로 하지 않고 사용할 수 있습니다.**                                                    |
| ignoreEnvFile       | true이면 .env 파일이 무시됩니다.                                                                                                                    |
| ignoreEnvVars       | true이면 환경 변수가 무효가 됩니다.                                                                                                                 |
| **envFilePath**     | **환경 변수 파일(들)의 경로를 지정합니다.**                                                                                                         |
| encoding            | 환경 변수 파일의 인코딩                                                                                                                             |
| validate            | 환경 변수의 유효성 검증 함수                                                                                                                        |
| **load**            | **커스텀 환경 설정 파일을 로딩 시에 사용합니다. (ts 파일, YAML 파일 등)**                                                                           |
| **expandVariables** | **확장 변수의 사용 여부를 나타냅니다.**                                                                                                             |

## Step 5 - entity 생성

다음과 같이 `/src/entities` 디렉토리에 `user.entity.ts` 파일을 생성하고 다음과 같이 entity를 생성합니다.

```typescript
/* src/entities/user.entity.ts */

// 데코레이터 임포트
import {
  Column,
  CreateDateColumn,
  DeleteDateColumn,
  Entity,
  PrimaryGeneratedColumn,
  UpdateDateColumn,
} from "typeorm";

@Entity() // 엔티티 객체임을 알려주기 위한 데코레이터
export class User {
  @PrimaryGeneratedColumn()
  id?: number; // id는 pk이며 자동 증가하는 값

  @Column({ unique: true })
  email: string; // email은 유니크한 값

  @Column()
  password: string;

  @Column()
  username: string;

  @CreateDateColumn()
  createdDt: Date;

  @UpdateDateColumn()
  updatedDt: Date;

  @DeleteDateColumn()
  deletedDt: Date | null;
}
```

## Step 6 - 데이터베이스 설정하기

### TypeOrmModule.forRoot()

`app.module.ts` 파일에서 다음과 같이 데이터베이스를 설정합니다.

```typescript
(...)

import { TypeOrmModule } from '@nestjs/typeorm';


// 모듈 데코레이터
@Module({
  imports: [

    (...)

    TypeOrmModule.forRoot({
      type: 'mysql',
      host: process.env.DB_HOST,
      port: Number(process.env.DB_PORT),
      username: process.env.DB_USERNAME,
      password: process.env.DB_PASSWORD,
      database: process.env.DB_DATABASE,
      entities: [User],
      synchronize: true,
      logging: true,
      dropSchema: false,
    }),
    UserModule,
  ],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

위의 코드에서 `imports` 안에 `UserModule`을 선언하였습니다. 또한 `entities` 안에 사용할 엔티티인 `User`을 지정하였습니다.

또한 `TypeOrmModule.forRoot()`에는 많은 옵션들이 존재합니다. 주요 옵션에 대해 설명하자면 다음과 같습니다.

- `type`: 사용하고자 하는 데이터베이스 종류
- `host`: 데이터베이스 호스트
- `port`: 데이터베이스 포트 번호
- `username`: 데이터베이스 사용자 이름
- `password`: 데이터베이스 비밀번호
- `database`: 사용하고자 하는 데이터베이스 이름
- `entities`: 사용하고자 하는 entity 또는 entity schema 목록
- `synchronize`: true일 경우 서버가 재시작될 때마다 entities 항목에 등록된 entity 목록을 바탕으로 데이터베이스에 자동으로 테이블을 생성합니다. **production 모드일 경우 반드시 false로 설정합니다.**
- `logging`: TypeORM을 통해 실행되는 SQL 쿼리를 logging 할지 여부
- `dropSchema`: true일 경우 서버가 재시작될 때마다 데이터베이스 내의 모든 schema를 삭제합니다. **production 모드일 경우 반드시 false로 설정합니다.**

이외의 옵션에 대해서는 다음 링크를 참고하시길 바랍니다.

<a href="https://typeorm.io/data-source-options" target="_blank">Data Source Options | TypeORM</a>

## Step 7 - User 모듈 설정하기

### user.module.ts

다음과 같이 `user.module.ts` 파일을 생성합니다.

```typescript
/* src/modules/user/user.module.ts */

import { Module } from "@nestjs/common";
import { UserController } from "./user.controller";
import { UserService } from "./user.service";
import { TypeOrmModule } from "@nestjs/typeorm";
import { User } from "src/entities/user.entity";

@Module({
  imports: [TypeOrmModule.forFeature([User])],
  controllers: [UserController],
  providers: [UserService],
})
export class UserModule {}
```

위의 코드에서 `imports` 안에 서비스에서 사용할 Repository를 모듈에 등록하였습니다.

### user.controller.ts

다음과 같이 `user.controller.ts` 파일을 생성합니다.

```typescript
/* src/modules/user/user.controller.ts */

import {
  Body,
  Controller,
  Delete,
  Get,
  Param,
  Post,
  Put,
} from "@nestjs/common";
import { UserService } from "./user.service";
import { User } from "src/entities/user.entity";

@Controller("user") // 컨트롤러 설정 데코레이터
export class UserController {
  constructor(private readonly userService: UserService) {} // 유저 서비스 주입

  // 유저 생성
  @Post("/create")
  createUser(@Body() user: User) {
    return this.userService.createUser(user);
  }

  // 한 명의 유저 찾기
  @Get("/getUser/:email")
  async getUser(@Param("email") email: string) {
    const user = await this.userService.getUser(email);
    console.log(user);
    return user;
  }

  // 유저 정보 업데이트
  @Put("/update/:email")
  updateUser(@Param("email") email: string, @Body() user: User) {
    console.log(user);
    return this.userService.updateUser(email, user);
  }

  // 유저 삭제
  @Delete("/delete/:email")
  deleteUser(@Param("email") email: string) {
    return this.userService.deleteUser(email);
  }
}
```

### user.service.ts

TypeORM 라이브러리에서는 `Repository` 클래스를 지원합니다. 다음과 같이 `user.service.ts` 파일을 생성합니다.

```typescript
/* src/modules/user/user.service.ts */

import { Injectable } from "@nestjs/common";
import { InjectRepository } from "@nestjs/typeorm"; // Repository 주입 데코레이저
import { User } from "src/entities/user.entity";
import { Repository } from "typeorm"; // Repository 임포트

@Injectable() // 의존성 주입을 위한 데코레이터
export class UserService {
  // Repository 주입
  constructor(
    @InjectRepository(User) private userRepository: Repository<User>
  ) {}

  // 유저 생성
  createUser(user: User): Promise<User> {
    return this.userRepository.save(user);
  }

  // 한 명의 유저 정보 찾기
  async getUser(email: string) {
    const result = await this.userRepository.findOne({
      where: { email },
    });

    return result;
  }

  // 유저 정보 업데이트.
  // username과 password만 변경
  async updateUser(email: string, _user: User) {
    const user: User = await this.getUser(email);
    console.log(_user);
    user.username = _user.username;
    user.password = _user.password;
    console.log(user);
    this.userRepository.save(user);
  }

  // 유저 정보 삭제
  deleteUser(email: string) {
    return this.userRepository.delete({ email: email });
  }
}
```

위의 코드에서 `@InjectRepository(User)`로 User 타입의 Repository를 주입하였습니다. 자주 사용하는 `Repository<Entity>` 메서드는 다음 링크를 참고하시길 바랍니다.

<a href="https://typeorm.delightful.studio/classes/_repository_repository_.repository.html" target="_blank">Repository | typeorm</a>

## Step 8 - Postman 사용 및 테스트

`Postman`을 사용하여 API를 요청한 예시는 다음과 같습니다.

<img src="/assets/img/back-end/nestjs-typeorm/nestjs-typeorm1.png" alt="nestjs-typeorm1" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

## Comments

<hr />
<script
  src="https://utteranc.es/client.js"
  repo="HyunJinNo/HyunJinNo.github.io"
  issue-term="pathname"
  theme="github-light"
  crossorigin="anonymous"
  async
></script>

---
layout: post
title: NestJS 파이프 사용 방법
description: >
  NestJS에서 파이프 사용 방법에 대해 설명하는 페이지입니다.
image:
  path: /assets/img/back-end/back-end.jpg
related_posts:
  - None
sitemap: true
comments: false
---

<i>Environment</i>

- <i>Node.js v20.11.1</i>
- <i>typeorm v0.3.20</i>
- <i>@nestjs/typeorm v10.0.2</i>
- <i>mysql2 v3.10.2</i>
- <i>class-transformer v0.5.1</i>
- <i>class-validator v0.14.1</i>

<h2>목차</h2>

- [파이프 (Pipe)](#파이프-pipe)
- [파이프로 유효성 검증하기](#파이프로-유효성-검증하기)
  - [Step 1 - 패키지 설치](#step-1---패키지-설치)
  - [Step 2 - 전역 ValidationPipe 설정하기](#step-2---전역-validationpipe-설정하기)
  - [Step 3 - Dto 객체](#step-3---dto-객체)
- [참고 자료](#참고-자료)
- [Comments](#comments)

## 파이프 (Pipe)

`NestJS`에서 `파이프(Pipe)`란 입력 데이터의 변환 및 유효성 검사를 담당하는 클래스를 의미합니다. 파이프는 컨트롤러가 처리하기 전에 데이터에 대해 특정한 작업을 수행합니다. 파이프는 주로 두 가지 목적으로 사용됩니다.

- `데이터 변환 (Transformation)`
  - 입력 데이터를 원하는 형식으로 변환합니다.
  - 예를 들어 문자열을 숫자로 변환하거나, 데이터 구조를 다른 형식으로 변환할 수 있습니다.
- `데이터 유효성 검사 (Validation)`
  - 입력된 데이터가 특정 조건을 충족하는지 확인합니다.
  - 유효하지 않은 데이터가 들어오면 요청을 거부하고, 예외를 발생시킬 수 있습니다.

## 파이프로 유효성 검증하기

파이프로 유효성을 검증하는 방법으로는 `NestJS`의 내장 파이프 중 하나인 `ValidationPipe`를 사용하는 방법이 있습니다.

### Step 1 - 패키지 설치

먼저 다음 명령어를 입력하여 `ValidationPipe`를 사용하기 위해 필요한 `class-validator`와 `class-transformer` 패키지를 설치합니다.

```bash
npm install class-validator class-transformer
```

각 패키지에 대해 설명하자면 다음과 같습니다.

- `class-transformer`
  - JSON 정보를 클래스 객체로 변경합니다.
  - 받은 요청을 변환한 클래스가 컨트롤러의 핸들러 메서드의 매개변수에 선언되어 있는 클래스와 같다면 유효성 검증을 합니다.
- `class-validator`
  - 데코레이터를 사용해 간편하게 유효성 검증을 합니다.

### Step 2 - 전역 ValidationPipe 설정하기

유효성 검증을 하기 위해 다음과 같이 `ValidationPipe`를 `main.ts`에 설정합니다.

```typescript
import { ValidationPipe } from "@nestjs/common";

// NestJS를 실행시키는 함수
// NestJS에서는 진입점을 bootstrap()으로 이름 짓는 것이 관례이다.
async function bootstrap() {

  (...)

  // 전역 파이프에 validationPipe 객체 추가
  app.useGlobalPipes(new ValidationPipe());

  (...)
}

bootstrap();
```

### Step 3 - Dto 객체

다음과 같이 Dto 객체를 생성한 다음 `class-validator`를 임포트하여 유효성 검증을 수행합니다.

```typescript
import { IsEmail, IsString } from "class-validator";

// email, password, username 필드를 만들고 데코레이터 붙이기
export class CreateUserDto {
  @IsEmail()
  email: string;

  @IsString()
  password: string;

  @IsString()
  username: string;
}

// 업데이트의 유효성 검증 시 사용할 DTO
export class UpdateUserDto {
  @IsString()
  password: string;

  @IsString()
  username: string;
}
```

유효성 검증 데코레이터 종류는 다음 링크에 정리되어 있습니다.

<a href="https://github.com/typestack/class-validator?tab=readme-ov-file#validation-decorators" target="_blank">https://github.com/typestack/class-validator?tab=readme-ov-file#validation-decorators</a>

## 참고 자료

- <a href="https://docs.nestjs.com/pipes" target="_blank">Pipes | NestJS - A progressive Node.js framework</a>
- <a href="https://docs.nestjs.com/techniques/validation" target="_blank">Validation | NestJS - A progressive Node.js framework</a>
- <a href="https://github.com/typestack/class-validator" target="_blank">class-validator</a>

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

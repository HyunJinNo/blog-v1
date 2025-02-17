---
layout: post
title: NestJS 가드 사용 방법
description: >
  NestJS에서 가드 사용 방법에 대해 설명하는 페이지입니다.
image:
  path: /assets/img/back-end/back-end.jpg
related_posts:
  - None
sitemap: true
comments: false
---

<i>Environment</i>

- <i>Node.js v20.11.1</i>

<h2>목차</h2>

- [가드 (Guards)](#가드-guards)
- [Step 1 - cookie-parser 패키지](#step-1---cookie-parser-패키지)
- [Step 2 - Guard 클래스 작성하기](#step-2---guard-클래스-작성하기)
- [Step 3 - 가드 사용하기](#step-3---가드-사용하기)
- [Comments](#comments)

## 가드 (Guards)

`NestJS`에서 인증할 때 `가드(Guards)`라는 미들웨어를 보편적으로 사용합니다. `가드`는 특정 조건에 따라 요청을 처리할지 거부할지를 결정하는 데 사용되며, 주로 인증 및 권한 부여와 같은 보안 관련 작업을 처리하는 데 초점을 맞춥니다. 가드는 @Injectable 데코레이터가 붙어 있고 CanActivate 인터페이스를 구현한 클래스를 의미합니다. @UseGuards 데코레이터를 사용하여 가드를 사용할 수 있습니다.

## Step 1 - cookie-parser 패키지

`cookie-parser` 패키지는 HTTP 헤더에서 쿠키를 읽기 위해 사용하는 패키지입니다. 다음 명령어를 입력하여 `cookie-parser` 패키지를 설치합니다.

```bash
npm install cookie-parser @types/cookie-parser
```

패키지를 설치한 이후에는 `main.ts`에서 다음과 같이 `use()` 함수를 사용해 cookie-parser 설정을 합니다.

```typescript
// main.ts

(...)

import * as cookieParser from "cookie-parser";

// NestJS를 실행시키는 함수
// NestJS에서는 진입점을 bootstrap()으로 이름 짓는 것이 관례이다.
async function bootstrap() {
  (...)

  // cookie-parser 설정
  app.use(cookieParser());

  (...)
}

bootstrap();
```

## Step 2 - Guard 클래스 작성하기

가드를 사용하려면 다음과 같이 `CanActivate` 인터페이스를 구현해야 합니다. `CanActivate` 인터페이스를 구현하려면 `canActivate()` 메서드를 구현해야 합니다. `canActivate()`는 `boolean` 또는 `Promise<boolean>`을 반환하며 true일 경우 핸들러 메서드를 실행하고 false이면 `403 Forbidden` 에러를 응답합니다.

```typescript
// auth.guard.ts

import { CanActivate, ExecutionContext, Injectable } from "@nestjs/common";
import { AuthService } from "./auth.service";

// Injectable이 있으니 프로바이더
// CanActivate 인터페이스 구현
@Injectable()
export class LoginGuard implements CanActivate {
  // authService를 주입받음
  constructor(private authService: AuthService) {}

  // CanActivate 인터페이스의 메서드
  async canActivate(context: ExecutionContext): Promise<boolean> {
    // context에서 request 정보를 가져옴.
    const request = context.switchToHttp().getRequest();

    // 쿠키가 있으면 인증된 것
    if (request.cookies["login"]) {
      return true;
    }

    // 쿠키가 없으면 request의 body 정보 확인
    if (!request.body.email || !request.body.password) {
      return false;
    }

    // 인증 로직은 기존의 authService.validateUser 사용
    const user = await this.authService.validateUser(
      request.body.email,
      request.body.password
    );

    // 유저 정보가 없으면 false 반환
    if (!user) {
      return false;
    }

    // 유저 정보가 있으면 request에 user 정보를 추가하고 true 반환
    request.user = user;
    return true;
  }
}
```

위의 코드에서 `context`는 `ExecutionContext` 타입으로 주로 Request나 Response 객체를 가져올 때 사용합니다. `context.switchToHttp().getRequest()`와 같이 Request 객체를 가져올 수 있습니다. **가드 내에서 응답에 쿠키를 설정할 수 없다는 점을 주의합니다. 또한 가드는 모든 미들웨어의 실행이 끝난 다음 실행되며 filter나 pipe보다는 먼저 실행됩니다.**

## Step 3 - 가드 사용하기

가드를 사용하려면 다음과 같이 `@UseGuards` 데코레이터를 사용합니다.

```typescript
// auth.controller.ts

(...)

@UseGuards(LoginGuard) // LoginGuard 사용
@Post('login2')
async login2(@Request() req, @Response() res) {
  // 쿠키 정보는 없지만 request에 user 정보가 있다면 응답값에 쿠키 정보 추가
  if (!req.cookies['login'] && req.user) {
    // 응답에 쿠키 정보 추가
    res.cookie('login', JSON.stringify(req.user), {
      httpOnly: true,
      maxAge: 1000 * 10, // 로그인 테스트를 고려해 10초로 설정
    });
  }
  return res.send({ message: 'login2 success' });
}


// 로그인을 할 때만 실행되는 메서드
@UseGuards(LoginGuard)
@Get('test-guard')
testGuard() {
  return '로그인된 때만 이 글이 보입니다.';
}

(...)
```

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

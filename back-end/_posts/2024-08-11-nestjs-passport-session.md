---
layout: post
title: NestJS 패스포트 및 세션 사용 방법
description: >
  NestJS에서 패스포트 및 세션 사용 방법에 대해 설명하는 페이지입니다.
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

- [패스포트(Passport)란?](#패스포트passport란)
- [세션(Session)이란?](#세션session이란)
- [Step 1 - 패키지 설치하기](#step-1---패키지-설치하기)
- [Step 2 - 패스포트와 세션 설정하기](#step-2---패스포트와-세션-설정하기)
- [Step 3 - Guard 구현하기](#step-3---guard-구현하기)
- [Step 4 - LocalStrategy 구현하기](#step-4---localstrategy-구현하기)
- [Step 5 - SessionSerializer 구현하기](#step-5---sessionserializer-구현하기)
- [Step 6 - 모듈에 설정 추가하기](#step-6---모듈에-설정-추가하기)
- [Step 7 - Controller 설정하기](#step-7---controller-설정하기)
- [Comments](#comments)

## 패스포트(Passport)란?

`NestJS`에서 `패스포트(Passport)`는 다양한 인증 전략을 간편하게 구현할 수 있게 해주는 인증 미들웨어입니다. 패스포트는 로컬(Local), OAuth, JWT(JSON Web Token) 등 다양한 인증 방식을 지원합니다. 패스포트를 사용하면 인증 로직을 쉽게 분리해서 개발할 수 있습니다.

`strategy`는 패스포트에서 인증 로직 수행을 담당하는 클래스를 의미하며, 패스포트 사용 시 인증 로직은 `Strategy` 파일을 생성해서 사용합니다.

## 세션(Session)이란?

`세션(Session)`이란 서버 측에서 사용자의 상태를 저장하는 방식으로, 사용자가 웹사이트에 접속할 때 생성된 고유한 세션 ID를 통해 서버에서 사용자를 식별할 수 있습니다. 세션 ID는 보통 쿠키에 저장되어 클라이언트로 전송됩니다.

세션 기반 인증 시스템에서 사용자가 로그인을 하면, 서버는 세션 저장소에 사용자의 정보를 조회하고 세션 ID를 발급합니다. 발급된 ID는 주로 브라우저의 쿠키에 저장합니다. 그 다음에 사용자가 다른 요청을 보낼 때마다 서버는 세션 저장소에서 세션을 조회한 후 로그인 여부를 결정하여 작업을 처리하고 응답을 합니다. 세션 저장소는 주로 메모리, 디스크, 데이터베이스 등을 사용합니다.

`NestJS`에서 `세션(Session)`은 사용자 인증 및 상태 관리를 위한 방법 중 하나로, 세션을 통해 서버는 사용자의 상태를 유지하고, 로그인 상태나 기타 사용자 정보를 지속적으로 관리할 수 있습니다. 세션을 사용하면 서버 자원을 사용하는 것이므로 서버에 부하를 주는 단점이 있지만, 중요한 정보에 대해 위조, 변조, 탈취가 불가능하므로 보안적인 측면에서 더 안전합니다.

## Step 1 - 패키지 설치하기

다음 명령어를 입력하여 `passport` 라이브러리와 `express-session` 라이브러리를 설치합니다.

```bash
npm install @nestjs/passport passport passport-local express-session
```

```bash
npm install --save-dev @types/passport-local @types/express-session
```

각 패키지에 대해 설명하자면 다음과 같습니다.

- `passport`
  - 패스포트 라이브러리
- `passport-local`
  - 유저 아이디와 패스워드로 인증하는 로컬 전략을 사용할 때의 Strategy
- `express-session`
  - 세션 저장 라이브러리

## Step 2 - 패스포트와 세션 설정하기

다음과 같이 `main.ts`에 패스포트와 세션 설정 코드를 추가합니다.

```typescript
(...)

import * as session from "express-session";
import * as passport from "passport";

// NestJS를 실행시키는 함수
// NestJS에서는 진입점을 bootstrap()으로 이름 짓는 것이 관례이다.
async function bootstrap() {

  (...)

  // 세션 사용
  app.use(
    session({
      secret: "very-important-secret", // 세션 암호화에 사용되는 키
      resave: false, // 세션을 항상 저장할 지 여부
      saveUninitialized: false, // 세션이 저장되기 전에는 초기화되지 않은 상태로 세션을 미리 만들어 저장
      cookie: { maxAge: 1000 * 60 * 60 }, // 쿠키 유효기간: 1시간
    })
  );

  // passport 초기화 및 세션 저장소 초기화
  app.use(passport.initialize());
  app.use(passport.session());

  (...)
}

bootstrap();
```

위의 코드를 설명하자면 다음과 같습니다.

- `secret`
  - 세션 암호화에 사용되는 키로, 외부로 유출되지 않도록 주의해야 합니다.
- `resave`
  - 세션 데이터가 변경되지 않더라도 세션을 다시 저장할지 여부를 나타냅니다.
- `saveUninitialized`
  - 초기화되지 않은 세션을 저장할지 여부를 나타냅니다.

## Step 3 - Guard 구현하기

다음과 같이 **로그인에 사용할 가드**와 **로그인 후 인증에 사용할 가드**를 구현합니다.

```typescript
// auth.guard.ts

import { CanActivate, ExecutionContext, Injectable } from "@nestjs/common";
import { AuthGuard } from "@nestjs/passport";

// AuthGuard 상속
@Injectable()
export class LocalAuthGuard extends AuthGuard("local") {
  async canActivate(context: ExecutionContext): Promise<boolean> {
    const result = (await super.canActivate(context)) as boolean;

    // 로컬 스트래티지 실행
    const request = context.switchToHttp().getRequest();
    await super.logIn(request); // 세션 저장
    return result;
  }
}

@Injectable()
export class AuthenticatedGuard implements CanActivate {
  canActivate(context: ExecutionContext): Promise<boolean> {
    const request = context.switchToHttp().getRequest();
    return request.isAuthenticated(); // 세션에서 정보를 읽어서 인증 확인
  }
}
```

위의 코드를 설명하자면 다음과 같습니다.

- `@nestjs/passport`
  - 패스포트 인증에 가드를 사용할 수 있도록 감싸둔 `AuthGuard`를 제공하는 라이브러리입니다.
- `AuthGuard('local')`
  - 로컬 strategy를 사용한다는 의미입니다.
- `super.canActive()`
  - 로컬 strategy를 사용하므로 해당 부분에서 `passport-local`의 로직을 구현한 메서드를 실행합니다.
- `super.logIn()`
  - 로그인 처리 및 세션을 저장합니다.

## Step 4 - LocalStrategy 구현하기

다음과 같이 유저 아이디와 패스워드로 인증하는 LocalStrategy를 생성합니다.

```typescript
// local.strategy.ts

import { Injectable } from "@nestjs/common";
import { PassportStrategy } from "@nestjs/passport";
import { Strategy } from "passport-local";
import { AuthService } from "./auth.service";

@Injectable()
export class LocalStrategy extends PassportStrategy(Strategy) {
  // PassportStrategy 믹스인
  constructor(private authService: AuthService) {
    super({ usernameField: "email" }); // 기본값이 username이므로 email로 변경해줌
  }

  // 유저 정보의 유효성 검증
  async validate(email: string, password: string): Promise<any> {
    const user = await this.authService.validateUser(email, password);
    if (!user) {
      return null; // null이면 401 에러 발생
    }
    return user; // null이 아니면 user 정보 반환
  }
}
```

`PassportStrategy(Strategy)`는 믹스인이라고 불리는 방법으로, 클래스의 일부만 확장하고 싶을 때 사용합니다. 또한 local-strategy에는 인증 시 사용하는 필드명이 username, password로 정해져 있습니다. 위의 코드에서는 email, password로 인증하게 되므로 usernameField를 email로 변경하였습니다.

추가적으로 위에서 사용한 local-strategy 인증 방법 이외에도 다양한 strategy가 있습니다.

| 인증 방법   | 패키지명          | 설명                                             |
| ----------- | ----------------- | ------------------------------------------------ |
| Local       | passport-local    | 유저명과 패스워드를 사용해 인증                  |
| OAuth       | passport-oauth    | 페이스북, 구글, 트위터 등의 외부 서비스에서 인증 |
| SAML        | passport-saml     | SAML 신원 제공자에서 인증, OneLogin, Okta 등     |
| JWT         | passport-jwt      | JSON Web Token을 사용해 인증                     |
| AWS Cognito | passport-cognito  | AWS의 Cognito user pool을 사용해 인증            |
| LDAP        | passport-ldapauth | LDAP 디렉토리를 사용해 인증                      |

이외의 인증 방법에 대해서는 다음 링크를 참고하시길 바랍니다.

<a href="https://www.passportjs.org/" target="_blank">Passport.js</a>

## Step 5 - SessionSerializer 구현하기

다음과 같이 세션에 정보를 저장하거나, 세션에서 정보를 가져오는 SessionSerializer를 생성합니다.

```typescript
// session.serializer.ts

import { Injectable } from "@nestjs/common";
import { PassportSerializer } from "@nestjs/passport";
import { UserService } from "../user/user.service";

// PassportSerializer를 상속받음
@Injectable()
export class SessionSerializer extends PassportSerializer {
  // userService를 주입받음
  constructor(private userService: UserService) {
    super();
  }

  // 세션에서 정보를 저장할 때 사용
  serializeUser(user: any, done: (err: Error, user: any) => void): any {
    done(null, user.email); // 세션에 저장할 정보
  }

  // 세션에서 정보를 꺼내올 때 사용
  async deserializeUser(
    payload: any,
    done: (err: Error, payload) => void
  ): Promise<any> {
    const user = await this.userService.getUser(payload);

    // 유저 정보가 없는 경우 done() 함수에 에러 전달
    if (!user) {
      done(new Error("No User"), null);
      return;
    }

    // eslint-disable-next-line @typescript-eslint/no-unused-vars
    const { password, ...userInfo } = user;

    // 유저 정보가 있다면 유저 정보 반환
    done(null, userInfo);
  }
}
```

위의 코드에서 `PassportStrategy`는 `serializeUser()`, `deserializeUser()`, `getPassportInstance()`를 제공합니다. 각 메서드에 대해 설명하자면 다음과 같습니다.

- `serializeUser()`
  - 세션에 정보를 저장합니다.
- `deserializeUser()`
  - 세션에서 가져온 정보로 유저 정보를 반환합니다.
- `getPassportUser()`
  - 패스포트 인스턴스를 가져옵니다. 패스포트 인스턴스의 데이터가 필요한 경우 사용합니다.

또한 `payload`는 세션에서 꺼내온 값을 의미하며, 세션 정보가 없는 경우 `403 에러`를 응답합니다.

## Step 6 - 모듈에 설정 추가하기

다음과 같이 모듈에 세션을 사용할 수 있도록 설정을 추가합니다. `{ session: true }`을 지정하여 세션을 사용할 수 있게 해줍니다.

```typescript
import { Module } from "@nestjs/common";
import { AuthController } from "./auth.controller";
import { AuthService } from "./auth.service";
import { UserModule } from "../user/user.module";
import { PassportModule } from "@nestjs/passport";
import { LocalStrategy } from "./local.strategy";
import { SessionSerializer } from "./session.serializer";

@Module({
  imports: [UserModule, PassportModule.register({ session: true })],
  controllers: [AuthController],
  providers: [AuthService, LocalStrategy, SessionSerializer],
})
export class AuthModule {}
```

## Step 7 - Controller 설정하기

마지막으로 로그인 테스트를 위한 메서드들을 추가합니다.

```typescript
// auth.controller.ts

  (...)

  @UseGuards(LocalAuthGuard)
  @Post('login3')
  login3(@Request() req) {
    return req.user;
  }

  @UseGuards(AuthenticatedGuard)
  @Get('test-guard2')
  testGuardWithSession(@Request() req) {
    return req.user;
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

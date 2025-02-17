---
layout: post
title: Express.js 패스포트 및 세션 사용 방법
description: >
  Express.js에서 패스포트 및 세션 사용 방법에 대해 설명하는 페이지입니다.
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
- [Step 3 - AuthService와 AuthController 구현하기](#step-3---authservice와-authcontroller-구현하기)
- [Step 4 - LocalStrategy 구현하기](#step-4---localstrategy-구현하기)
- [Step 5 - SessionSerializer 구현하기](#step-5---sessionserializer-구현하기)
- [Step 6 - LocalStrategy와 SessionSerializer 설정하기](#step-6---localstrategy와-sessionserializer-설정하기)
- [Step 7 - 인증 미들웨어 구현하기](#step-7---인증-미들웨어-구현하기)
- [Step 8 - Route 설정하기](#step-8---route-설정하기)
- [Comments](#comments)

## 패스포트(Passport)란?

`패스포트(Passport)`는 다양한 인증 전략을 간편하게 구현할 수 있게 해주는 인증 미들웨어입니다. 패스포트는 로컬(Local), OAuth, JWT(JSON Web Token) 등 다양한 인증 방식을 지원합니다. 패스포트를 사용하면 인증 로직을 쉽게 분리해서 개발할 수 있습니다.

`strategy`는 패스포트에서 인증 로직 수행을 담당하는 클래스를 의미하며, 패스포트 사용 시 인증 로직은 `Strategy` 파일을 생성해서 사용합니다.

## 세션(Session)이란?

`세션(Session)`이란 서버 측에서 사용자의 상태를 저장하는 방식으로, 사용자가 웹사이트에 접속할 때 생성된 고유한 세션 ID를 통해 서버에서 사용자를 식별할 수 있습니다. 세션 ID는 보통 쿠키에 저장되어 클라이언트로 전송됩니다.

세션 기반 인증 시스템에서 사용자가 로그인을 하면, 서버는 세션 저장소에 사용자의 정보를 조회하고 세션 ID를 발급합니다. 발급된 ID는 주로 브라우저의 쿠키에 저장합니다. 그 다음에 사용자가 다른 요청을 보낼 때마다 서버는 세션 저장소에서 세션을 조회한 후 로그인 여부를 결정하여 작업을 처리하고 응답을 합니다. 세션 저장소는 주로 메모리, 디스크, 데이터베이스 등을 사용합니다.

`세션(Session)`은 사용자 인증 및 상태 관리를 위한 방법 중 하나로, 세션을 통해 서버는 사용자의 상태를 유지하고, 로그인 상태나 기타 사용자 정보를 지속적으로 관리할 수 있습니다. 세션을 사용하면 서버 자원을 사용하는 것이므로 서버에 부하를 주는 단점이 있지만, 중요한 정보에 대해 위조, 변조, 탈취가 불가능하므로 보안적인 측면에서 더 안전합니다.

## Step 1 - 패키지 설치하기

다음 명령어를 입력하여 `passport` 라이브러리와 `express-session` 라이브러리를 설치합니다.

```bash
npm install passport passport-local express-session bcrypt mysql2
```

각 패키지에 대해 설명하자면 다음과 같습니다.

- `passport`
  - 패스포트 라이브러리
- `passport-local`
  - 유저 아이디와 패스워드로 인증하는 로컬 전략을 사용할 때의 Strategy
- `express-session`
  - 세션 저장 라이브러리
- `bcrypt`
  - 단방향 해시 함수를 사용한 비밀번호 암호화 라이브러리
- `mysql2`
  - MySQL 모듈로, TypeORM과 같은 라이브러리를 사용하셔도 무방합니다.

## Step 2 - 패스포트와 세션 설정하기

다음과 같이 `app.js`에 패스포트와 세션 설정 코드를 추가합니다.

```javascript
(...)

import session from "express-session";

(...)


// 세션 설정
app.use(
  session({
    secret: process.env.SESSION_SECRET, // 세션 암호화에 사용되는 키
    resave: false, // 세션을 항상 저장할 지 여부
    saveUninitialized: false, // 세션이 저장되기 전에는 초기화되지 않은 상태로 세션을 미리 만들어 저장
    cookie: { maxAge: 1000 * 60 * 60, httpOnly: true }, // 쿠키 유효기간: 1시간
  })
);

// passport 초기화 및 세션 저장소 초기화
app.use(passport.initialize()); // req에 passport 요청을 심음
app.use(passport.session()); // req.session 객체에 passport정보를 추가 저장

(...)
```

위의 코드를 설명하자면 다음과 같습니다.

- `secret`
  - 세션 암호화에 사용되는 키로, 외부로 유출되지 않도록 주의해야 합니다.
- `resave`
  - 세션 데이터가 변경되지 않더라도 세션을 다시 저장할지 여부를 나타냅니다.
- `saveUninitialized`
  - 초기화되지 않은 세션을 저장할지 여부를 나타냅니다.

주의할 점으로 `app.use(session({}))` 부분이 패스포트 초기화 부분보다 먼저 작성되어야 합니다. 또한 `app.use(session({}))` 세션 설정 부분이 다른 미들웨어보다 먼저 설정되어야 합니다.

## Step 3 - AuthService와 AuthController 구현하기

다음과 같이 인증 시 사용하는 AuthService를 구현합니다.

```javascript
import pool from "../../db/db.js";
import bcrypt from "bcrypt";

/**
 * 닉네임과 비밀번호로 사용자 정보를 확인하는 함수
 *
 * @param {string} nickname 닉네임
 * @param {string} password 비밀번호
 * @returns {Promise<{ id: number, nickname: string } | null>}
 */
const validateUser = async (nickname, password) => {
  const [rows] = await pool.execute(
    "select id, nickname as name, hashed_password as hashedPassword from users where nickname = ?;",
    [nickname]
  );

  // 해당 닉네임을 가진 사용자가 없는 경우
  if (rows.length === 0) {
    return null;
  }

  const { id, name, hashedPassword } = rows[0];

  // 비밀번호를 따로 뽑아냅니다.

  bcrypt.compare(password, hashedPassword).then((flag) => {
    if (flag) {
      // 비밀번호가 일치하면 성공
      return {
        id: id,
        nickname: name,
      };
    } else {
      return null;
    }
  });

  if (await bcrypt.compare(password, hashedPassword)) {
    // 비밀번호가 일치하면 성공
    return {
      id: id,
      nickname: name,
    };
  }
  return null;
};

/**
 * 사용자 id 값이 주어졌을 때 사용자 정보를 확인하는 함수
 *
 * @param {number} id 사용자 id 값
 */
const findUser = async (id) => {
  const [rows] = await pool.execute(
    "select id, nickname, hashed_password from users where id = ?;",
    [id]
  );
  return rows[0];
};
```

다음으로 사용자의 로그인 HTTP 요청을 처리할 AuthController를 구현합니다.

```javascript
import { request, response } from "express";
import authService from "./auth.service.js";

/**
 * 로그인
 *
 * POST /api/auth/login
 *
 * @param {request} req
 * @param {response} res
 */
const logIn = async (req, res) => {
  res.status(200).json(req.user);
};
```

위의 코드에서 `req.user`는 passport를 사용 시 유저 정보를 가져오는 방법입니다.

## Step 4 - LocalStrategy 구현하기

다음과 같이 닉네임과 비밀번호로 인증하는 LocalStrategy를 생성합니다.

```javascript
import passport from "passport";
import LocalStrategy from "passport-local";
import authService from "./auth.service.js";

export const useLocalStrategy = () => {
  passport.use(
    new LocalStrategy.Strategy(
      { usernameField: "nickname", passwordField: "password" },
      async (nickname, password, done) => {
        const user = await authService.validateUser(nickname, password);
        if (!user) {
          return done(null, false, {
            message: "닉네임 또는 비밀번호 정보가 올바르지 않습니다.",
          });
        }
        return done(null, user);
      }
    )
  );
};
```

LocalStrategy는 기본적으로 인증 시 사용하는 필드명이 username과 password로 정해져 있습니다. 저는 닉네임과 비밀번호로 인증을 할 예정이므로 `{ usernameField: "nickname", passwordField: "password" }`와 같이 usernameField를 nickname으로 변경하였습니다.

추가적으로 위에서 사용한 LocalStrategy 인증 방법 이외에도 다양한 strategy가 있습니다.

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

```javascript
import passport from "passport";
import authService from "./auth.service.js";

export const useSession = () => {
  passport.serializeUser((user, done) => {
    done(null, user.id);
  });

  passport.deserializeUser(async (id, done) => {
    const user = await authService.findUser(id);

    // 사용자 정보가 없는 경우 done() 함수에 에러 전달
    if (!user) {
      done(new Error("No User"), null);
      return;
    }

    // 사용자 정보가 있다면 사용자 정보 반환
    done(null, user);
  });
};
```

각 메서드에 대해 설명하자면 다음과 같습니다.

- `serializeUser()`
  - 세션에 정보를 저장합니다.
- `deserializeUser()`
  - 세션에서 가져온 정보로 유저 정보를 반환합니다.

## Step 6 - LocalStrategy와 SessionSerializer 설정하기

위에서 작성한 LocalStrategy와 SessionSerializer를 사용하기 위해 `app.js`에 `useLocalStrategy()`와 `useSession()` 코드를 추가합니다.

```javascript
// passport 초기화 및 세션 저장소 초기화
app.use(passport.initialize()); // req에 passport 요청을 심음
app.use(passport.session()); // req.session 객체에 passport정보를 추가 저장
useLocalStrategy(); // LocalStrategy 사용
useSession(); // Session 사용
```

## Step 7 - 인증 미들웨어 구현하기

먼저 인증 시 사용할 미들웨어를 구현합니다.

```javascript
import { request, response } from "express";
import passport from "passport";

/**
 * @param {request} req
 * @param {response} res
 * @param {import("express").NextFunction} next
 */
const authenticate = (req, res, next) => {
  passport.authenticate("local", (authError, user, info) => {
    if (authError) {
      return next(authError);
    }

    if (!user) {
      return res.status(401).json({ message: info.message });
    }

    req.logIn(user, (err) => {
      if (err) {
        console.error(err);
        return res.sendStatus(500);
      }
      next();
    });
  })(req, res, next);
};
```

## Step 8 - Route 설정하기

마지막으로 다음과 같이 route 설정을 진행합니다.

```javascript
import express from "express";
import authController from "../modules/auth/auth.controller.js";
import authMiddleware from "../modules/auth/auth.middleware.js";

export const router = express.Router();

router.post("/login", authMiddleware.authenticate, authController.logIn);
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

---
layout: post
title: NestJS 파일 업로드
description: >
  NestJS에서 파일 업로드 기능을 구현하는 방법을 설명하는 페이지입니다.
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

- [Step 1 - @types/multer 패키지 설치하기](#step-1---typesmulter-패키지-설치하기)
- [Step 2 - 단일 파일 업로드 API 구현하기](#step-2---단일-파일-업로드-api-구현하기)
- [Step 3 - 파일 저장 경로 지정하기](#step-3---파일-저장-경로-지정하기)
- [Step 4 - 정적 파일 서비스하기](#step-4---정적-파일-서비스하기)
- [참고 자료](#참고-자료)
- [Comments](#comments)

## Step 1 - @types/multer 패키지 설치하기

`multer`란 파일 업로드에 사용되는 multipart/form-data를 다루는 Node.js 라이브러리입니다. 먼저 다음 명령어를 입력하여 `@types/multer` 패키지를 설치합니다.

```bash
npm install --save-dev @types/multer
```

## Step 2 - 단일 파일 업로드 API 구현하기

다음과 같이 단일 파일 업로드 API를 구현합니다.

```typescript
// app.controller.ts

  (...)

  @Post('file-upload') // POST 메서드로 localhost:3000/file-upload 호출 시 동작
  @UseInterceptors(FileInterceptor('file', multerOptions)) // 파일 인터셉터
  // 인터셉터에서 준 파일을 받음
  fileUpload(@UploadedFile() file: Express.Multer.File) {
    // 텍스트 파일 내용 출력
    console.log(file.buffer.toString('utf-8'));

    return `${file.originalname} File Uploaded check http://localhost:3000/uploads/${file.filename}`;
  }

  (...)
```

위의 코드를 설명하자면 다음과 같습니다.

- 파일 업로드는 `POST 메서드`로만 가능하며, `Content-Type`을 `multipart/form-data`로 지정해야 합니다.
- `@UseInterceptors`
  - 인터셉터를 사용하기 위해 사용하는 데코레이터입니다.
  - 여기서 인터셉터는 클라이언트와 서버 간의 요청 및 응답 간에 로직을 추가하는 미들웨어를 의미합니다.
- `FileInterceptor`
  - 클라이언트의 요청에 대해서 파일명이 `'file'`인 파일이 있는지 확인하기 위해 사용하는 파일 인터셉터입니다.
  - `FileInterceptor`의 첫 번째 파라미터에는 폼 필드의 이름을 넣습니다.
  - 두 번째 파라미터에는 여러 가지 옵션을 지정합니다.
- `@UploadedFile()`
  - 파라미터로 받은 값 중에서 file 객체를 가져오는데 사용하는 데코레이터입니다.
- `Express.Multer.File`
  - 파일의 타입은 `Express.Multer.File`에 해당합니다.

## Step 3 - 파일 저장 경로 지정하기

`FileInterceptor`의 두 번째 파라미터에는 `multer`에서 지원하는 여러 가지 옵션을 지정할 수 있습니다. `multer`에서 지원하는 옵션은 다음과 같습니다.

| 옵션명       | 설명                                                                                |
| ------------ | ----------------------------------------------------------------------------------- |
| storage      | 파일이 저장될 위치와 파일 이름을 제어합니다.                                        |
| fileFilter   | 어떤 형식의 파일을 허용할지 제어합니다.                                             |
| limits       | 필드명, 값, 파일 개수, 파일 용량, multipart 폼의 파라미터 개수, 헤더 개수 제한 설정 |
| preservePath | 파일의 전체 경로를 유지할지 여부                                                    |

파일은 디스크에 저장하는 `diskStorage()`와 메모리에 저장하는 `memoryStorage()`가 있으며, 아무 것도 설정하지 않은 상태에서는 `memoryStorage()`를 사용합니다. 디스크를 사용하기 위해 다음과 같이 `multer.option.ts` 파일을 작성합니다.

```typescript
// multer.option.ts

import { randomUUID } from "crypto";
import { diskStorage } from "multer";
import { extname, join } from "path";

// multerOption 객체 선언
export const multerOptions = {
  // 디스크 스토리지 사용
  storage: diskStorage({
    destination: join(__dirname, "..", "uploads"), // 파일 저장 경로 설정
    filename: (req, file, callback) => {
      // 파일명 설정
      callback(null, randomUUID() + extname(file.originalname));
    },
  }),
};
```

위의 코드를 설명하자면 다음과 같습니다.

- `diskStorage()`
  - 파일을 저장하는 위치를 디스크로 설정하는 부분입니다.
  - `destination` 부분은 파일이 저장될 경로를 설정하는 부분입니다.
  - `filename` 부분은 파일이 저장될 때 파일명을 설정하는 부분입니다.
- `randomUUID()`
  - `UUID`란 <b>범용 고유 식별자(Universally Unique Identifier)</b>라는 뜻으로, 여기서는 파일명이 중복되지 않게 저장하기 위해 사용합니다.
- `extname()`
  - 클라이언트로부터 받은 파일의 확장자명을 붙일 때 사용합니다.

## Step 4 - 정적 파일 서비스하기

`정적 파일(Static File)`이란 <b>텍스트, 이미지, 동영상 같이 한 번 저장되면 변경되지 않는 파일</b>을 의미합니다. `NestJS`에서 정적 파일을 서비스하기 위해 다음 명령어를 입력하여 패키지를 설치합니다.

```bash
npm install @nestjs/serve-static
```

패키지를 설치한 후에는 `ServeStaticModule`을 초기화해야 합니다. 다음과 같이 `app.module.ts` 파일에서 초기화를 진행합니다.

```typescript
// app.module.ts

(...)

import { ServeStaticModule } from "@nestjs/serve-static";
import { join } from "path";

// 모듈 데코레이터
@Module({
  imports: [
    (...)

    ServeStaticModule.forRoot({
      // 초기화 함수 실행
      rootPath: join(__dirname, "..", "uploads"), // 실제 파일이 있는 디렉토리 경로
      serveRoot: "/uploads", // url 뒤에 붙을 경로를 지정
    }),

    (...)

  ],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

위의 코드를 설명하자면 다음과 같습니다.

- `ServeStaticModule.forRoot()`
  - 모듈을 초기화하는 부분입니다.
  - imports 옵션에 추가해야 합니다.
- `rootPath`
  - 실제 파일이 존재하는 디렉토리 경로를 지정합니다.
- `serveRoot`
  - <b><u>해당 옵션이 존재하지 않는 경우</u></b> 업로드한 파일에 `http://localhost:3000/{파일명}`으로 접근할 수 있습니다.
  - 위의 예시에서 serveRoot를 `"/uploads"`로 지정하였으므로 업로드한 파일에 `http://localhost:3000/uploads/{파일명}`으로 접근할 수 있습니다.

## 참고 자료

- <a href="https://docs.nestjs.com/techniques/file-upload" target="_blank">File Upload | NestJS - A progressive Node.js framework</a>
- <a href="https://github.com/expressjs/multer/blob/master/doc/README-ko.md" target="_blank">Multer</a>

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

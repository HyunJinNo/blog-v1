---
layout: post
title: Next.js에서 ReactQuill 사용 방법
description: >
  Next.js에서 ReactQuill 사용 방법에 대해 설명하는 페이지입니다.
image:
  path: /assets/img/front-end/front-end.jpg
  srcset:
    1060w: /assets/img/front-end/front-end.jpg
    530w: /assets/img/front-end/front-end.jpg
    265w: /assets/img/front-end/front-end.jpg
related_posts:
  - None
sitemap: true
comments: false
---

<i>Environment</i>

- <i>Next.js 14.2.3</i>
- <i>react-quill v2.0.0</i>

<h2> 목차 </h2>

- [Step 1 - ReactQuill 패키지 설치](#step-1---reactquill-패키지-설치)
- [Step 2 - ReactQuill 모듈 로드하기](#step-2---reactquill-모듈-로드하기)
  - [document is not found](#document-is-not-found)
  - [Dynamic Import](#dynamic-import)
- [Step 3 - Quill 에디터 사용하기](#step-3---quill-에디터-사용하기)
- [Step 4 - Quill 에디터 커스텀하기](#step-4---quill-에디터-커스텀하기)
- [Comments](#comments)

## Step 1 - ReactQuill 패키지 설치

다음 명령어를 입력하여 ReactQuill 패키지를 설치합니다.

```bash
npm install react-quill
```

## Step 2 - ReactQuill 모듈 로드하기

### document is not found

`React` 프로젝트와 달리 `Next.js`에서 ReactQuill를 직접 로드하려고 하면 `document is not found` 에러가 발생합니다.
이는 ReactQuill에서 `document` 객체를 활용하기 때문입니다. 따라서 Next.js에서는 `document`가 먼저 로드된 후에 `ReactQuill` 모듈을
불러오도록 수정해야 합니다.

### Dynamic Import

`document`가 로드된 후 `ReactQuill` 모듈을 로드하기 위해선 `지연 로딩(lazy loading)`을 활용하면 됩니다. Next.js에서는 `지연 로딩(lazy loading)`을 두 가지 방식으로 지원합니다.

1. `next/dynamic`로 `동적 임포트(dynamic import)`하는 방법
2. `React.lazy()`와 `Suspense`를 이용하는 방법

이 중에서 `동적 임포트`를 통해 `ReactQuill` 모듈을 로드하면 됩니다. 다음과 같이 `next/dynamic`로 클라이언트 사이드(= 브라우저)에서 모듈을 불러오도록 설정합니다.

```typescript
import dynamic from "next/dynamic";

// react-quill은 서버 사이드 렌더링을 지원하지 않기 때문에
// 클라이언트 사이드에서 모듈을 불러오도록 설정한다.
// 이를 통해 "document is not found" 에러를 방지할 수 있다.
const ReactQuill = dynamic(() => import("react-quill"), {
  ssr: false,
  loading: () => <div>loading...</div>,
});
```

위의 코드에서 `ssr: false`는 pre-rendering, 즉 서버 사이드 렌더링을 하지 않고 클라이언트 사이드에서만 로드하겠다는 의미입니다. 또한 `loading`은 동적 임포트가 완료되기 전에 표시할 컴포넌트를 지정하는 부분입니다.

## Step 3 - Quill 에디터 사용하기

다음과 같이 Quill 에디터를 사용하는 컴포넌트를 작성합니다.

<img src="/assets/img/front-end/react-quill/reactquill0.png" alt="reactquill0" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

위의 코드의 주요 부분을 설명하자면 다음과 같습니다.

- `"use client"`: `ReactQuill` 모듈을 클라이언트 사이드에서 로드해야 하므로 컴포넌트를 클라이언트 컴포넌트로 선언합니다.
- `import "react-quill/dist/quill.snow.css";`: Quill 에디터에서 사용할 테마를 지정하는 부분입니다. css 파일을 임포트하고 난 후 `theme="snow"`와 같이 사용하려는 테마를 지정하면 됩니다. 테마로는 다음과 같이 2종류가 존재합니다.

  - bubble

    <img src="/assets/img/front-end/react-quill/reactquill1.png" alt="reactquill1" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

  - snow

    <img src="/assets/img/front-end/react-quill/reactquill2.png" alt="reactquill2" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

- `import "@/styles/quillEditor.css";`: Quill 에디터를 커스텀하는 css 파일을 임포트합니다. 해당 부분은 아래 [Step 4 - Quill 에디터 커스텀하기](#step-4---quill-에디터-커스텀하기) 부분에서 다루도록 하겠습니다.
- `modules`: Quill 에디터의 toolbar를 커스텀하는 부분입니다. 이미지 삽입 기능을 추가하거나 글씨 관련 기능 추가 등 여러 기능을 커스텀할 수 있습니다. 옵션들의 경우 다음 링크를 참고하시길 바랍니다.

  <a href="https://quilljs.com/docs/modules/toolbar" target="_blank">Toolbar Module - Quill Rich Text Editor</a>

  <img src="/assets/img/front-end/react-quill/reactquill3.png" alt="reactquill3" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

## Step 4 - Quill 에디터 커스텀하기

css 파일을 생성하여 위에서 생성한 Quill 에디터 컴포넌트를 커스텀할 수 있습니다. 다음은 Quill 에디터를 커스텀한 예시입니다.

```css
/* quillEditor.css */

.ql-toolbar {
  margin-top: 1.125rem;
  border-top-left-radius: 1rem;
  border-top-right-radius: 1rem;
}

.ql-container {
  border-bottom-left-radius: 1rem;
  border-bottom-right-radius: 1rem;
}

/* 최소 크기 지정 및 padding 제거 */
.ql-editor {
  padding-top: 1rem;
  padding-left: 1rem;
  min-height: 22.625rem;
  font-size: 1rem;
  line-height: 1.5;
}

.ql-editor.ql-blank::before {
  left: 1rem;
}
```

해당 css 파일을 Quill 에디터 컴포넌트에서 임포트하면 커스텀을 할 수 있습니다.

```typescript
// QuillEditor.tsx

import "@/styles/quillEditor.css";

(... 생략)
```

다음은 위에서 선언한 toolbar 옵션과 Quill 에디터를 커스텀한 결과입니다.

<img src="/assets/img/front-end/react-quill/reactquill4.png" alt="reactquill4" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

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

---
layout: post
title: VSCode에서 Prettier와 ESLint 설정 방법
description: >
  VSCode에서 Prettier와 ESLint 설정 방법에 대해 설명하는 페이지입니다.
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

- <i>eslint v9.11.1</i>

<h2>목차</h2>

- [개요](#개요)
- [Prettier란?](#prettier란)
- [ESLint란?](#eslint란)
- [Step 1 - VSCode Extensions](#step-1---vscode-extensions)
- [Step 2 - Prettier 설정하기](#step-2---prettier-설정하기)
  - [.prettierrc 파일 생성하기](#prettierrc-파일-생성하기)
  - [Editor: Format On Save](#editor-format-on-save)
- [Step 3 - ESLint 설정하기](#step-3---eslint-설정하기)
  - [ESLint 설치하기](#eslint-설치하기)
  - [.eslint.config.mjs 파일 설정하기](#eslintconfigmjs-파일-설정하기)
- [참고 자료](#참고-자료)
- [Comments](#comments)

## 개요

이번 글에서는 VSCode에서 Prettier와 ESLint 설정 방법에 대해 설명하겠습니다. <b>ESLint의 경우 v9.11.1 버전을 기준으로 설명하겠습니다.</b>

## Prettier란?

`Prettier`란 코드의 포맷을 자동으로 정리해주는 `코드 포맷터(Code Formatter)`입니다. JavaScript, TypeScript, HTML 등 다양한 프로그래밍 언어를 지원하며, Prettier를 사용하면 일관된 코드 스타일을 유지할 수 있습니다. 이를 통해 팀으로 협업하는 데 있어서 코드 스타일을 통일할 수 있다는 장점이 있습니다.

## ESLint란?

`ESLint`란 JavaScript와 TypeScript 코드에서 문법 오류나 잠재적 문제를 식별하거나, 코드 스타일 규칙을 통일하는데 사용하는 도구입니다. ESLint를 사용하면 코드 품질을 높이고 일관된 코드 스타일을 유지할 수 있습니다. 주로 버그를 예방하거나, 유지 보수를 쉽게 하기 위해 사용합니다.

## Step 1 - VSCode Extensions

먼저 다음과 같이 VSCode에서 Extensions 탭을 클릭하여 `Prettier - Code formatter`와 `ESLint`를 검색하여 설치합니다.

<img src="/assets/img/front-end/prettier-eslint/pic1.png" alt="pic1" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

<img src="/assets/img/front-end/prettier-eslint/pic2.png" alt="pic2" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

## Step 2 - Prettier 설정하기

VSCode에서 `Prettier - Code formatter`를 설치하고 설정 파일을 따로 생성하지 않으면 Prettier의 디폴트 규칙이 적용됩니다. Prettier의 디폴트 규칙 예시는 다음과 같습니다.

- `따옴표`: 작은 따옴표 대신 큰 따옴표를 사용합니다.
- `세미콜론`: 문장(statement)의 끝에 세미콜론을 사용합니다.
- `탭 너비`: 2칸
- `줄바꿈`: 80자 기준으로 자동으로 줄바꿈합니다.
- `탭 사용 여부`: 탭 대신 스페이스로 들여쓰기합니다.

### .prettierrc 파일 생성하기

Prettier 규칙을 설정하려면 다음과 같이 `.prettierrc` 설정 파일을 생성해야 합니다. **자주 사용되는 Prettier 옵션**을 기반으로 설정 파일을 생성한 예시는 다음과 같습니다.

```json
{
  "semi": true,
  "singleQuote": false,
  "printWidth": 80,
  "trailingComma": "all",
  "tabWidth": 2,
  "useTabs": false,
  "bracketSpacing": true,
  "jsxSingleQuote": false,
  "arrowParens": "always",
  "endOfLine": "auto"
}
```

위의 옵션들에 대해 설명하자면 다음과 같습니다.

- `semi`

  각 문장의 끝에 자동으로 세미콜론을 추가할지 여부를 설정합니다. 디폴트 값은 `true`입니다.

- `singleQuote`

  문자열에 작은 따옴표를 사용할지 여부를 설정합니다. 디폴트 값은 `큰 따옴표`입니다.

- `printWidth`

  한 줄에서 허용하는 최대 글자 수를 설정합니다. 최대 글자 수 이후에는 자동으로 줄바꿈 처리됩니다. 디폴트 값은 `80자`입니다.

- `trailingComma`

  콤마로 나열된 여러 줄에 걸친 객체나 배열의 마지막 항목 뒤에 콤마를 추가할지 여부를 설정합니다. 디폴트 값은 `es5`입니다.

  - `none`

    콤마를 설정하지 않습니다.

  - `es5`

    ES5에 해당하는 경우에만 콤마를 설정합니다.

  - `all`

    가능한 모든 곳에 콤마를 설정합니다.

- `tabWidth`

  들여쓰기에 사용할 공백(스페이스)의 수를 설정합니다. 디폴트 값은 `2칸`입니다.

- `useTabs`

  들여쓰기에 탭을 사용할지 여부를 설정합니다. 디폴트 값은 `false`입니다.

- `bracketSpacing`

  객체 리터럴에서 중괄호 내부에 스페이스를 추가할지 여부를 설정합니다. 디폴트 값은 `true`입니다.

- `jsxSingleQuote`

  JSX에서 작은 따옴표를 사용할지 여부를 설정합니다. 디폴트 값은 `false`입니다.

- `arrowParens`

  화살표 함수에서 파라미터 개수가 오직 한 개일 때 괄호를 사용할지 여부를 설정합니다. 디폴트 값은 `always`입니다.

  - `always`

    항상 괄호를 사용합니다.

  - `avoid`

    파라미터 개수가 오직 한 개일 때 괄호를 생략합니다.

- `endOfLine`

  파일 끝에 사용할 줄바꿈 문자를 설정합니다. 디폴트 값은 `lf`입니다.

  - `lf`

    `Line Feed`의 약자로, 유닉스 스타일(`\n`)을 적용합니다.

  - `crlf`

    `Carriage Return + Line Feed`의 약자로, 윈도우 스타일(`\r\n`)을 적용합니다.

  - `cr`

    `Carriage Return`의 약자로, 오직 `\r`을 사용합니다. 매우 드물게 사용되는 옵션입니다.

  - `auto`

    시스템에 맞게 자동 설정합니다.

위에서 설명한 것 외에 프로젝트에 따라 사용할 수 있는 옵션들이 여러 가지가 존재합니다. Prettier에서 사용할 수 있는 모든 옵션들은 다음 링크에서 참고하실 수 있습니다.

<a href="https://prettier.io/docs/en/options" target="_blank">https://prettier.io/docs/en/options</a>

### Editor: Format On Save

파일을 저장할 때마다 자동으로 코드 포맷팅을 적용하려면 VSCode 설정에서 다음과 같이 `Editor: Format On Save` 항목을 체크하면 됩니다.

<img src="/assets/img/front-end/prettier-eslint/pic3.png" alt="pic3" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

<img src="/assets/img/front-end/prettier-eslint/pic4.png" alt="pic4" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

<br />

이후 `Default Formatter`를 `Prettier - Code formatter`로 변경합니다.

<img src="/assets/img/front-end/prettier-eslint/pic5.png" alt="pic5" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

<br />

마지막으로 `settings.json` 파일을 확인하여 `"editor.formatOnSave": true`로 설정되어 있는지 확인합니다.

<img src="/assets/img/front-end/prettier-eslint/pic6.png" alt="pic6" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

<img src="/assets/img/front-end/prettier-eslint/pic7.png" alt="pic7" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

## Step 3 - ESLint 설정하기

<b>이 글에서 다루는 ESLint 설정 방법은 `v9.11.1` 버전을 기준으로 설명합니다. 다른 버전을 사용하고 있다면 설정 방법이 다를 수 있습니다.</b>

### ESLint 설치하기

먼저 다음 명령어를 입력하여 ESLint 설치 및 ESLint 초기화를 진행합니다.

```bash
npx eslint --init
```

해당 명령어를 실행하면 몇 가지 질문이 나옵니다. 자신이 사용하고 있는 프로젝트에 맞는 설정을 진행하면 됩니다. 저의 경우, 이번 글에서 제가 사용한 설정에 대해 밑줄 표시를 하였습니다.

- <b>How would you like to use ESLint? (ESLint를 어떻게 사용할 건가요?)</b>
  - To check syntax only
  - <b><u>To check syntax and find problems</u></b>
- <b>What type of modules does your project use? (프로젝트에서 사용하는 모듈 타입은 무엇인가요?)</b>
  - <b><u>JavaScript modules (import/export)</u></b>
  - CommonJS (require/exports)
  - None of these
- <b>Which framework does your project use? (사용 중인 프레임워크는 무엇인가요?)</b>
  - React
  - Vue.js
  - <b><u>None of these</u></b>
- <b>Does your project use TypeScript? (프로젝트에서 타입스크립트를 사용하나요?)</b>
  - No
  - <b><u>Yes</u></b>
- <b>Where does your code run? (코드가 실행되는 환경은 무엇인가요?)</b>
  - Browser
  - <b><u>Node</u></b>
- <b>Would you like to install them now? (지금 패키지를 설치할까요?)</b> - No / <b><u>Yes</u></b>

모든 선택이 끝나면 자동으로 패키지가 설치된 후, 설정 파일이 생성됩니다.

### .eslint.config.mjs 파일 설정하기

위의 초기화 과정이 끝나면 다음과 같은 `eslint.config.mjs` 파일이 생성됩니다.

```javascript
// eslint.config.mjs

import globals from "globals";
import pluginJs from "@eslint/js";
import tseslint from "typescript-eslint";

export default [
  { files: ["**/*.{js,mjs,cjs,ts}"] },
  { languageOptions: { globals: globals.node } },
  pluginJs.configs.recommended,
  ...tseslint.configs.recommended,
];
```

위의 설정은 ESLint 플러그인에서 자체적으로 제공하는 기본 또는 추천 설정을 적용한다는 의미입니다.

위의 설정 파일을 수정하여 프로젝트에서 사용하는 코드 스타일 규칙과 사용자 정의 규칙을 설정할 수 있습니다. ESLint 설정 예시는 다음과 같습니다.

```javascript
// eslint.config.mjs

export default [
  {
    files: ["**/*.js", "**/*.ts", "**/*.tsx"], // ESLint를 적용할 파일 패턴 설정
    languageOptions: {
      ecmaVersion: "latest", // 최신 ECMAScript 버전 사용
      sourceType: "module", // 모듈 사용
    },
    extends: [
      "eslint:recommended", // ESLint 기본 추천 규칙 사용
      "plugin:@typescript-eslint/recommended", // TypeScript 지원 규칙 사용
      "plugin:react/recommended", // React 관련 규칙 사용
    ],
    plugins: ["@typescript-eslint", "react"], // 추가 플러그인
    rules: {
      // 원하는 규칙 설정 (예시)
      "no-unused-vars": "warn", // 사용되지 않는 변수에 대한 경고
      "react/prop-types": "off", // PropTypes 사용 안 할 경우 비활성화
      semi: ["error", "always"], // 세미콜론 필수
      quotes: ["error", "double"], // 큰 따옴표 사용
    },
    ignores: [".dist/*"], // ESLint를 적용하지 않을 파일 및 폴더 지정
  },
];
```

ESLint 설정 시 주로 사용되는 속성들에 대해 설명하자면 다음과 같습니다.

- `files`

  ESLint를 적용할 파일 패턴을 지정하는 부분입니다. 위의 예시에서는 `.js` 파일, `.ts` 파일, `.tsx`에 대해 ESLint를 적용합니다.

- `languageOptions`

  Lint를 위해 파일이 어떤 방식으로 구성되는지에 대한 설정을 지정하는 부분입니다.

  - `ecmaVersion`

    ECMAScript 버전을 지정함으로써 어떤 문법을 사용할 것인지를 설정하는 부분입니다.

  - `sourceType`

    소스 코드가 모듈 형태로 작성되었는지 여부를 지정하는 부분입니다. `module`로 지정하면 ECMAScript modules (ESM)을 사용한다는 의미이고, `commonjs`로 지정하면 CommonJS 파일이라는 의미입니다.

  - `globals`
  - `parser`
  - `parserOptions`

- `extends`

  미리 정의된 규칙을 가져와 프로젝트에 적용하는 부분입니다. 여러 플러그인의 권장 규칙들을 불러올 때 사용됩니다.

- `plugins`

  ESLint에 추가적인 기능과 규칙을 제공하는 플러그인을 사용하는 부분입니다.

- `rules`

  개별 규칙을 정의하거나 덮어쓰는 부분입니다. 특정 규칙을 활성화, 경고, 에러로 설정할 수 있습니다.

- `ignores`

  특정 파일이나 폴더를 ESLint 검사 대상에서 제외하는 부분입니다.

이외에도 ESLint 설정 시 사용할 수 있는 옵션들이 많이 있습니다. 사용할 수 있는 옵션들에 대해서는 다음 링크를 참고하시길 바랍니다.

<a href="https://eslint.org/docs/latest/" target="_blank">Documentation - ESLint - Pluggable JavaScript Linter</a>

## 참고 자료

- <a href="https://prettier.io/" target="_blank">Prettier · Opinionated Code Formatter</a>
- <a href="https://eslint.org/" target="_blank">Find and fix problems in your JavaScript code - ESLint - Pluggable JavaScript Linter</a>

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

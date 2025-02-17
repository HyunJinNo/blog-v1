---
layout: post
title: 챌린지 7일차 학습 정리
description: >
  네이버 부스트캠프 9기 챌린지 7일차 학습 정리 페이지입니다.
image:
  path: /assets/img/naver-boostcamp-9th/naver-boostcamp-9th.jpg
  srcset:
    1060w: /assets/img/naver-boostcamp-9th/naver-boostcamp-9th.jpg
    530w: /assets/img/naver-boostcamp-9th/naver-boostcamp-9th.jpg
    265w: /assets/img/naver-boostcamp-9th/naver-boostcamp-9th.jpg
related_posts:
  - None
sitemap: true
comments: false
---

> - 누군가 작성한 것을 그대로 쓰는 것이 아니라 **나만의 언어로 재구조화하여 작성**해야 합니다.
> - 기술 키워드에 대한 상세 내용도 좋고, 미션 해결 과정에서 기능 구현을 성공한 사례도, 트러블 슈팅 경험도 좋습니다.

<h2> 목차 </h2>

- [학습한 내용](#학습한-내용)
  - [소프트웨어 테스트(Software Test)](#소프트웨어-테스트software-test)
  - [소프트웨어 테스트 유형](#소프트웨어-테스트-유형)
  - [단위 테스트(Unit Test)](#단위-테스트unit-test)
  - [정규 표현식 (Regular Expression)](#정규-표현식-regular-expression)
  - [Jest 사용 방법](#jest-사용-방법)
    - [Step 1 - Jest 설치하기](#step-1---jest-설치하기)
    - [Step 2 - package.json 스크립트 설정하기](#step-2---packagejson-스크립트-설정하기)
    - [Step 3 - 테스트 코드 작성하기](#step-3---테스트-코드-작성하기)
    - [Step 4 - 테스트 실행하기](#step-4---테스트-실행하기)
- [Comments](#comments)

## 학습한 내용

### 소프트웨어 테스트(Software Test)

소프트웨어 테스트란 소프트웨어 제품이나 애플리케이션이 올바르게 작동하고 있는지 평가하고 검증하는 프로세스입니다. 즉, 테스트는 의도한대로 동작하는 지 검증하는 과정을 말합니다.

소프트웨어 테스트의 장점은 다음과 같습니다.

- 소프트웨어의 버그를 방지하고 성능을 개선할 수 있습니다.
- 애플리케이션 품질을 검증하여 사용자 요구사항을 충족하는지 확인할 수 있습니다.
- 소프트웨어 테스트를 통해 다음과 같은 문제를 더 신속하게 해결할 수 있습니다.
  - `아키텍처 결함`
  - `잘못된 설계 결정`
  - `유효하지 않거나 잘못된 기능`
  - `보안 취약점`
  - `확장성 문제`

### 소프트웨어 테스트 유형

소프트웨어 테스트에는 다양한 유형이 있으며, 각 테스트는 특정한 목표와 전략을 갖고 있습니다. 테스트 유형에 대한 설명은 다음과 같습니다.

- `코드 검토`: 새 소프트웨어와 수정된 소프트웨어가 조직의 코딩 표준을 따르고 모범 사례를 준수하는지 확인합니다.
- **`통합 테스트(Integration Test)`: 소프트웨어 구성 요소 또는 기능이 함께 작동하는지 확인합니다. 즉, 단위 기능이 합쳐진 기능에 대한 테스트를 말합니다.**
- **`단위 테스트(Unit Test)`: 각 소프트웨어 단위가 예상대로 실행되는지 확인합니다. 단위는 애플리케이션에서 테스트할 수 있는 가장 작은 구성 요소입니다. 단위의 경우 보통 함수를 가리킵니다.**
- `기능 테스트`: 기능 요구 사항을 기반으로 비즈니스 시나리오를 에뮬레이션하여 기능을 확인합니다. `블랙박스 테스트`는 기능을 검증하는 일반적인 방법입니다.
- `성능 테스트`: 다양한 워크로드에서 소프트웨어가 어떻게 실행되는지 테스트합니다. 예를 들어, `부하 테스트`는 실제 부하 조건에서 성능을 평가하는 데 사용됩니다.
- `회귀 테스트`: 새로운 기능이 중단되거나 기능이 저하되는지 확인합니다. 전체 회귀 테스트를 수행할 시간이 없는 경우 온전성 테스트를 사용해 메뉴, 기능 및 명령을 표면 수준에서 검증할 수 있습니다.
- `보안 테스트`: 소프트웨어가 해커나 기타 악의적인 유형의 취약점에 노출되어 있지 않은지 확인하여 서비스에 대한 액세스를 거부하거나 잘못된 성능을 유발하는 데 악용될 수 있는 취약점이 없는지 확인합니다.
- `스트레스 테스트`: 시스템이 장애를 일으키기 전까지 어느 정도의 부담을 견딜 수 있는지 테스트합니다. 스트레스 테스트는 `비기능 테스트`의 일종으로 간주됩니다.
- `사용성 테스트`: 고객이 작업을 완료하기 위해 시스템 또는 웹 애플리케이션을 얼마나 잘 사용할 수 있는지 확인합니다.
- **`시스템 테스트(System test)`: 통합 테스트보다 더 큰 개념으로 전체 시스템에 대한 동작 테스트를 수행합니다.**
- **`인수 테스트(Acceptance Test)`: 고객이 ok할 수 있는지 판단하기 위한 테스트를 말합니다.**

위와 별개로 아래 두 가지 테스트가 존재합니다.

- `UI Test`: FE나 모바일 분야에서 UI 기능 단위로 진행하는 테스트입니다. 보통 Unit test와 System test 사이라고 볼 수 있습니다.
- `E2E test`: End-to-end 테스트. 이 역시 UI 테스트와 같이 말하는 경우도 있고, 전체 시스템 관점에서의 테스트로 보는 경우도 있습니다.

### 단위 테스트(Unit Test)

단위 테스트란 프로그래밍의 최소 단위를 테스트하는 것으로, 보통 함수를 뜻합니다.

단위 테스트의 특징은 다음과 같습니다.

- 빠르게 수행하고(quickly)
- 격리된 방식으로(isolation)
- 작은 코드 조각(단위)을 검증하고(validate)
- 자동화된 (automatic) 진짜 코드

### 정규 표현식 (Regular Expression)

정규 표현식에 대해서는 다음 링크에 작성하였습니다.

[정규 표현식 (Regular Expression)](../../cs/2024-08-09-regular-expression)

### Jest 사용 방법

#### Step 1 - Jest 설치하기

먼저 다음 명령어를 입력하여 Jest를 설치합니다.

```
npm install --save-dev jest
```

#### Step 2 - package.json 스크립트 설정하기

<h4> script란? </h4>

`package.json` 파일의 scripts 항목을 통해 다양한 명령어를 설정할 수 있습니다. 해당 항목을 통해 빌드, 실행 등에 사용되는 명령어를 설정합니다.

<h4> scripts 설정 </h4>

`package.json` 파일의 scripts 항목에 다음 코드를 입력합니다.

```json
"scripts": {
  "test": "jest"
},
```

위에서 설정한 명령어에 대해 설명하면 다음과 같습니다.

- `test`: 테스트 코드를 실행하는 명령어입니다. 터미널에서 `npm test` 또는 `npm run test`를 입력하면 `jest` 명령어를 실행할 수 있습니다.

#### Step 3 - 테스트 코드 작성하기

다음과 같은 함수가 있다고 가정합니다.

```javascript
// calculator.js

function sum(a, b) {
  return a + b;
}

function average(a, b) {
  return (a + b) / 2;
}

module.exports = {
  sum,
  average,
};
```

테스트 코드를 작성하려면 `*.test.js` 또는 `*.spec.js` 파일을 생성해야 합니다. 위의 `calculator.js` 파일을 테스트하려면 `calculator.test.js` 또는 `calculator.spec.js` 테스트 파일을 생성해야 합니다.

```javascript
// calculator.test.js

const { sum, average } = require("./calculator");

describe("Calculator 테스트", () => {
  let a = 4;
  let b = 6;

  beforeEach(() => {
    a = 4;
    b = 6;
  });

  test("sum 테스트", () => {
    expect(sum(a + b)).toBe(10);
  });

  test("average 테스트", () => {
    expect(average(a + b)).toBe(5);
  });
});
```

주요 Jest 기능에 대해 설명하면 다음과 같습니다.

- `describe`: 테스트를 그룹화하기 위해 사용합니다.
- **Matchers**: Jest는 다양한 매처(matchers)를 제공합니다.
  - `toBe`
  - `toEqual`
  - `toContain`
  - `toHaveLength`
  - `toStrictEqual`
  - 이외의 matchers는 다음 링크를 참고하시길 바랍니다.
    <a href="https://jestjs.io/docs/using-matchers" target="_blank">Using Matchers</a>

#### Step 4 - 테스트 실행하기

다음 명령어를 입력하여 테스트를 실행합니다.

```
npm test
```

또는

```
npm run test
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

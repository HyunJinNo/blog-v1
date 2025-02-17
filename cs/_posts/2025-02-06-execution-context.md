---
layout: post
title: 실행 컨텍스트 (Execution Context)
description: >
  실행 컨텍스트(Execution Context)에 대해 설명하는 페이지입니다.
image:
  path: /assets/img/cs/study.jpg
related_posts:
  - None
sitemap: true
comments: false
---

<i>last modified at: 2025. 02. 06.</i>

<h2>목차</h2>

- [개요](#개요)
- [실행 컨텍스트란?](#실행-컨텍스트란)
- [실행 컨텍스트 종류](#실행-컨텍스트-종류)
- [실행 컨텍스트와 콜 스택](#실행-컨텍스트와-콜-스택)
- [실행 컨텍스트의 구성 요소](#실행-컨텍스트의-구성-요소)
- [실행 컨텍스트의 생성 및 실행 과정](#실행-컨텍스트의-생성-및-실행-과정)
  - [생성 단계 (Creation Phase)](#생성-단계-creation-phase)
    - [VariableEnvironment와 LexicalEnvironment 생성](#variableenvironment와-lexicalenvironment-생성)
    - [this 바인딩 결정](#this-바인딩-결정)
    - [outerEnvironmentReference 설정](#outerenvironmentreference-설정)
  - [실행 단계 (Execution Phase)](#실행-단계-execution-phase)
- [참고 자료](#참고-자료)
- [Comments](#comments)

## 개요

자바스크립트의 실행 컨텍스트에 대해 정리한 페이지입니다.

## 실행 컨텍스트란?

`실행 컨텍스트(Execution Context)`란 실행할 코드에 제공한 환경 정보들을 모아놓은 객체로, 코드가 실제로 실행될 때 그 코드의 변수, 함수, 스코프, this 바인딩 등 실행에 필요한 모든 정보를 관리하는 내부 메커니즘입니다. 자바스크립트는 어떤 실행 컨텍스트가 활성화되는 시점에 선언된 변수를 위로 끌어올리고(=호이스팅, Hoisting), 외부 환경 정보를 구성하고, this 값을 설정하는 등의 동작을 수행합니다. 동일한 환경에 있는 코드들을 실행할 때 필요한 환경 정보들을 모아 컨텍스트를 구성하고, 이를 콜스택(Call Stack)에 쌓아올렸다가, 가장 위에 쌓여있는 컨텍스트와 관련 있는 코드들을 실행하는 식으로 전체 코드의 환경과 순서를 보장합니다.

## 실행 컨텍스트 종류

실행 컨텍스트를 구성할 수 있는 방법으로 `전역 공간`, `함수`, `eval 함수`가 있습니다. 각 실행 컨텍스트에 대해 정리하자면 다음과 같습니다.

- `전역 실행 컨텍스트(Global Execution Context)`

  페이지나 스크립트가 로드될 때 자동으로 생성되며, 브라우저에서는 `window` 객체와 연결되고 Node.js에서는 `global` 객체와 연결됩니다. 전역 실행 컨텍스트는 변수 객체를 생성하는 대신 전역 객체를 활용하며, 전역 객체에는 브라우저의 `window` 객체, Node.js의 `global` 객체 등이 있습니다. 이들은 자바스크립트 `내장 객체(Native Object)`가 아닌 `호스트 객체(Host Object)`로 분류됩니다.

- `함수 실행 컨텍스트(Function Execution COntext)`

  함수가 호출될 때마다 생성되며, 각 함수는 자신만의 컨텍스트를 가집니다.

- `Eval 실행 컨텍스트`

  `eval()` 함수로 실행되는 코드에 대해 생성되지만, 보안과 성능 문제로 거의 사용되지 않습니다.

## 실행 컨텍스트와 콜 스택

실행 컨텍스트가 콜 스택에 어떤 순서로 쌓이는지 다음 자바스크립트 코드를 통해 확인해보도록 하겠습니다.

```javascript
// 자바스크립트 코드를 실행하는 순간 전역 실행 컨텍스트가 콜 스택에 담깁니다.
console.log(a); // undefined

var a = 1;

function outer() {
  function inner() {
    console.log(a); // undefined
    var a = 3;
  }

  inner();
  console.log(a); // 1
}

outer();
console.log(a); // 1
```

<img src="/assets/img/cs/execution-context/pic1.png" alt="pic1" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

먼저 위의 사진과 같이 처음 자바스크립트 코드가 실행되는 순간 자동으로 전역 실행 컨텍스트가 콜 스택에 담기게 됩니다.

<br />

<img src="/assets/img/cs/execution-context/pic2.png" alt="pic2" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

이후 outer 함수가 호출되면 자바스크립트 엔진은 outer에 대한 환경 정보를 수집해서 outer 실행 컨텍스트를 생성한 후 콜 스택에 담습니다. 콜 스택 맨 위에 outer 실행 컨텍스트가 놓인 상태이므로 전역 실행 컨텍스트와 관련된 코드의 실행을 일시 중단하고 outer 실행 컨텍스트와 관련된 코드, 즉 outer 함수 내부의 코드를 순서대로 실행합니다.

<br />

<img src="/assets/img/cs/execution-context/pic3.png" alt="pic3" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

outer 함수 내부에서 inner 함수가 호출되면 inner 실행 컨텍스트를 생성한 후 콜 스택에 담습니다. outer 실행 컨텍스트와 관련된 코드의 실행을 일시 중단하고 inner 함수 내부의 코드를 순서대로 실행합니다.

<br />

<img src="/assets/img/cs/execution-context/pic4.png" alt="pic4" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

이후 inner 함수의 실행이 종료되면 콜 스택에서 inner 실행 컨텍스트를 제거합니다. 콜 스택에서 inner 실행 컨텍스트를 제거하여 콜 스택 맨 위에 outer 실행 컨텍스트가 놓인 상태이므로 outer 함수 내부에서 inner 함수를 호출하는 코드 이후부터 실행합니다.

<br />

<img src="/assets/img/cs/execution-context/pic5.png" alt="pic5" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

outer 함수의 실행이 종료되면 콜 스택에서 outer 실행 컨텍스트를 제거합니다. 콜 스택에서 outer 실행 컨텍스트를 제거하여 콜 스택에는 전역 실행 컨텍스트만 남게 됩니다.

<br />

<img src="/assets/img/cs/execution-context/pic6.png" alt="pic6" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

마지막으로 자바스크립트 코드 실행이 종료되면 전역 공간에는 실행할 코드가 남아 있지 않으므로 콜 스택에서 전역 실행 컨텍스트를 제거하여 콜 스택에는 아무 것도 없는 상태로 종료됩니다.

## 실행 컨텍스트의 구성 요소

어떤 실행 컨텍스트가 생성될 때 자바스크립트 엔진은 해당 실행 컨텍스트에 관련된 코드들을 실행하는 데 필요한 환경 정보들을 수집해서 실행 컨텍스트 객체에 저장합니다. 실행 컨텍스트의 구성 요소들은 다음과 같습니다.

- `VariableEnvironment`

  현재 실행 컨텍스트 내의 식별자(변수)와 함수 선언 등 <b>초기 상태</b> 정보를 저장합니다. 선언 시점의 `LexicalEnvironment`의 스냅샷(Snapshot)을 저장하며, 변경 사항은 반영하지 않습니다. 실행 컨텍스트를 생성할 때 `VariableEnvironment`에 정보를 먼저 담은 다음, 이를 그대로 복사해서 `LexicalEnvironment`를 만듭니다.

  `VariableEnvironment` 내부는 `environmentRecord(snapshot)`와 `outerEnvironmentReference(snapshot)`으로 구성됩니다. `VariableEnvironment`는 스냅샷 유지 목적으로 사용되고, 주로 아래의 `LexicalEnvironment`를 활용합니다.

  - `environmentRecord(snapshot)`
  - `outerEnvironmentReference(snapshot)`

- `LexicalEnvironment`

  처음에는 `VariableEnvironment`와 동일하지만 변경 사항이 실시간으로 반영됩니다. `LexicalEnviroment`는 다음 두 부분으로 구성됩니다.

  - `environmentRecord`

    `environmentRecord`에는 현재 실행 컨텍스트와 관련된 코드의 식별자 정보들이 저장됩니다. 여기서 식별자란, 컨텍스트를 구성하는 함수에 지정된 매개변수 식별자, 선언한 함수가 있는 경우 그 함수 자체, var로 선언된 변수의 식별자 등을 의미합니다. 실행 컨텍스트 내부 전체를 처음부터 끝까지 확인하면서 순서대로 수집합니다.

    코드 실행 전에 자바스크립트 엔진이 먼저 모든 식별자 정보를 수집하는 [호이스팅 (Hoisting)](../2025-02-05-hoisting) 현상이 바로 이 과정에서 발생합니다.

  - `outerEnvironmentReference`

    `스코프(Scope)`란 <b>식별자에 대한 유효범위</b>를 의미합니다. 스코프의 개념은 대부분의 언어에서 존재하지만, ES5까지의 자바스크립트는 특이하게도 오직 함수에 의해서만 스코프가 생성됩니다.

    `outerEnvironmentReference`는 현재 실행 컨텍스트가 속한 상위 스코프(LexicalEnvironment)를 참조하는 포인터로, `스코프 체인(Scope Chain)`을 형성해 상위 스코프를 참조합니다. `outerEnvironmentReference`는 <b>현재 호출된 함수가 선언될 당시의 LexicalEnvironment를 참조</b>합니다. 만약 현재 실행 컨텍스트 내에서 식별자를 찾지 못하면 이 참조를 따라 상위 컨텍스트에서 변수를 검색하게 됩니다. 또한 여러 스코프에서 동일한 식별자를 선언한 경우, 무조건 스코프 체인 상에서 가장 먼저 발견된 식별자에만 접근 가능하게 됩니다.

- `ThisBinding`

  실행 컨텍스트가 생성될 때, 해당 컨텍스트 내에서 사용될 this 식별자가 가리키는 대상 객체가 결정됩니다. 실행 컨텍스트 활성화 당시에 this가 지정되지 않은 경우 this에는 전역 객체가 저장됩니다.

## 실행 컨텍스트의 생성 및 실행 과정

실행 컨텍스트는 크게 `생성 단계(Creation Phase)`와 `실행 단계(Execution Phase)`로 구분됩니다.

### 생성 단계 (Creation Phase)

#### VariableEnvironment와 LexicalEnvironment 생성

먼저 코드가 실행되기 전에 자바스크립트 엔진은 현재 실행 컨텍스트에 포함된 모든 식별자(변수, 함수, 매개변수 등)를 `EnvironmentRecord`에 등록합니다. 이 때 `var`로 선언된 변수는 선언과 동시에 초기값(undefined)으로 설정되며, 함수 선언문은 함수 전체(선언과 정의)가 등록되어 호이스팅됩니다.

#### this 바인딩 결정

실행 컨텍스트의 종류와 함수 호출 방식에 따라 this가 결정됩니다.

#### outerEnvironmentReference 설정

현재 실행 컨텍스트가 속한 상위 스코프의 `LexicalEnvironment`가 할당되어 스코프 체인이 형성됩니다.

### 실행 단계 (Execution Phase)

코드의 각 줄이 실제로 실행되면서 변수에 값이 할당되고 함수가 호출되며 실제 연산이 진행됩니다.

## 참고 자료

- <a href="https://velog.io/@ghost1434/%EC%8B%A4%ED%96%89-%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8" target="_blank">실행 컨텍스트</a>
- <a href="https://www.yes24.com/Product/Goods/78586788" target="_blank">코어 자바스크립트 - 예스 24</a>
- <a href="https://gamguma.dev/post/2022/04/js_execution_context" target="_blank">JS Execution Context (실행 컨텍스트) 란?</a>
- <a href="https://junilhwang.github.io/TIL/Javascript/Domain/Execution-Context/" target="_blank">자바스크립트 실행 컨텍스트</a>

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

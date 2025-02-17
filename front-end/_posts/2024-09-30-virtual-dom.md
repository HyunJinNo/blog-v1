---
layout: post
title: 가상 돔(Virtual DOM)과 React 렌더링
description: >
  가상 돔(Virtual DOM)과 React 렌더링에 대해 정리한 페이지입니다.
image:
  path: /assets/img/front-end/front-end.jpg
related_posts:
  - None
sitemap: true
comments: false
---

<i>last modified at: 2025. 02. 13.</i>

<h2>목차</h2>

- [개요](#개요)
- [가상 돔(Virtual DOM)이란?](#가상-돔virtual-dom이란)
- [가상 돔(Virtual DOM)을 사용하는 이유](#가상-돔virtual-dom을-사용하는-이유)
- [가상 돔(Virtual DOM)과 React 렌더링 과정](#가상-돔virtual-dom과-react-렌더링-과정)
  - [Render Phase \& Commit Phase](#render-phase--commit-phase)
  - [createElement 함수와 렌더링](#createelement-함수와-렌더링)
  - [재조정(Reconciliation)](#재조정reconciliation)
    - [Diff 알고리즘 적용](#diff-알고리즘-적용)
    - [실제 DOM 업데이트](#실제-dom-업데이트)
    - [기존 가상 돔을 새로운 가상 돔으로 교체](#기존-가상-돔을-새로운-가상-돔으로-교체)
- [참고 자료](#참고-자료)
- [Comments](#comments)

## 개요

<hr />

가상 돔(Virtual DOM)의 개념과 리액트(React)에서 가상 돔을 활용하여 렌더링하는 방식에 대해 정리한 페이지입니다.

## 가상 돔(Virtual DOM)이란?

<hr />

`가상 돔(Virtual DOM, Virtual Document Object Model)`이란 메모리에 유지하는 실제 DOM의 가상 표현입니다. React나 Vue.js 등에서 사용하는 개념으로, 실제 DOM과 유사한 가벼운 객체를 메모리 상에 유지하면서 UI 업데이트 성능을 최적화하는 데 도움을 줍니다. UI가 변경되는 경우 실제 DOM에 바로 반영되지 않고 가상 돔에 먼저 반영된 후, 기존 가상 돔과 새로운 가상 돔을 비교하여 차이점을 찾아 실제 DOM을 업데이트하게 됩니다.

## 가상 돔(Virtual DOM)을 사용하는 이유

<hr />

가상 돔을 사용하는 이유는, 실제 DOM을 최소한으로 조작하기 위해서입니다. 실제 DOM 조작은 브라우저의 렌더링 엔진에서 많은 리소스를 소비하게 됩니다. 특히, 페이지 전체를 새로 렌더링하는 경우 많은 시간이 소요될 수 있습니다. 또한 사용자 입력이나 애니메이션으로 인해 DOM을 자주 변경해야 하는 경우, DOM을 직접적으로 다루는 것은 많은 리소스를 소모할 수 있습니다.

이러한 성능 문제를 해결하기 위해 가상 돔이 사용됩니다. UI 변경 사항을 먼저 가상 돔에 반영한 후, 기존 가상 돔과 새로운 가상 돔을 비교하여 변경 사항을 모은 후 실제 DOM에 반영함으로써 불필요한 DOM 조작을 줄이고 성능을 최적화할 수 있습니다.

## 가상 돔(Virtual DOM)과 React 렌더링 과정

<hr />

`리액트(React)`에서 렌더링이란 컴포넌트가 현재 state와 props를 기반으로 UI를 생성하고 업데이트하는 과정을 의미합니다. 리액트에서는 효율적인 UI 업데이트를 위해 `가상 돔(Virtual DOM)`과 `재조정(Reconciliation)` 과정을 활용합니다.
`리액트(React)`의 렌더링 과정은 다음과 같습니다.

### Render Phase & Commit Phase

리액트 렌더링은 크게 다음 2개의 단계로 이루어집니다.

- `Render Phase`: 컴포넌트를 렌더링하고 변경 사항을 계산하는 모든 작업
- `Commit Phase`: DOM에 변경 사항을 적용하는 과정

리액트는 먼저 `Render 단계`에서 JSX를 `React Elements`로 변경하고 Diffing 연산을 수행합니다. 이후 `Commit 단계`에서는 실제 DOM에 변경 사항을 적용합니다. 만약 변경 사항이 존재하지 않는다면 `Commit`할 사항이 없을 수 있습니다. 즉, 리액트에서 컴포넌트가 렌더링된다고 해서 실제 DOM 조작이 무조건 발생하는 것은 아닙니다.

### createElement 함수와 렌더링

먼저 JSX로 작성된 리액트 컴포넌트가 브라우저에 렌더링될 때, JS 파일로 컴파일되고 런타임 시점에 `React.createElement` 함수 호출로 변환됩니다.

`createElement` 함수는 먼저 노드 타입을 확인하여, `div`, `span` 같은 문자열은 기본 HTML 요소로, 함수나 클래스는 사용자 정의 컴포넌트로 인식합니다. 이후 전달된 속성(Props)들을 파싱하여 노드에 필요한 속성을 설정합니다. 이후 자식 요소들을 재귀적으로 탐색하고 처리하여 노드 트리를 구성함으로써 주어진 태그, 속성, 자식 요소들을 조합하여 새로운 가상 돔과 실제 DOM 노드를 생성합니다.

### 재조정(Reconciliation)

리액트에서 `재조정(Reconciliation)`이란 상태(State)나 props의 변경으로 인해 UI가 변경될 때, 새로운 가상 돔과 기존 가상 돔을 비교하여 최소한의 변경만 실제 DOM에 적용하는 과정을 의미합니다. 이 과정을 통해 불필요한 DOM 업데이트가 최소화되고 애플리케이션의 성능이 크게 향상됩니다.

재조정은 주로 다음 단계를 통해 이루어집니다.

#### Diff 알고리즘 적용

상태(State)나 props가 변경되는 경우, 리액트는 변경된 상태를 반영하는 트리 구조를 갖는 새로운 가상 돔을 생성합니다. 이 새로운 가상 돔과 기존 가상 돔을 비교하여 변경 사항을 찾습니다. 이 때 사용하는 알고리즘이 바로 `Diff 알고리즘`입니다.

`Diff 알고리즘(또는 Diffing 알고리즘)`이란 <b>새로운 가상 돔과 기존 가상 돔을 비교하여 변경 사항을 찾는 알고리즘</b>입니다. 해당 알고리즘은 크게 다음 3가지 단계로 나뉩니다.

1. `노드 타입 비교`

   노드의 타입이 다르면, 해당 노드를 완전히 교체합니다. 즉, 두 요소가 서로 다른 타입의 컴포넌트이거나 DOM 요소일 경우, 리액트는 해당 요소와 그 하위 트리를 완전히 제거하고 새로 생성합니다. 예를 들어 `<div>`에서 `<span>`으로 변경되는 경우, 리액트는 `<div>`와 그 하위 요소를 삭제하고 새로운 `<span>`을 추가합니다.

2. `속성(Props) 비교`

   노드의 타입이 동일한 경우, 리액트는 그 노드의 속성을 비교하여 달라진 부분만 수정합니다. 예를 들어, `<div>` 요소의 `className` 속성만 변경되었다면, 전체 요소를 다시 생성하지 않고 해당 속성만 업데이트합니다.

3. `자식 노드 비교`

   각 노드의 자식 요소들을 순서대로, 그리고 재귀적으로 비교하여 차이점을 찾고 업데이트합니다. 이 때 리스트가 있는 경우 `key`를 통해 최적화된 비교를 수행합니다.

   리스트가 렌더링될 때 `key`는 재조정 과정에서 매우 중요한 역할을 수행합니다. `key`는 리액트가 리스트 항목의 고유성을 추적하고, 어떤 항목이 추가, 삭제, 또는 이동되었는지 쉽게 파악할 수 있도록 도와줍니다. `key`가 없거나 잘못 설정되었을 때, 성능 저하나 잘못된 업데이트가 발생할 수 있습니다. 예를 들어, `key`가 없을 때 리스트의 순서가 바뀌는 경우 모든 항목이 다시 렌더링되지만, `key`가 있는 경우 바뀐 항목만 업데이트됩니다.

#### 실제 DOM 업데이트

리액트에서는 변경 사항을 감지하면 즉시 실제 DOM에 업데이트하는 것이 아니라, 변경 사항을 모아 두었다가 한 번에 실제 DOM에 반영합니다. 이를 `배치 처리`한다고 표현합니다. 이와 같은 배치 처리를 통해 변경 사항을 모아서 한 번에 처리하면 DOM 조작을 최소화할 수 있어서 성능이 향상됩니다. 또한 `리플로우(Reflow)`와 `리페인트(Repaint)`를 줄여서 브라우저 성능에 유리한 측면이 있습니다.

#### 기존 가상 돔을 새로운 가상 돔으로 교체

실제 DOM이 업데이트된 후, 기존 가상 돔을 새로운 가상 돔으로 교체합니다. 이후 다시 재조정 과정이 발생하는 경우, 리액트는 이 새로운 가상 돔을 기준으로, 또 다른 새로운 가상 돔을 생성하고 비교하는 과정을 반복하게 됩니다.

## 참고 자료

<hr />

- <a href="https://ko.legacy.reactjs.org/docs/reconciliation.html" target="_blank">재조정 (Reconciliation) – React</a>
- <a href="https://yceffort.kr/2022/04/deep-dive-in-react-rendering" target="_blank">리액트의 렌더링은 어떻게 일어나는가?</a>
- <a href="https://hyunjinlee.com/blog/%EB%A6%AC%EC%95%A1%ED%8A%B8%20%EB%A0%8C%EB%8D%94%EB%A7%81%EC%97%90%20%EB%8C%80%ED%95%9C%20%EC%9D%B4%ED%95%B4" target="_blank">리액트 렌더링에 대한 이해</a>

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

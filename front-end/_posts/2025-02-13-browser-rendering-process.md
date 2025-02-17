---
layout: post
title: 브라우저 렌더링 과정
description: >
  브라우저 렌더링 과정에 대해 정리한 페이지입니다.
image:
  path: /assets/img/front-end/front-end.jpg
related_posts:
  - None
sitemap: true
comments: false
---

<i>last modified at: 2025. 02. 16.</i>

<h2>목차</h2>

- [개요](#개요)
- [브라우저 렌더링 과정](#브라우저-렌더링-과정)
  - [Step 1 - 네트워크 요청 및 리소스 로드](#step-1---네트워크-요청-및-리소스-로드)
  - [Step 2 - HTML 파싱 및 DOM 트리 생성](#step-2---html-파싱-및-dom-트리-생성)
  - [Step 3 - CSS 파싱 및 CSSOM 트리 생성](#step-3---css-파싱-및-cssom-트리-생성)
  - [Step 4 - 렌더 트리 생성](#step-4---렌더-트리-생성)
  - [Step 5 - 레이아웃/리플로우(Layout/Reflow)](#step-5---레이아웃리플로우layoutreflow)
  - [Step 6 - 페인팅(Painting)](#step-6---페인팅painting)
  - [Step 7 - 합성(Composite)](#step-7---합성composite)
  - [Step 8 - 리렌더링](#step-8---리렌더링)
- [참고 자료](#참고-자료)
- [Comments](#comments)

## 개요

<hr />

브라우저 렌더링 과정에 대해 정리한 페이지입니다.

## 브라우저 렌더링 과정

<hr />

`브라우저 렌더링(Browser Rendring)`은 HTML, CSS, JavaScript 등의 웹 페이지 자원을 화면에 그리는 과정을 말합니다. 그리고 `브라우저 렌더링 과정(Browser Rendering Process)`은 웹 페이지를 사용자에게 보여주기 위해 거치는 일련의 과정으로 여러 단계로 구성됩니다. 브라우저 렌더링 과정은 <b>네트워크 요청 → HTML/CSS 파싱 → DOM/CSSOM 생성 → 렌더 트리 생성 → 레이아웃 계산 → 페인팅 → 합성</b>의 순서로 진행됩니다. 브라우저 렌더링 과정을 단계별로 설명하자면 다음과 같습니다.

### Step 1 - 네트워크 요청 및 리소스 로드

먼저 사용자가 주소창에 URL을 입력하면 브라우저는 URL을 IP 주소로 변환하기 위해 `DNS(Domain Name System)` 서버에 쿼리를 보냅니다. DNS 서버는 도메인 이름을 IP 주소로 변환하여 브라우저에 반환하고, 브라우저는 해당 IP 주소를 통해 서버에 HTTP/HTTPS 요청을 보냅니다. 요청을 받은 서버는 HTML 문서와 함께 CSS, JavaScript, 이미지 등 웹 페이지를 구성하는 모든 리소스를 클라이언트로 전달합니다.

<img src="/assets/img/front-end/browser-rendering-process/pic1.png" alt="pic1" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

<img src="/assets/img/front-end/browser-rendering-process/pic2.png" alt="pic2" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

### Step 2 - HTML 파싱 및 DOM 트리 생성

브라우저는 다운로드한 HTML 문서를 파싱하여 `DOM 트리`를 생성합니다. 이때, HTML 태그를 노드로 변환하고, 노드 간의 계층 관계를 형성합니다. 이 과정은 HTML 문서의 위에서 아래 방향으로 이루어집니다.

만약 HTML 파싱 중에 `async`나 `defer` 같은 설정이 되어있지 않은 `<script>` 태그를 만나는 경우 HTML 파싱을 일시 중단하고 자바스크립트를 실행하게 됩니다. 또한 외부 CSS 파일은 만나는 경우 `CSSOM` 생성 전까지 렌더링이 지연됩니다.

> DOM은 **Document Object Model**으로 XML이나 HTML 문서에 접근하기 위한 인터페이스입니다. DOM은 자바스크립트로 접근하여 수정될 수 있습니다.  
> `defer`: defer 속성을 사용하면 HTML 파싱이 완료된 후 자바스크립트를 실행합니다.  
> `async`: async 속성을 사용하면 자바스크립트를 비동기적으로 로드하여 실행합니다.

### Step 3 - CSS 파싱 및 CSSOM 트리 생성

HTML 파싱과 `DOM 트리` 생성이 완료되면, 브라우저는 `CSSOM(CSS Object Model) 트리`를 생성합니다. 브라우저는 다운로드한 CSS 파일을 파싱하여 `CSSOM 트리`를 생성합니다. 이때, CSS 속성을 노드로 변환하고, 노드 간의 계층 관계를 형성합니다. 이 과정에서 브라우저는 CSS의 cascading 규칙에 따라 최종 스타일을 결정합니다.

> <b>CSSOM(CSS Object Model)</b>은 CSS 객체 모델로 자바스크립트가 CSS를 동적으로 조작할 수 있게 해줍니다. HTML 대신 CSS가 대상인 DOM이라고 할 수 있습니다.

### Step 4 - 렌더 트리 생성

`DOM 트리`와 `CSSOM 트리`를 결합하여 `렌더 트리`를 생성합니다. 이때, 실제 화면에 표시될 요소만을 선택하여 `렌더 트리`를 형성합니다. 즉, `display: none` 속성이 적용된 요소는 `렌더 트리`에 포함되지 않습니다. 반면에 `visibility: hidden` 속성이 적용된 요소는 공간을 차지합니다.

### Step 5 - 레이아웃/리플로우(Layout/Reflow)

`렌더 트리`가 생성되면 각 노드의 정확한 위치와 크기를 계산하는 레이아웃(또는 리플로우) 단계가 시작됩니다. 이 과정에서 `상대적 단위(%, em, rem 등)`를 실제 px 단위로 변환하며, 요소의 박스 모델(margin, padding, border 등)을 고려하여 최종 배치가 결정됩니다.

### Step 6 - 페인팅(Painting)

계산된 레이아웃 정보를 바탕으로 브라우저는 각 요소를 px 단위로 변환하며, 실제 화면에 그리는 페인팅 단계를 실행합니다. 이 때, 각 요소의 배경, 테두리, 글자 등을 그리게 됩니다. 이 과정에서 브라우저는 성능 최적화를 위해 요소를 독립적인 레이어(Layer)로 분리하여 렌더링합니다. 이는 성능 저하를 초래할 수 있는 `레이아웃 스래싱(Layout Thrashing)`을 방지하기 위함입니다.

> `레이아웃 스레싱(Layout Thrashing)`이란 웹 페이지에서 JavaScript가 <b>반복적으로 DOM을 읽고 쓰는 작업을 수행</b>할 때, 브라우저가 불필요하게 <b>레이아웃 계산을 여러 번 강제로 실행</b>하며 성능이 급격히 저하되는 현상을 말합니다.

### Step 7 - 합성(Composite)

이 단계에서는 여러 레이어(Layer)를 합성하여 최종 화면을 출력합니다. 이 때, GPU와 합성 스레드가 협력하여, 스크롤이나 애니메이션 시 부드러운 업데이트를 가능하게 합니다. `transform`, `opacity` 속성은 GPU 가속을 지원하며 별도 레이어에서 처리되어 리페인트 없이 애니메이션이 가능합니다.

### Step 8 - 리렌더링

리렌더링이 발생하면 `리플로우(Reflow)`와 `리페인트(Repaint)`가 발생합니다.

- `리플로우(Reflow)`

  DOM이나 CSSOM에 변경이 발생하면, 영향을 받는 부분에 대해 다시 레이아웃 계산이 필요합니다. 이 경우, 변경된 부분에 대해 다시 렌더 트리를 구성하고 레이아웃 및 페인팅 단계, 합성도 다시 수행됩니다. 예를 들어, 요소의 크기, 위치, margin 등이 변경될 때 리플로우가 발생하며, 이 경우 페인팅과 합성도 다시 수행됩니다.

- `리페인트(Repaint)`

  레이아웃 변화 없이 색상, 배경 등 스타일만 변경되면 리페인트만 일어나며, 레이아웃 재계산없이 다시 화면을 그립니다.

## 참고 자료

<hr />

- <a href="https://developer.mozilla.org/ko/docs/Web/Performance/How_browsers_work" target="_blank">웹페이지를 표시한다는 것: 브라우저는 어떻게 동작하는가 - 웹 성능 | MDN</a>
- <a href="https://f-lab.kr/insight/understanding-browser-rendering-process-20240711" target="_blank">브라우저 렌더링 과정 이해하기</a>
- <a href="https://jyostudy.tistory.com/207" target="_blank">주소창에 www.naver.com을 쳤을 때 일어나는 일</a>

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

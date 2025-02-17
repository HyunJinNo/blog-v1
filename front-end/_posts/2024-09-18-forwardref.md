---
layout: post
title: forwardRef 사용 방법
description: >
  forwardRef 사용 방법에 대해 설명하는 페이지입니다.
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

<h2> 목차 </h2>

- [개요](#개요)
- [ref란?](#ref란)
  - [개념](#개념)
  - [ref를 사용하는 경우](#ref를-사용하는-경우)
  - [ref 사용 시 주의사항](#ref-사용-시-주의사항)
- [forwardRef란?](#forwardref란)
- [Step 1 - 자식 컴포넌트](#step-1---자식-컴포넌트)
- [Step 2 - displayName 설정하기](#step-2---displayname-설정하기)
- [Step 3 - 부모 컴포넌트](#step-3---부모-컴포넌트)
- [Comments](#comments)

## 개요

이번 글에서는 React에서 컴포넌트에 `ref`를 전달할 때 사용하는 고차 함수인 `forwardRef` 사용 방법에 대해 설명하겠습니다.

## ref란?

### 개념

`ref`는 React에서 특정 DOM 요소나 컴포넌트에 대해 직접 접근해야 할 때 사용합니다. 일반적으로 React에서는 컴포넌트의 상태나 props를 통해 UI를 업데이트하지만, 가끔 직접 접근해서 제어해야 하는 상황이 발생할 수도 있는데, 이런 경우 `ref`를 사용합니다. `ref`를 사용하면 코드를 DOM에 직접 접근하거나 조작할 수 있습니다.

### ref를 사용하는 경우

`ref`를 사용하는 예시는 다음과 같습니다.

- `포커스 제어`

  특정 input 요소에 포커스를 주고 싶을 때 사용합니다.

- `텍스트 선택 또는 포커스`

  자동으로 포커스가 입력 상자에 맞춰지게 하거나, 사용자가 입력한 텍스트를 선택해야 할 때 사용합니다.

- `애니메이션 제어`

  특정 DOM 요소에 접근하여 애니메이션을 제어하거나 트리거할 때 사용할 수 있습니다.

- `외부 라이브러리 통합`

  외부 라이브러리가 DOM을 직접 조작해야 할 때 `ref`로 DOM 요소를 전달할 수 있습니다.

### ref 사용 시 주의사항

`ref`를 사용할 때 알아두어야 할 주의사항은 다음과 같습니다.

- <b>ref는 반드시 필요한 경우에만 사용할 것</b>

  일반적으로 React에서는 상태와 props를 통해 UI를 선언적 방식으로 관리하므로 DOM에 직접 접근해야 할 일이 많지 않습니다. `ref`는 이를 우회하고 DOM에 직접 접근하여 제어하는 명령적 방식이므로, `ref`는 반드시 필요한 경우에만 사용해야 합니다.

- <b>ref와 상태 관리</b>

  `ref`는 렌더링에 영향을 주지 않습니다. `ref` 값이 변경되어도 컴포넌트는 다시 렌더링되지 않으므로, 만약 UI에 영향을 미쳐야 한다면 `ref` 대신 `상태(state)`를 사용해야 합니다.

## forwardRef란?

`forwardRef`란 React에서 컴포넌트에 `ref`를 전달하기 위해 사용하는 고차 함수입니다. 기본적으로 함수형 컴포넌트를 사용하는 경우, 부모 컴포넌트에서 자식 컴포넌트로 `ref`를 전달할 수 없지만, `forwardRef`를 사용하면 부모 컴포넌트에서 자식 컴포넌트로 `ref`를 전달할 수 있습니다. 이를 통해 자식 컴포넌트의 DOM 요소나 다른 컴포넌트의 인스턴스에 직접 접근할 수 있습니다.

## Step 1 - 자식 컴포넌트

TypeScript에서 `forwardRef`를 사용할 때, `props`와 `ref`의 타입을 명시적으로 지정해야 합니다. `ref`는 전달된 DOM 요소나 컴포넌트 인스턴스의 타입을 정의하고, `props`는 해당 컴포넌트에 전달될 프로퍼티를 정의하면 됩니다.

```typescript
// MyChildComponent.tsx

import { forwardRef } from "react";

interface Props {
  text: string;
}

const MyChildComponent = forwardRef<HTMLDivElement, Props>(({ text }, ref) => {
  return <div ref={ref}>{text}</div>;
});

export default MyChildComponent;
```

위의 코드에서 `HTMLDivElement`는 `ref`가 참조할 DOM 요소의 타입을 나타냅니다. `div` 태그를 참조하고 있으므로 `HTMLDivElement`를 지정하였습니다. 또한 `Props`는 해당 컴포넌트에 전달될 프로퍼티들을 정의한 인터페이스를 지정하였습니다.

## Step 2 - displayName 설정하기

TypeScript에서 `forwardRef`를 사용할 때 **Component definition is missing display name**라는 오류가 발생합니다. 이는 `forwardRef`를 호출할 때 익명 함수를 넘기게 되면 React 개발자 도구에서 해당 컴포넌트의 이름을 알 수 없기 때문에 해당 경고가 발생하는 것입니다. 이를 해결하기 위해 `forwardRef`로 래핑된 컴포넌트에 이름을 `displayName`을 사용하여 설정하면 됩니다.

```typescript
// MyChildComponent.tsx

import { forwardRef } from "react";

interface Props {
  text: string;
}

const MyChildComponent = forwardRef<HTMLDivElement, Props>(({ text }, ref) => {
  return <div ref={ref}>{text}</div>;
});

// displayName을 명시적으로 설정합니다.
MyChildComponent.displayName = "MyChildComponent";

export default MyChildComponent;
```

## Step 3 - 부모 컴포넌트

부모 컴포넌트에서는 `useRef`를 사용하여 `ref`를 생성하고, 해당 `ref`를 자식 컴포넌트로 전달합니다.

```typescript
// MyParentComponent.tsx

import { useRef } from "react";

const MyParentComponent = () => {
  const ref = useRef<HTMLDivElement>(null);

  /* ref를 사용하는 기능을 추가하는 곳 */

  return <MyChildComponent text="My Text" ref={ref} />;
};

export default MyParentComponent;
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

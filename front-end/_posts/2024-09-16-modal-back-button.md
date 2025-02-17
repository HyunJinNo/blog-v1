---
layout: post
title: 모달 창(Modal Window) 뒤로가기 이벤트 처리 방법
description: >
  모달 창(Modal Window) 뒤로가기 이벤트 처리 방법에 대해 설명하는 페이지입니다.
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

<h2> 목차 </h2>

- [개요](#개요)
- [모달 창(Modal Window)란?](#모달-창modal-window란)
  - [모달 창의 주요 특징](#모달-창의-주요-특징)
  - [모달 창의 종류](#모달-창의-종류)
- [Step 1 - useModalBackHandler 커스텀 훅 생성하기](#step-1---usemodalbackhandler-커스텀-훅-생성하기)
- [Step 2 - useModalBackHandler 커스텀 훅 사용하기](#step-2---usemodalbackhandler-커스텀-훅-사용하기)
- [Step 3 - 모달 창을 닫을 때 history 제거하기](#step-3---모달-창을-닫을-때-history-제거하기)
- [테스트 결과](#테스트-결과)
- [Comments](#comments)

## 개요

이번 글에서는 모달 창(Modal Window)이 열린 상태에서 뒤로가기 버튼을 클릭했을 때 모달 창을 닫는 방법에 대해 설명하겠습니다.

## 모달 창(Modal Window)란?

`모달 창(Modal Window)`란 사용자 인터페이스(UI)에서 화면의 다른 부분과의 상호 작용을 잠시 차단하고, 사용자가 반드시 확인하거나 조작해야 하는 창 또는 대화 상자를 말합니다. 웹에서는 주로 브라우저 프로그램 자체에서 새 창을 띄우는 팝업 창과 달리 같은 창 내부에서 상위 레이어를 띄우는 방식을 사용하는 창을 의미합니다.

### 모달 창의 주요 특징

모달창의 주요 특징은 다음과 같습니다.

- `포커스 집중`

  모달 창이 열리면 해당 모달 창이 화면의 중심에 위치하고, 사용자는 그 모달 안에 있는 요소와만 상호작용할 수 있습니다. 보통 배경에 있는 다른 요소는 비활성화되거나 흐리게 처리됩니다.

- `작업 완료 후 닫힘`

  사용자가 모달 창에서 특정 작업을 완료하거나 취소해야만 모달 창이 닫힙니다. 작업을 완료하지 않으면 배경의 다른 요소와 상호작용할 수 없습니다.

- `작업 중단 방지`

  중요한 정보나 작업을 요구하는 경우 모달 창을 사용하여 사용자의 주의를 끌고, 해당 작업이 완료될 때까지 다른 작업을 수행하지 못하게 합니다.

- `다양한 용도`

  모달 창은 사용자에게 중요한 정보를 제공하거나 특정 작업을 요구할 때 사용됩니다. 모달 창은 주로 다음과 같이 다양한 용도로 사용됩니다.

  - `경고창`

    중요한 경고 메세지나 사용자 확인을 요구할 때 사용됩니다.

  - `로그인 창`

    사용자 로그인 또는 인증을 요구할 때 사용됩니다.

  - `이미지 확대 보기`

    갤러리에서 선택한 이미지를 크게 표시할 때 사용됩니다.

  - `폼 입력`

    데이터를 입력하거나 특정 작업을 처리할 때 사용됩니다.

### 모달 창의 종류

모달 창의 종류는 다음과 같습니다.

- `정보성 모달 창`

  사용자가 반드시 확인해야 할 정보를 표시하는 모달 창으로, 확인 버튼을 눌러야 닫힙니다.

- `확인/취소 모달 창`

  사용자가 중요한 작업을 수행할 때 확인을 요구하는 모달 창으로, 작업을 완료하거나 취소할 수 있습니다.

- `폼 모달 창`

  데이터를 입력하거나 편집할 수 있는 폼을 제공하는 모달 창으로, 사용자의 닉네임을 변경하거나 프로필을 변경하는 등의 작업을 수행할 수 있습니다.

- `다중 모달 창`

  하나의 모달 창에서 또 다른 모달 창을 열 수 있지만, 사용자가 혼란을 겪을 수 있으므로 UX 관점에서는 지양됩니다.

## Step 1 - useModalBackHandler 커스텀 훅 생성하기

먼저 다음과 같이 뒤로가기 버튼을 눌렀을 때 모달 창을 닫을 수 있는 커스텀 훅을 생성합니다.

```typescript
// useModalBackHandler.ts

import { useEffect } from "react";

/**
 * 뒤로가기 버튼을 눌렀을 때 모달 창을 닫는 커스텀 훅
 *
 * @param isOpen 모달 창이 열린 상태인지 여부
 * @param closeModal 모달 창을 닫는 메서드
 */
const useModalBackHandler = (isOpen: boolean, closeModal: () => void) => {
  useEffect(() => {
    if (isOpen) {
      // 모달 창이 열릴 때 history에 새로운 상태 추가
      window.history.pushState(null, "");
    }

    const handlePopState = () => {
      if (isOpen) {
        closeModal(); // 모달 창 닫기
      }
    };

    // popstate 이벤트 리스너 추가
    window.addEventListener("popstate", handlePopState);

    return () => {
      // 컴포넌트가 언마운트되거나 모달 창이 닫힐 때 이벤트 리스너 제거
      window.removeEventListener("popstate", handlePopState);
    };
  }, [closeModal, isOpen]);
};

export default useModalBackHandler;
```

위의 커스텀 훅은 모달 창이 열릴 때(isOpen가 true일 때) `window.history.pushState`를 사용하여 history에 새로운 상태를 추가합니다. 그리고 뒤로가기 버튼이 눌리면 발생하는 이벤트인 `popstate` 이벤트를 감지하여 뒤로가기 버튼을 클릭했을 때 모달 창을 닫습니다.

## Step 2 - useModalBackHandler 커스텀 훅 사용하기

위에서 생성한 커스텀 훅을 사용하면 뒤로가기 버튼을 클릭 시 열린 모달 창을 닫을 수 있습니다. 다음은 useModalBackHandler 커스텀 훅을 사용한 예시입니다.

```typescript
(...)

const [modalVisible, setModalVisible] = useState<boolean>(false);
useModalBackHandler(modalVisible, () => setModalVisible(false));

(...)
```

위의 코드에서 `modalVisible`이 true이면, 모달 창이 열린 상태이며, false일 경우 모달 창이 닫힌 상태임을 나타냅니다. 또한 `() => setModalVisible(false)`는 모달 창을 닫는 함수입니다.

## Step 3 - 모달 창을 닫을 때 history 제거하기

위에서 정의한 커스텀 훅은 모바일 또는 웹 브라우저에서 뒤로가기 버튼을 눌렀을 때, 모달 창이 열리면서 추가한 history를 제거합니다. 하지만 일반적으로 모달 창에는 모달 창을 닫을 수 있는 버튼을 제공하므로 해당 버튼을 클릭했을 때 history를 제거할 수 있어야 합니다. 따라서 `window.history.back()`을 추가하여 뒤로가기 버튼이 아닌 다른 방법으로 모달 창을 닫을 때 history를 제거할 수 있도록 합니다.

```typescript
(...)

closeModal={() => {
  window.history.back();
  setModalVisible(false);
}}

(...)
```

## 테스트 결과

테스트 결과는 다음과 같습니다.

<video width="720" controls> 
<source src="/assets/video/front-end/modal-back-button/video1.mp4" type="video/mp4" />
Your browser does not support the video tag.
</video>

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

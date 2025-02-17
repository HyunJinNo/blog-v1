---
layout: post
title: Debounce와 Throttle
description: >
  Debounce와 Throttle에 대해 정리한 페이지입니다.
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
- [Debounce와 Throttle이란?](#debounce와-throttle이란)
- [Debounce](#debounce)
  - [동작 원리](#동작-원리)
  - [useDebounce 구현하기](#usedebounce-구현하기)
  - [사용 예시](#사용-예시)
- [Throttle](#throttle)
  - [동작 원리](#동작-원리-1)
  - [useThrottle 구현하기](#usethrottle-구현하기)
  - [사용 예시](#사용-예시-1)
- [참고 자료](#참고-자료)
- [Comments](#comments)

## 개요

<hr />

`Debounce`와 `Throttle`에 대해 정리한 페이지입니다.

## Debounce와 Throttle이란?

<hr />

`Debounce`와 `Throttle`은 성능 최적화를 위한 기술로, 주로 사용자의 입력 이벤트(스크롤, 창 크기 조정 등)가 너무 자주 발생하는 경우 이를 조절하는 역할을 수행합니다. 이 둘의 핵심은 모두 <b>특정 함수의 호출 횟수를 줄여서 웹 성능이 저하되는 것을 방지하는 것</b>입니다.

`Debounce`와 `Throttle`의 차이점을 비교하면 다음과 같습니다.

| 구분       | 동작 방식                                           | 사용 사례                                    |
| ---------- | --------------------------------------------------- | -------------------------------------------- |
| `Debounce` | 특정 시간 동안 이벤트가 발생하지 않으면 함수를 실행 | 검색 입력, 자동 저장, 실시간 필터            |
| `Throttle` | 일정 시간 간격으로 함수를 실행                      | 무한 스크롤, 버튼 연타 방지, 윈도우 리사이즈 |

## Debounce

<hr />

`Debounce`는 연속적인 이벤트가 발생해도 마지막 이벤트가 발생한 후 일정 시간이 지나야 함수를 실행하도록 제한하는 역할을 수행합니다. 주로 검색 자동 완성, 검색어 입력 시 API 호출 등의 상황에서 사용합니다.

### 동작 원리

`Debounce`의 동작 원리는 다음과 같습니다.

1. 이벤트가 발생하면 타이머를 설정합니다.
2. 만약 기존 타이머가 완료되기 전에 이벤트가 발생하는 경우 기존 타이머를 제거하고 새로운 타이머를 설정합니다.
3. 타이머가 완료되면 함수를 실행합니다. 이후 `1.`부터 반복합니다.

### useDebounce 구현하기

`useDebounce` 커스텀 훅을 구현한 예시는 다음과 같습니다.

```typescript
/* useDebounce.ts */

import { useRef } from "react";

export const useDebounce = <T extends unknown[]>(
  callback: (...params: T) => void,
  debounceTime = 1000
) => {
  const timeout = useRef<ReturnType<typeof setTimeout> | null>(null);

  return (...params: T) => {
    if (timeout.current) {
      clearTimeout(timeout.current);
    }

    timeout.current = setTimeout(() => {
      callback(...params);
      timeout.current = null;
    }, debounceTime);
  };
};
```

위의 코드를 설명하자면 다음과 같습니다.

```typescript
const timeout = useRef<ReturnType<typeof setTimeout> | null>(null);
```

타이머를 관리하는 변수입니다. `useRef`를 사용하여 리렌더링이 되더라도 타이머가 초기화되는 문제를 방지합니다.

```typescript
if (timeout.current) {
  clearTimeout(timeout.current);
}
```

기존 타이머가 완료되기 전에 이벤트가 발생하는 경우 기존 타이머를 제거하는 부분입니다.

```typescript
timeout.current = setTimeout(() => {
  callback(...params);
  timeout.current = null;
}, debounceTime);
```

마지막 이벤트가 발생한 후 일정 시간이 지난 후에 함수를 실행하는 코드입니다.

### 사용 예시

```typescript
/* useSurveyContentItemList.ts */

import { BACKEND_URL } from "@env";
import { zodResolver } from "@hookform/resolvers/zod";
import { useDebounce } from "@src/hooks/common/useDebounce";
import { getNewAccessToken } from "@src/libs/getNewAccessToken";
import { ContentTitleSchema } from "@src/libs/zod/ContentTitleSchema";
import { SurveyContent, SurveyContentList } from "@src/types/survey";
import { useSuspenseQuery } from "@tanstack/react-query";
import { useEffect, useState } from "react";
import { useForm } from "react-hook-form";
import EncryptedStorage from "react-native-encrypted-storage";

export const useSurveyContentItemList = (
  contentCategory: "DRAMA" | "ARTIST" | "MOVIE" | "ENTERTAINMENT"
) => {
  const { data } = useSuspenseQuery<SurveyContentList>({
    queryKey: ["surveyContentItemList", contentCategory],
    queryFn: async () => {
      const accessToken = await EncryptedStorage.getItem("access_token");
      const response = await fetch(
        `${BACKEND_URL}/api/media?type=${contentCategory}&page=0&size=636`,
        { method: "GET", headers: { Cookie: `access_token=${accessToken}` } }
      );

      if (response.status === 401) {
        await getNewAccessToken();
        throw new Error("Access token has expired.");
      }

      if (!response.ok) {
        throw new Error("Failed to fetch data.");
      }

      return await response.json();
    },
    staleTime: 1000 * 60 * 10,
    gcTime: 1000 * 60 * 30,
    retry: 1,
  });

  const methods = useForm<{ title: string }>({
    resolver: zodResolver(ContentTitleSchema),
    defaultValues: { title: "" },
    mode: "onChange",
  });

  const [surveyContentList, setSurveyContentList] = useState<SurveyContent[]>(
    data.content
  );

  const handleFiltering = useDebounce(
    () =>
      setSurveyContentList(
        data.content.filter((content) =>
          content.mediaName
            .replaceAll(" ", "")
            .includes(methods.watch("title").replaceAll(" ", ""))
        )
      ),
    300
  );

  useEffect(() => {
    handleFiltering();
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, [methods.watch("title")]);

  return { surveyContentList, methods };
};
```

<video controls> 
<source src="/assets/video/front-end/debounce-throttle/video1.mp4" type="video/mp4" />
Your browser does not support the video tag.
</video>

## Throttle

<hr />

`Throttle`은 일정 시간 간격으로 함수가 최대 한 번만 실행되도록 제한하는 역할을 수행합니다. 주로 무한 스크롤, 버튼 연타 방지, 윈도우 리사이즈 등에서 사용됩니다.

### 동작 원리

`Throttle`의 동작 원리는 다음과 같습니다.

1. 이벤트가 발생하면 함수를 호출한 후 타이머를 설정합니다.
2. 만약 타이머가 완료되기 전에 이벤트가 발생하는 경우 함수를 호출하지 않습니다.
3. 타이머가 완료된 후에 이벤트가 발생하면 `1.`부터 반복합니다.

### useThrottle 구현하기

`useThrottle` 커스텀 훅을 구현한 예시는 다음과 같습니다.

```typescript
/* useThrottle.ts */

import { useRef } from "react";

export const useThrottle = <T extends unknown[]>(
  callback: (...params: T) => void,
  throttleTime = 1000
) => {
  const timeout = useRef<ReturnType<typeof setTimeout> | null>(null);

  return (...params: T) => {
    if (timeout.current) {
      return;
    }

    callback(...params);

    timeout.current = setTimeout(() => {
      timeout.current = null;
    }, throttleTime);
  };
};
```

위의 코드를 설명하자면 다음과 같습니다.

```typescript
const timeout = useRef<ReturnType<typeof setTimeout> | null>(null);
```

타이머를 관리하는 변수입니다. `useRef`를 사용하여 리렌더링이 되더라도 타이머가 초기화되는 문제를 방지합니다.

```typescript
if (timeout.current) {
  return;
}

callback(...params);
```

타이머가 설정되기 전에 이벤트가 발생하면 함수를 호출하는 부분입니다. 만약 타이머가 완료되기 전에 이벤트가 발생하는 경우 함수를 호출하지 않습니다.

```typescript
timeout.current = setTimeout(() => {
  timeout.current = null;
}, throttleTime);
```

함수 호출 후 일정 시간이 지난 후에 함수를 실행할 수 있도록 타이머를 설정하는 부분입니다.

### 사용 예시

```typescript
/* LobbyItem.tsx */

"use client";

import { Room } from "@/types/room";
import UsersIcon from "../common/icons/UsersIcon";
import { ROOM_STATUS } from "@/constants/roomStatus";
import { useSocketStore } from "@/stores/socketStore";
import { useThrottle } from "@/hooks/utils/useThrottle";

interface LobbyItemProps {
  room: Room;
  setTargetRoom: () => void;
}

const LobbyItem = ({ room, setTargetRoom }: LobbyItemProps) => {
  const { socket } = useSocketStore();

  const enterRoom = useThrottle(() => {
    setTargetRoom();
    socket?.emit("enter-room", { roomId: room.roomId });
  }, 2000);

  return (
    <div
      className={[
        `${
          (room.participants === room.capacity || room.status === "RUNNING") &&
          "opacity-50"
        }`,
        "flex h-60 flex-col justify-between rounded-3xl border border-slate-200 bg-slate-600/50 p-6 duration-300 hover:bg-slate-400/50",
      ].join(" ")}
    >
      <div className="flex h-8 w-20 items-center justify-center rounded-2xl border border-blue-200 bg-blue-50 text-xs text-blue-800">
        {ROOM_STATUS[room.status]}
      </div>
      <div className="flex flex-col gap-3">
        <h2 className="line-clamp-2 text-lg text-white">{room.title}</h2>
        <p className="text-sm text-slate-200">{room.owner}</p>
        <div className="flex flex-row items-center justify-between">
          <div className="flex flex-row items-center gap-2">
            <UsersIcon />
            <p className="text-white">
              {room.participants} /{" "}
              <span className="font-bold">{room.capacity}</span>
            </p>
          </div>
          {room.participants === room.capacity || room.status === "RUNNING" ? (
            <div className="flex h-9 w-[7.5rem] cursor-not-allowed items-center justify-center rounded-2xl bg-white text-sm font-semibold text-slate-800">
              참가하기
            </div>
          ) : (
            <button
              className="flex h-9 w-[7.5rem] items-center justify-center rounded-2xl bg-white text-sm font-semibold text-slate-800 hover:scale-105"
              onClick={() => enterRoom()}
            >
              참가하기
            </button>
          )}
        </div>
      </div>
    </div>
  );
};

export default LobbyItem;
```

## 참고 자료

<hr />

- <a href="https://velog.io/@ansrjsdn/TypeScript%EC%97%90%EC%84%9C-useDebounce-useThrottle-%EB%A7%8C%EB%93%A4%EA%B8%B0" target="_blank">TypeScript에서 useDebounce, useThrottle 만들기</a>

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

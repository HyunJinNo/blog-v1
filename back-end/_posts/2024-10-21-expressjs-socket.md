---
layout: post
title: Express.js socket.io 사용 방법
description: >
  Express.js에서 socket.io 사용 방법을 설명하는 페이지입니다.
image:
  path: /assets/img/back-end/back-end.jpg
related_posts:
  - None
sitemap: true
comments: false
---

<i>Environment</i>

- <i>Node.js v20.11.1</i>
- <i>express v4.21.0</i>
- <i>socket.io v4.8.0</i>
- <i>typeorm v0.3.20</i>

<h2>목차</h2>

- [개요](#개요)
- [socket.io란?](#socketio란)
- [socket.io의 특징](#socketio의-특징)
- [Step 1 - 서버 설정하기](#step-1---서버-설정하기)
  - [socket.io 패키지 설치하기](#socketio-패키지-설치하기)
  - [chat 테이블 생성하기](#chat-테이블-생성하기)
  - [채팅 API 구현하기](#채팅-api-구현하기)
  - [HTTP 서버와 Socket.IO 연결하기](#http-서버와-socketio-연결하기)
- [Step 2 - 클라이언트 설정하기](#step-2---클라이언트-설정하기)
  - [socket.io-client 패키지 설치하기](#socketio-client-패키지-설치하기)
  - [채팅 컴포넌트 구현하기](#채팅-컴포넌트-구현하기)
- [Step 3 - 테스트 결과](#step-3---테스트-결과)
- [참고 자료](#참고-자료)
- [Comments](#comments)

## 개요

이번 글에서는 `Express.js`에서 `socket.io`를 활용하여 채팅 애플리케이션을 만드는 방법을 설명하겠습니다. 웹소켓에 대한 개념의 경우 다음 링크에 작성하였으니 참고하시길 바랍니다.

<a href="../../cs/2024-08-16-websocket">웹소켓 (WebSocket)</a>

## socket.io란?

`socket.io`란 웹소켓을 기반으로 서버와 클라이언트의 양방향 통신을 지원하는 라이브러리입니다. 주로 실시간 웹 애플리케이션을 개발할 때 자주 사용되는 JavaScript 라이브러리입니다. 기본적으로 웹소켓을 지원하며, 웹소켓을 지원하지 않는 브라우저에서는 롱폴링 방식을 사용한 통신을 지원합니다. 또한 재접속, 브로드캐스트, 멀티플렉싱(채팅방) 기능도 제공합니다.

## socket.io의 특징

`socket.io`의 특징은 다음과 같습니다.

- **브라우저 호환성(Browser Compatibility)**

  `socket.io`는 다양한 브라우저 환경에서 호환성을 보장합니다. 웹소켓을 지원하지 않는 브라우저에서는 `롱폴링(Long Polling)`과 같은 다른 대체 통신 방식을 사용하여 연결을 유지합니다.

- **자동 재연결(Automatic Reconnection)**

  클라이언트와 서버 간의 연결이 끊어지면, `socket.io`는 자동으로 재연결을 시도합니다. 이를 통해 네트워크 상태가 불안정한 환경에서도 안정적인 통신이 가능합니다.

- **이벤트 기반 통신(Event-based Communication)**

  `socket.io`는 이벤트 기반 모델을 사용하여 데이터를 주고받습니다. 개발자는 특정 이벤트를 정의하고, 그 이벤트가 발생할 때 실행한 콜백 함수를 지정할 수 있습니다. 이를 통해 데이터 전송 및 처리 로직을 단순하고 직관적으로 만들 수 있습니다.

- **실시간 채팅, 알림, 스트리밍에 최적화**

  `socket.io`는 실시간 채팅 애플리케이션, 알림 시스템, 스트리밍 애플리케이션 등에 최적화되어 있어, 실시간 웹 애플리케이션을 개발할 때 널리 사용됩니다.

- **네임스페이스(Namespaces)**

  `socket.io`는 네임스페이스를 지원하여 동일한 서버에서 여러 개의 독립적인 통신 채널을 관리할 수 있습니다. 이를 통해 다양한 유형의 통신을 분리하고 관리할 수 있습니다.

- **룸(Rooms)**

  네임스페이스 내에서 클라이언트를 그룹으로 묶어 특정 그룹(룸)으로 메세지를 보낼 수 있습니다. 예를 들어, 같은 채팅방에 있는 사용자들에게만 메세지를 전송할 때 유용합니다.

## Step 1 - 서버 설정하기

### socket.io 패키지 설치하기

먼저 다음 명령어를 입력하여 `socket.io` 패키지를 설치합니다.

```bash
npm install socket.io
```

### chat 테이블 생성하기

채팅 내용을 저장하기 위해 다음과 같이 채팅 내용을 저장하는 `chat` 테이블을 생성합니다. 해당 테이블은 작성자 닉네임 정보와 채팅 내용을 저장하는 테이블입니다.

```typescript
// chatEntity.ts

import {
  Column,
  CreateDateColumn,
  Entity,
  PrimaryGeneratedColumn,
} from "typeorm";

@Entity("chat")
export class Chat {
  @PrimaryGeneratedColumn({ name: "chat_id" })
  chatId: number;

  @Column("varchar", { name: "nickname", length: "20" })
  nickname: string;

  @Column("varchar", { name: "message", length: "50" })
  message: string;

  @CreateDateColumn({ name: "created_at" })
  createdAt: Date;
}
```

### 채팅 API 구현하기

사용자가 채팅 애플리케이션에 접속할 때 기존의 채팅 내용을 불러올 수 있어야 합니다. 다음과 같이 모든 채팅 목록을 조회하는 API를 작성합니다. 또한 추가적으로 채팅을 입력하면 저장할 수 있는 API도 작성합니다.

```typescript
// chatService.ts

import { AppDataSource } from "../../dataSource";
import { ChatCreateRequest } from "./chatDto";
import { Chat } from "./chatEntity";

const chatRepository = AppDataSource.getRepository(Chat);

/**
 * @description 모든 채팅 목록 조회
 * @returns 모든 채팅 목록
 */
const getChatList = async () => {
  return await chatRepository
    .createQueryBuilder("chat")
    .orderBy("chat.createdAt", "ASC")
    .getMany();
};

/**
 * @description 채팅 메세지 생성
 * @param chatCreateRequest
 */
const createChat = async (chatCreateRequest: ChatCreateRequest) => {
  await chatRepository
    .createQueryBuilder("chat")
    .insert()
    .into(Chat)
    .values({
      ...chatCreateRequest,
    })
    .execute();
};

export const chatService = {
  getChatList,
  createChat,
};
```

### HTTP 서버와 Socket.IO 연결하기

다음과 같이 `app.ts` 파일에서 HTTP 서버와 Socket.IO를 연결하는 코드를 작성합니다.

```typescript
import express from "express";
import { createServer } from "node:http";
import { Server } from "socket.io";
import { AppDataSource } from "./dataSource";
import cors from "cors";
import cron from "node-cron";
import { chatService } from "./modules/chat/chatService";
import { ChatCreateRequest } from "./modules/chat/chatDto";

AppDataSource.initialize()
  .then(async () => {
    const app = express();
    const server = createServer(app);
    const io = new Server(server, {
      cors: {
        origin: "http://localhost:5173",
      },
    });
    const port = Number(process.env.PORT ?? 4000);

    app.use(express.json());
    app.use(express.urlencoded({ extended: true }));
    app.use(express.static("public"));
    app.use(cors());

    app.get("/", (req, res) => {
      res.send("Newsstand Server");
    });

    const chatNamespace = io.of("/chat");

    chatNamespace.on("connection", async (socket) => {
      // 기존 메세지 불러오기
      const chatList = await chatService.getChatList();
      socket.emit("previous messages", chatList);

      socket.on(
        "chat message",
        async (chatCreateRequest: ChatCreateRequest) => {
          await chatService.createChat(chatCreateRequest);
          chatNamespace.emit("chat message", chatCreateRequest);
        }
      );

      socket.on("disconnect", () => {
        console.log("user disconnected");
      });
    });

    server.listen(port, () => {
      console.log();
      console.log(`  [Local] http://localhost:${port}`);
      console.log();
    });
  })
  .catch((err) => console.error(err));
```

위의 코드를 설명하자면 다음과 같습니다.

먼저 Socket.IO를 사용하기 위해서 다음과 같이 HTTP 서버를 생성합니다.

```typescript
const server = createServer(app);
```

HTTP 서버를 생성한 이후에는 다음과 같이 HTTP 서버와 Socket.IO를 연결합니다. 이 때 동일한 기기에서 서로 다른 포트 위에 Express.js 애플리케이션과 리액트 애플리케이션을 실행시킬 예정이므로 위와 같이 CORS 설정을 추가하였습니다.

```typescript
const io = new Server(server, {
  cors: {
    origin: "http://localhost:5173",
  },
});
```

연결 이후에는 다음과 같이 소켓 연결 및 메세지를 처리하는 코드를 작성합니다.

```typescript
const chatNamespace = io.of("/chat");

chatNamespace.on("connection", async (socket) => {
  // 기존 메세지 불러오기
  const chatList = await chatService.getChatList();
  socket.emit("previous messages", chatList);

  socket.on("chat message", async (chatCreateRequest: ChatCreateRequest) => {
    await chatService.createChat(chatCreateRequest);
    chatNamespace.emit("chat message", chatCreateRequest);
  });

  socket.on("disconnect", () => {
    console.log("user disconnected");
  });
});
```

위의 코드에서 소켓 연결이 성공한 경우 기존의 채팅 메세지 목록을 웹 페이지에 접속한 클라이언트에게 전송합니다. 또한 클라이언트로부터 채팅 메세지를 전달받는 경우 DB에 저장한 후 메세지를 보낸 클라이언트를 포함한 모든 클라이언트에게 채팅 메세지를 전달합니다. 이 때 `/chat`이라는 네임스페이스를 사용하였으므로 전체 클라이언트에게 메세지를 전송하기 위해선 `io.emit()` 대신 `chatNamespace.emit()`을 사용해야 합니다.

## Step 2 - 클라이언트 설정하기

### socket.io-client 패키지 설치하기

먼저 다음 명령어를 입력하여 `socket.io-client` 패키지를 설치합니다.

```bash
npm install socket.io-client
```

### 채팅 컴포넌트 구현하기

다음과 같이 `socket.io-client`를 사용하여 채팅 기능을 구현하는 컴포넌트를 생성합니다.

```typescript
import { useEffect, useState } from "react";
import { io } from "socket.io-client";

const socket = io(`${import.meta.env.VITE_BACKEND_URL}/chat`);

const ChatViewer = () => {
  const [chatList, setChatList] = useState<
    { nickname: string; message: string }[]
  >([]);
  const [nickname, setNickname] = useState("");
  const [message, setMessage] = useState("");

  const sendMessage = () => {
    socket.emit("chat message", { nickname, message });
    setNickname("");
    setMessage("");
  };

  useEffect(() => {
    socket.on(
      "previous messages",
      (previousMessages: { nickname: string; message: string }[]) => {
        setChatList(previousMessages);
      }
    );

    socket.on("chat message", (chat: { nickname: string; message: string }) => {
      setChatList([...chatList, chat]);
    });

    return () => {
      socket.off("previous messages");
      socket.off("chat message");
    };
  }, [chatList]);

  return (
    <div className="fixed left-6 top-6 z-10 flex h-[40rem] w-60 flex-col gap-2 rounded-2xl border bg-white p-4 drop-shadow transition-colors duration-500 dark:bg-grayscale-500">
      <div className="h-full w-full rounded-2xl border border-grayscale-100 p-4 transition-colors duration-500 dark:text-white">
        {chatList.map((value, index) => (
          <p key={index}>{`${value.nickname}: ${value.message}`}</p>
        ))}
      </div>
      <form
        className="flex flex-col gap-2"
        onSubmit={(e) => {
          e.preventDefault();
          sendMessage();
        }}
      >
        <input
          className="w-full rounded-2xl border border-grayscale-100 px-4 py-1 outline-none transition-colors duration-500 dark:bg-grayscale-400 dark:text-white"
          type="text"
          maxLength={20}
          value={nickname}
          placeholder="닉네임"
          onChange={(e) => setNickname(e.target.value)}
        ></input>
        <input
          className="w-full rounded-2xl border border-grayscale-100 px-4 py-1 outline-none transition-colors duration-500 dark:bg-grayscale-400 dark:text-white"
          type="text"
          max={50}
          value={message}
          placeholder="메세지"
          onChange={(e) => setMessage(e.target.value)}
        ></input>
        <button
          className="h-8 w-full rounded-2xl border bg-custom-blue-100 text-white hover:scale-105"
          type="submit"
        >
          보내기
        </button>
      </form>
    </div>
  );
};

export default ChatViewer;
```

위의 코드를 설명하자면 다음과 같습니다.

먼저 소켓 초기화를 진행합니다. 파라미터로 연결할 서버의 URL을 입력하면 됩니다. 또한 `chat`이라는 네임스페이스에 접근하기 위해 URL 뒤의 `/chat`을 붙였습니다.

```typescript
const socket = io(`${import.meta.env.VITE_BACKEND_URL}/chat`);
```

위의 코드를 통해 서버 연결이 성공한 경우 클라이언트에서 `connect` 이벤트가 발생하며, 서버에서는 `connection` 이벤트가 발생하게 됩니다. 서버 코드에서 `connection` 이벤트가 발생하면 기존의 채팅 메시지 목록을 전달하는 이벤트(=`previous messages`)를 발생시킵니다.

소켓 초기화 이후에는 다음과 같이 서버로부터 받은 이벤트를 처리하기 위해 `useEffect()`에 메세지 수신 코드를 작성합니다.

```typescript
useEffect(() => {
  socket.on(
    "previous messages",
    (previousMessages: { nickname: string; message: string }[]) => {
      setChatList(previousMessages);
    }
  );

  socket.on("chat message", (chat: { nickname: string; message: string }) => {
    setChatList([...chatList, chat]);
  });

  return () => {
    socket.off("previous messages");
    socket.off("chat message");
  };
}, [chatList]);
```

마지막으로 입력한 채팅 메세지를 전송하기 위한 form 이벤트 코드를 구현합니다.

```typescript
const sendMessage = () => {
  socket.emit("chat message", { nickname, message });
  setNickname("");
  setMessage("");
};
```

## Step 3 - 테스트 결과

테스트 결과는 다음과 같습니다.

<img src="/assets/img/back-end/expressjs-socket/pic1.png" alt="pic1" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

## 참고 자료

- <a href="https://socket.io" target="_blank">Socket.IO</a>

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

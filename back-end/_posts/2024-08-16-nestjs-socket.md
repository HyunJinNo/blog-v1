---
layout: post
title: NestJS socket.io 사용 방법
description: >
  NestJS에서 socket.io 사용 방법을 설명하는 페이지입니다.
image:
  path: /assets/img/back-end/back-end.jpg
related_posts:
  - None
sitemap: true
comments: false
---

<i>Environment</i>

- <i>Node.js v20.11.1</i>

<h2>목차</h2>

- [개요](#개요)
- [socket.io란?](#socketio란)
- [socket.io의 특징](#socketio의-특징)
- [Step 1 - 패키지 설치하기](#step-1---패키지-설치하기)
- [Step 2 - 정적 파일 서비스하기](#step-2---정적-파일-서비스하기)
- [Step 3 - 게이트웨이 생성하기](#step-3---게이트웨이-생성하기)
- [Step 4 - 게이트웨이 등록하기](#step-4---게이트웨이-등록하기)
- [Step 5 - 클라이언트 파일 생성하기](#step-5---클라이언트-파일-생성하기)
- [Step 6 - 테스트 결과](#step-6---테스트-결과)
- [참고 자료](#참고-자료)
- [Comments](#comments)

## 개요

이번 글에서는 `NestJS`에서 `socket.io`를 활용하여 채팅 애플리케이션을 만드는 방법을 설명하겠습니다. 웹소켓에 대한 개념의 경우 다음 링크에 작성하였으니 참고하시길 바랍니다.

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

## Step 1 - 패키지 설치하기

먼저 다음 명령어를 입력하여 필요한 패키지를 설치합니다.

```bash
npm install @nestjs/websockets @nestjs/platform-socket.io
```

```bash
npm install --save-dev @types/socket.io
```

각 패키지를 설명하자면 다음과 같습니다.

- `@nestjs/websockets`

  웹소켓 프로토콜 기반의 애플리케이션 구현 시 필요한 패키지입니다.

- `@nestjs/platform-socket.io`

  `NestJS`에서 `socket.io`을 사용할 때 설치하는 패키지입니다. `socket.io`가 아닌 웹소켓을 사용하기 위해 `@nestjs/platform-ws` 패키지를 대신 설치할 수 있습니다.

- `@types/socket.io`

  `socket.io`를 TypeScript 파일에서 사용할 수 있도록 설치하는 패키지입니다.

## Step 2 - 정적 파일 서비스하기

이번 글에서는 별도의 프론트엔드 프레임워크를 사용하지 않고 html 파일을 직접 작성하여 테스트할 예정입니다. `NestJS`에서 정적 파일을 서비스하는 방법은 [serve-static 패키지를 설치해서 서비스하는 방법](../2024-08-12-nestjs-file-upload/#step-4---정적-파일-서비스하기)도 있지만, 이번 글에서는 설정이 간단하므로 `Express.js`를 사용하여 `static asset`을 설정하는 방법을 선택하겠습니다. 다음과 같이 `main.ts` 파일에 정적 파일 경로를 지정하면 됩니다.

```typescript
(...)

import { NestFactory } from "@nestjs/core";
import { NestExpressApplication } from "@nestjs/platform-express";

// NestJS를 실행시키는 함수
// NestJS에서는 진입점을 bootstrap()으로 이름 짓는 것이 관례이다.
async function bootstrap() {
  // NestFactory를 사용해서 NestApplication 객체 생성
  const app = await NestFactory.create<NestExpressApplication>(AppModule);

  (...)

  // 정적 파일 경로 지정
  app.useStaticAssets(join(__dirname, "..", "static"));

  (...)
}

bootstrap();
```

위의 코드에서 `useStaticAssets()` 메서드에 경로만 지정하면 `NestJS`에세 정적 파일을 서비스할 수 있습니다. 또한 기존의 `main.ts` 파일과 달리 `NestFactory.create()` 메서드에 `NestExpressApplication`으로 반환값의 타입을 지정하였습니다. 이는 `useStaticAssets()` 미들웨어는 `Express.js`에 있기 때문에 `Express.js`의 미들웨어를 사용하기 위해 app 인스턴스를 만들 때 제네릭 타입으로 `NestExpressApplication`을 선언해야하기 때문입니다.

## Step 3 - 게이트웨이 생성하기

`NestJS`에서 웹소켓을 사용한 통신을 받아주는 클래스를 `게이트웨이(Gateways)`라고 부릅니다. HTTP 프로토콜을 컨트롤러가 받는다면, ws 프로토콜은 게이트웨이가 받습니다. 게이트웨이를 사용하면 의존성 주입, 데코레이터, 필터, 가드 등의 `NestJS` 기능을 사용할 수 있습니다.

다음과 같이 게이트웨이 파일을 생성합니다. `@WebSocketGateway()` 데코레이터를 클래스에 붙이면 해당 클래스는 게이트웨이 역할을 수행합니다.

```typescript
// app.gateway.ts

import {
  ConnectedSocket,
  MessageBody,
  SubscribeMessage,
  WebSocketGateway,
  WebSocketServer,
} from "@nestjs/websockets";
import { Server, Socket } from "socket.io";

// 웹소켓 서버 설정 데코레이터
@WebSocketGateway({ namespace: "chat" }) // 네임스페이스 추가
export class ChatGateway {
  @WebSocketServer() server: Server; // 웹소켓 서버 인스턴스 선언

  @SubscribeMessage("message") // message 이벤트 구독
  handleMessage(@ConnectedSocket() socket: Socket, @MessageBody() data: any) {
    const { message, nickname } = data; // 메세지와 닉네임을 데이터에서 추출

    // 접속한 모든 클라이언트들에게 메세지 전송
    // this.server.emit('message', `client-${socket.id.substring(0, 4)}: ${data}`);

    // 닉네임을 포함한 메세지 전송
    socket.broadcast.emit("message", `${nickname}: ${message}`);
  }
}

@WebSocketGateway({ namespace: "room" }) // room 네임스페이스를 사용하는 게이트웨이
export class RoomGateway {
  // 채팅 게이트웨이 의존성 주입
  constructor(private readonly chatGateway: ChatGateway) {}
  rooms = [];

  // 서버 인스턴스 접근을 위한 변수 선언
  @WebSocketServer() server: Server;

  @SubscribeMessage("createRoom") // createRoom 핸들러 메서드
  handleMessage(@MessageBody() data: any) {
    // 소켓없이 데이터만 받음.
    const { room, nickname } = data;

    // 방 생성 시 이벤트를 발생시켜 클라이언트에 송신
    this.chatGateway.server.emit("notice", {
      message: `${nickname}님이 ${room}방을 만들었습니다.`,
    });

    this.rooms.push(room); // 채팅방 정보를 받아서 추가
    this.server.emit("rooms", this.rooms); // rooms 이벤트로 채팅방 리스트 전송
  }

  @SubscribeMessage("joinRoom") // 방 입장 시 실행되는 핸들러 메서드
  handleJoinRoom(@ConnectedSocket() socket: Socket, @MessageBody() data: any) {
    const { room, nickname, toLeaveRoom } = data;
    socket.leave(toLeaveRoom); // 기존의 방에서 먼저 나간다.
    this.chatGateway.server.emit("notice", {
      message: `${nickname}님이 ${room}방에 입장했습니다.`,
    });
    socket.join(room); // 새로운 방에 입장합니다.
  }

  @SubscribeMessage("message") // RoomGateway로 message 이벤트가 오면 처리
  handleMessageToRoom(
    @ConnectedSocket() socket: Socket,
    @MessageBody() data: any
  ) {
    const { message, nickname, room } = data;
    console.log(data);

    // 나 이외의 사람들에게 데이터 전송
    socket.broadcast.to(room).emit("message", {
      message: `${nickname}: ${message}`,
    });
  }
}
```

위의 코드를 설명하자면 다음과 같습니다.

- `@WebSocketGateway()`

  게이트웨이 설정을 위한 데코레이터입니다. 해당 데코레이터를 클래스에 붙이면 해당 클래스는 게이트웨이 역할을 수행합니다. 데코레이터 설정 방법은 여러 가지가 존재합니다.

  | 데코레이터                 | 설명                             |
  | -------------------------- | -------------------------------- |
  | @WebSocketGateway()        | 기본 포트를 사용하는 설정        |
  | @WebSocketGateway(port)    | 매개변수로 주어진 포트를 사용    |
  | @WebSocketGateway(options) | 기본 포트를 사용하며 옵션을 적용 |
  | @WebSocketGateway()        | 포트와 옵션을 함께 설정          |

  옵션에 대해서는 다음 링크를 참고하시길 바랍니다.

  <a href="https://socket.io/docs/v4/server-options" target="_blank">Server options | Socket.IO</a>

- `{ namespace: "chat" }`

  네임스페이스를 지정하는 부분입니다. 네임스페이스는 네임스페이스로 지정된 곳에서만 이벤트를 발생시키고 메세지를 전송하는 것을 의미합니다. 다른 말로 멀티플렉싱이라고 합니다.

- `@WebSocketServer()`

  웹소켓 서버 인스턴스에 접근하는 데코레이터입니다.

- `@SubscribeMessage("message")`

  매개변수로 주어진 이벤트를 구독하는 리스너를 나타내는 데코레이터입니다. 클라이언트에서 특정 이벤트로 데이터를 전송하는 경우 해당 이벤트를 수신하는 리스너가 있으면 해당 리스너를 통해 클라이언트로부터 데이터를 전달받습니다.

- `@ConnectedSocket()`

  socket을 나타내는 데코레이터입니다.

- `@MessageBody()`

  클라이언트로부터 전달받은 데이터를 나타내는 데코레이터입니다.

- `this.server.emit()`

  웹소켓의 서버 인스턴스의 emit() 메서드입니다. 해당 메서드를 사용하면 접속한 모든 클라이언트들에게 메세지를 전송할 수 있습니다. 첫 번째 인수에는 이벤트명을 지정하고, 두 번째 인수에는 보내는 데이터를 지정합니다.

- `socket.id`

  클라이언트 인스턴스에 할당된 임의의 id값입니다. `socket.io`에서는 모든 클라이언트에 임의의 id값을 할당합니다.

- `socket.broadcast.emit()`

  전송을 요청한 클라이언트를 제외한 다른 클라이언트 모두에게 데이터를 전송하는 메서드입니다.

- `socket.broadcast.to().emit()`

  특정 채팅방에 입장한 클라이언트 중에서 자신을 제외한 모든 클라이언트에게 메세지를 전송하는 메서드입니다.

- `socket.leave()`

  채팅방에서 나가는 메서드입니다.

- `socket.join()`

  채팅방에 입장하는 메서드입니다.

## Step 4 - 게이트웨이 등록하기

게이트웨이를 사용하려면 모듈에 등록해야 합니다. 이 때 주의해야 할 점은 게이트웨이는 다른 클래스에 주입해서 사용할 수 있는 프로바이더라는 점입니다.

```typescript
(...)

import { ChatGateway, RoomGateway } from "./app.gateway";

// 모듈 데코레이터
@Module({
  imports: [
    (...)
  ],
  controllers: [AppController],
  providers: [AppService, ChatGateway, RoomGateway],
})
export class AppModule {}
```

## Step 5 - 클라이언트 파일 생성하기

다음과 같이 클라이언트를 위한 html 파일과 JavaScript 파일을 생성합니다.

```html
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>NestJS Chat</title>
  </head>
  <body>
    <h2>NestJS Chat</h2>

    <!-- 채팅방 목록 -->
    <div>
      <h2>채팅방 목록</h2>
      <ul id="rooms"></ul>
    </div>

    <!-- 채팅 영역 -->
    <div id="chat"></div>

    <!-- 글 입력 영역 -->
    <input type="text" id="message" place="메세지를 입력해 주세요." />

    <!-- [전송 버튼]-->
    <button onClick="sendMessage()">전송</button>

    <!-- [방 만들기] 버튼 -->
    <button onClick="createRoom()">방 만들기</button>

    <!-- 공지 영역 추가 -->
    <div>
      <h2>공지</h2>
      <div id="notice"></div>
    </div>

    <!-- jquery 로드 -->
    <script src="https://code.jquery.com/jquery-3.6.1.slim.js"></script>

    <!-- socket.io 클라이언트 로드 -->
    <script src="http://localhost:3000/socket.io/socket.io.js"></script>

    <!-- 자바스크립트 파일 분리 -->
    <script src="http://localhost:3000/script.js"></script>
  </body>
</html>
```

```javascript
// socket.io 인스턴스 생성
const socket = io("http://localhost:3000/chat"); // 네임스페이스 추가
const roomSocket = io("http://localhost:3000/room"); // 채팅방용 네임스페이스 생성
const nickname = prompt("닉네임을 입력해 주세요."); // 닉네임 입력받기
let currentRoom = ""; // 채팅방 초깃값

// [전송] 버튼 클릭 시 입력된 글을 message 이벤트로 보냄
// eslint-disable-next-line @typescript-eslint/no-unused-vars
function sendMessage() {
  // 선택된 방이 없는 경우 에러
  if (currentRoom === "") {
    alert("방을 선택해 주세요.");
    return;
  }

  const message = $("#message").val();
  const data = { message, nickname, room: currentRoom };
  $("#chat").append(`<div>나: ${message}</div>`); // 내가 보낸 메세지 바로 추가
  // socket.emit('message', { message, nickname }); // 메세지를 보낼 때 닉네임을 같이 전송
  roomSocket.emit("message", data);
  return false;
}

// [방 만들기] 버튼 클릭 시 실행하는 함수
// eslint-disable-next-line @typescript-eslint/no-unused-vars
function createRoom() {
  const room = prompt("생성할 방의 이름을 입력해 주세요.");
  roomSocket.emit("createRoom", { room, nickname });
}

// 방에 들어갈 때 기존에 있던 방에서는 나가기
// eslint-disable-next-line @typescript-eslint/no-unused-vars
function joinRoom(room) {
  // 서버 측의 joinRoom 이벤트를 발생시킴
  roomSocket.emit("joinRoom", { room, nickname, toLeaveRoom: currentRoom });
  $("#chat").html(""); // 채팅방 이동 시 기존 메세지 삭제
  currentRoom = room; // 현재 들어 있는 방의 값을 변경
}

// 채팅방 내에서 대화를 나눌 때 사용하는 이벤트
roomSocket.on("message", (data) => {
  console.log(data);
  $("#chat").append(`<div>${data.message}</div>`);
});

// 클라이언트 측에서 채팅방을 추가하는 함수
roomSocket.on("rooms", (data) => {
  console.log(data);
  $("#rooms").empty(); // 채팅방 갱신 시 일단 리스트를 비움.
  data.forEach((room) => {
    $("#rooms").append(
      `<li>${room} <button onClick="joinRoom('${room}')">join</button></li>`
    );
  });
});

// 서버 접속을 확인을 위한 이벤트
socket.on("connect", () => {
  console.log("connected");
});

// 서버에서 message 이벤트 발생 시 처리
socket.on("message", (message) => {
  $("#chat").append(`<div>${message}</div>`);
});

// notice 이벤트를 받아서 처리
socket.on("notice", (data) => {
  $("#notice").append(`<div>${data.message}</div>`);
});
```

## Step 6 - 테스트 결과

테스트 결과는 다음과 같습니다.

<img src="/assets/img/back-end/nestjs-socket/pic1.png" alt="pic1" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

## 참고 자료

- <a href="https://docs.nestjs.com/websockets/gateways" target="_blank">Gateways | NestJS - A progressive Node.js framework</a>
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

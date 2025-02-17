---
layout: post
title: 챌린지 18일차 학습 정리
description: >
  네이버 부스트캠프 9기 챌린지 18일차 학습 정리 페이지입니다.
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
  - [HTTP Request와 Response](#http-request와-response)
    - [HTTP Request](#http-request)
    - [HTTP Response](#http-response)
  - [TCP/IP](#tcpip)
    - [소켓이란?](#소켓이란)
  - [Echo Server](#echo-server)
    - [Echo Server의 특징](#echo-server의-특징)
    - [Echo Server 사용 사례](#echo-server-사용-사례)
    - [Echo Server 구현하기](#echo-server-구현하기)
  - [Client와 Server](#client와-server)
    - [Client](#client)
    - [Server](#server)
  - [JSON](#json)
    - [JS object와 JSON](#js-object와-json)
- [참고 자료](#참고-자료)
- [Comments](#comments)

## 학습한 내용

- 더 알아볼 내용
  - `Broadcast와 Unicast`
  - `TCP와 UDP`
  - `Socket 관리`

### HTTP Request와 Response

`HTTP` 란 `HyperText Transfer Protocol`의 약자로, 문서를 전송하기 위한 프로토콜을 의미합니다. 즉, 서버와 클라이언트 사이에서 어떻게 메세지를 교환할지를 정해 놓은 규칙입니다.

#### HTTP Request

`HTTP Request` 는 클라이언트(일반적으로 웹 브라우저를 의미함)가 서버에게 정보를 요청할 때 사용하는 메세지입니다. `HTTP Request` 는 다음과 같은 주요 구성 요소로 이루어져 있습니다.

- `요청 메서드(Request Method)` : 클라이언트가 서버에 수행하고자 하는 작업의 유형을 나타냅니다.
  - `GET` : 서버로부터 리소스를 요청합니다. 데이터를 요청할 때 주로 사용됩니다.
  - `POST` : 서버에 데이터를 제출합니다. 폼 제출 등에서 사용됩니다.
  - `PUT` : 서버에 데이터를 업데이트합니다.
  - `DELETE` : 서버에서 리소스를 삭제합니다.
  - `PATCH` : 서버에 데이터를 부분적으로 업데이트합니다.
  - `HEAD` : GET 요청과 동일하지만, 응답 본문을 포함하지 않습니다. 메타 데이터만 필요할 때 사용됩니다. GET 요청으로 반환될 데이터 중 헤더 부분에 해당하는 데이터만 요청합니다.
  - `TRACE` : 서버에 송신한 요청의 내용을 반환해 줄 것을 요청합니다.
  - `CONNECT` : 특정 종류의 프록시 서버에게 연결을 요청합니다.
  - `OPTIONS` : 서버가 지원하는 메서드를 요청합니다.
- `Request URL` : 요청하는 리소스의 주소를 나타냅니다. 이는 프로토콜, 도메인, 경로, 쿼리 문자열로 구성됩니다.
  ```
  https://www.example.com/path/to/resource?query=param
  ```
- `HTTP 버전` : 사용 중인 HTTP의 버전을 나타냅니다.
- `헤더(Headers)` : 추가적인 정보를 제공하는 키-값 쌍의 집합을 의미합니다.
- `본문(Body)` : 요청과 함께 전송되는 데이터를 의미합니다. 주로 `POST` 나 `PUT` 요청에서 사용됩니다.

#### HTTP Response

`HTTP Response` 는 서버가 클라이언트의 요청에 응답할 때 사용하는 메세지입니다. `HTTP Response` 는 다음과 같은 주요 구성 요소로 이루어져 있습니다.

- `상태 라인(Status Line)` : HTTP 버전, 상태 코드, 상태 메세지로 구성됩니다.
  ```
  HTTP/1.1 200 OK
  ```
- `상태 코드(Status Code)` : 요청의 처리 상태를 나타내는 3가지 숫자입니다.
  - `1XX(정보 제공)` : 요청을 받았으며 작업을 진행 중이라는 의미입니다.
  - `2XX(성공)` : 클라이언트의 요청을 수신하여 이해했고 받아들여졌으며, 성공적으로 처리했다는 의미입니다.
  - `3XX(리다이렉션)` : 클라이언트는 요청을 마치기 위해 추가 동작을 취해야 한다는 의미입니다.
  - `4XX(클라이언트 오류)` : 클라이언트의 요청이 올바르지 않다는 의미입니다.
  - `5XX(서버 오류)` : 클라이언트의 요청은 유효하지만 서버가 처리하는 데 실패했다는 의미입니다.
- `헤더(Headers)` : 추가적인 정보를 제공하는 키-값 쌍의 집합을 의미합니다.
- `본문(Body)` : 요청된 리소스의 데이터입니다. HTML, JSON, 이미지 등 다양한 형식이 될 수 있습니다.

### TCP/IP

#### 소켓이란?

외부 컴퓨터와 소통하기 위해 하나의 파일을 열어 외부 통신용으로 사용하는 것. TCP를 위한 구조이다.

소켓은 반드시 `IP 주소`와 `포트 번호`를 가져야하며, 포트번호는 중복되면 안된다.

- IP 주소
  - 네트워크 상의 여러 컴퓨터 중 목적지 컴퓨터를 식별하는데에 쓰이는 식별자.
  - OSI 7계층 중 LAYTER 3에 해당하는 프로토콜이다.
  - IPv4으로 할당가능한 수보다 컴퓨터가 많아지자 IPv6이 등장했다.
  - ifconfig로 이런저런 정보를 볼 수 있다.
  ```bash
  en0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
  	options=6460<TSO4,TSO6,CHANNEL_IO,PARTIAL_CSUM,ZEROINVERT_CSUM>
  	ether 14:7f:ce:93:d2:35
  	**inet6 fe80::b5:9f1f:f6ae:6db4%en0** prefixlen 64 secured scopeid 0xc
  	**inet 172.30.1.100** netmask 0xffffff00 broadcast 172.30.1.255
  	nd6 options=201<PERFORMNUD,DAD>
  	media: autoselect
  	status: active
  ```
- 포트 번호
  - 한 컴퓨터 안의 여러 프로세스 중 목적지가 어디인지 구분하는데 쓰이는 가상의 번호.
  - 22는 ping을 위해 사용되기 때문에 사용하지 않는 것이 좋다.

---

### Echo Server

`에코 서버(Echo Server)` 란 네트워크 통신에서 클라이언트가 보낸 데이터를 그대로 되돌려 전송해 주는 기능을 가진 서버를 말합니다. 에코 서버는 네트워크 프로토콜과 통신을 테스트하거나 디버깅할 때 자주 사용됩니다.

#### Echo Server의 특징

- `데이터 수신` : 클라이언트로부터 데이터를 수신합니다.
- `데이터 반환` : 수신한 데이터를 그대로 클라이언트에게 다시 보냅니다.

#### Echo Server 사용 사례

- `네트워크 디버깅` : 네트워크 연결의 문제를 진단하고 해결하는 데 사용됩니다.
- `통신 테스트` : 클라이언트와 서버 간의 통신이 올바르게 이루어지는지 확인할 때 사용됩니다.
- `교육 목적` : 네트워크 프로그래밍을 배우기 위한 교육 도구로 사용됩니다.

#### Echo Server 구현하기

```jsx
import net from "net";

const PORT = 2024;
const HOST = "127.0.0.1"; // localhost

const server = net.createServer((socket) => {
  console.log("Client connected");

  // Set the encoding to UTF-8
  socket.setEncoding("utf-8");

  // Handle incoming data from the client
  socket.on("data", (data) => {
    console.log(`Received: ${data}`);

    // 클라이언트에게 데이터를 그대로 전송합니다.
    socket.write(`Echo: ${data}`);
  });

  // Handle client disconnection
  socket.on("end", () => {
    console.log("Client disconnected");
  });

  // Handle errors
  socket.on("error", (err) => {
    console.error(`Socket error: ${err.message}`);
  });
});

// Start the server
server.listen(PORT, HOST, () => {
  console.log(`Server listening on ${HOST}:${PORT}`);
});

// Handle server errors
server.on("error", (err) => {
  console.error(`Server error: ${err.message}`);
});
```

위와 같이 클라이언트로부터 데이터를 받아서 그대로 반환하는 TCP 에코 서버를 구현할 수 있습니다. 위의 코드를 설명하자면 다음과 같습니다.

- `net.createServer`
  - 클라이언트 요청시 작업을 수행할 객체를 생성합니다. 이 안에서 클라이언트와 연결된 소켓을 가지고 여러 작업을 할 수 있습니다.
  - `listen` 메서드로 해당 소켓의 IP와 포트를 지정해야합니다.
- `socket.on("data")`
  - 클라이언트로부터 데이터를 수신하는 부분입니다.
- `socket.on(”end”)`
  - 클라이언트 연결 종료 시 실행되는 부분입니다.
- `socket.on(”error”)`
  - 소켓 오류를 처리하는 부분입니다.
- `socket.write()`
  - 클라이언트에게 메세지를 보내는 구간입니다.
  - `socket.setEncoding('utf-8');` 을 통해 한국어를 깨지지 않게 처리할 수 있습니다.
- socket 이벤트 목록
  - [이벤트:`'close'`](https://nodejs.org/api/net.html#event-close_1)
  - [이벤트:`'connect'`](https://nodejs.org/api/net.html#event-connect)
  - [이벤트:`'connectionAttempt'`](https://nodejs.org/api/net.html#event-connectionattempt)
  - [이벤트:`'connectionAttemptFailed'`](https://nodejs.org/api/net.html#event-connectionattemptfailed)
  - [이벤트:`'connectionAttemptTimeout'`](https://nodejs.org/api/net.html#event-connectionattempttimeout)
  - [이벤트:`'data'`](https://nodejs.org/api/net.html#event-data)
  - [이벤트:`'drain'`](https://nodejs.org/api/net.html#event-drain)
  - [이벤트:`'end'`](https://nodejs.org/api/net.html#event-end)
  - [이벤트:`'error'`](https://nodejs.org/api/net.html#event-error_1)
  - [이벤트:`'lookup'`](https://nodejs.org/api/net.html#event-lookup)
  - [이벤트:`'ready'`](https://nodejs.org/api/net.html#event-ready)
  - [이벤트:`'timeout'`](https://nodejs.org/api/net.html#event-timeout)
- `server.listen(PORT, HOST)`
  - 특정 포트 번호를 개방하여 서버를 실행하는 부분입니다.
  - localhost만 사용한다면 HOST를 생략할 수 있습니다.
- `server.on(”error”)`
  - 서버 오류를 처리하는 부분입니다.

### Client와 Server

#### Client

`클라이언트(Client)` 란 서비스를 요청하는 주체로, 일반적으로 사용자가 직접 조작하는 프로그램이나 장치에 해당합니다. 클라이언트는 서버에 특정 작업을 요청하고, 서버가 그 요청을 처리한 후 결과를 반환합니다. 클라이언트의 특징은 다음과 같습니다.

- `요청(Request)` : 클라이언트는 서버에 요청을 보냅니다. 웹 브라우저가 웹 서버에 요청을 보내는 것이 한 예시입니다.
- `응답(Response)` : 서버의 응답을 받습니다. 웹 페이지나 데이터를 수신하는 것과 같습니다.
- `사용자 인터페이스` : 사용자와 직접 상호작용하며, 입력을 받아 서버로 전송합니다.
- `종속성` : 서버가 제공하는 서비스나 데이터를 필요로 합니다.

#### Server

`서버(Server)` 란 클라이언트의 요청을 처리하고 응답을 제공하는 주체입니다. 서버는 여러 클라이언트의 요청을 동시에 처리할 수 있습니다. 서버의 특징은 다음과 같습니다.

- `수신` : 클라이언트로부터 요청을 수신합니다.
- `처리` : 요청을 처리하고 필요한 작업을 수행합니다. 데이터베이스에서 정보를 조회하거나 파일을 제공하는 등의 작업을 수행합니다.
- `응답` : 처리 결과를 클라이언트에게 반환합니다.
- `서비스 제공` : 특정 서비스나 자원을 클라이언트에게 제공합니다. 예를 들어, 웹 페이지를 제공하거나 데이터를 저장하는 등의 작업을 수행합니다.

### JSON

#### JS object와 JSON

- JSON 자체가 JavaScript Object Notation인 만큼 `JSON.parse` 와 `JSON.stringfy` 메서드 두개만 있으면 JS 환경에서 JSON을 쉽게 다룰 수 있다.
- JSON은 value값에 JSON array를 가질 수 있다.
- JSON ↔ String 변환
  ```jsx
  const inputString = '{"a": [1,2,3]}';
  const inputJSON = JSON.parse(input); // { a: [ 1, 2, 3 ] }
  const inputRecover = JSON.stringfy(inputJSON); // '{"a":[1,2,3]}'
  input.replace(" ", "") == inputRecover; // true
  ```
  ```jsx
  // JSON 추가 옵션
  JSON.stringfy(inputJSON, replacer, space)
  - replacer : 몇몇 속성만 골라서 가져오고싶을 때 사용. 모든 속성을 받고싶다면 null을 쓴다.
  - space : 공백 크기를 숫자로 넣거나 공간 채울 문자를 직접 넣어줄 수 있다.
  ```
- 추가
  ```jsx
  inputJSON["b"] = [3,4,5] // { a: [ 1, 2, 3 ], b: [ 3, 4, 5 ] }
  inputJSON.b = [3,4,5] 와 동일한 효과!
  ```
- 삭제
  ```jsx
  delete inputJSON["b"]; // true
  inputJSON; // { a: [ 1, 2, 3 ] }
  // delete 문법은 array에도 활용가능!
  ```
- 순회
  ```jsx
  for (const [key, value] of Object.entries(object1)) {
    console.log(`${key}: ${value}`);
  }
  ```
- 키 유무 확인
  ```jsx
  Object.hasOwnProperty(object1);
  ```
- . 접근법과 [ ] 접근법
  - `.` 접근법은 속성 및 메서드에 접근하는 기본 방식이고..
  - `[]` 접근법은 동적으로 속성을 변경하기 위해 사용하는 기능이라고 한다
  ```jsx
  const read = (key) => {
    let obj = {
      a: 1,
      foo: () => {
        console.log("foo");
      },
    };
    return eval(`obj.${key}`); // 나쁜예시
    return obj[key]; // 적절한 예시
  };
  ```
- 자주하는 실수
  - 작은 따옴표는 사용불가합니다. 큰 따옴표를 사용하세요
  - 문자열을 프로퍼티로 쓸 때 무조건 따옴표로 묶어야합니다.
  - `[]` 로 할당 or 접근시 무조건 문자열을 사용해야합니다. 할당시 Number, 배열 이런거 쓰면 다 문자열로 바뀌어서 들어갑니다.

## 참고 자료

- `Echo Server`
  - https://developer-jun.tistory.com/17
  - https://codingschool.tistory.com/11
- `Client & Server`
  - [https://ko.wikipedia.org/wiki/클라이언트*서버*모델#:~:text=클라이언트(영어%3A client 고객),과 성능을 가지고 있었다](<https://ko.wikipedia.org/wiki/%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8_%EC%84%9C%EB%B2%84_%EB%AA%A8%EB%8D%B8#:~:text=%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8(%EC%98%81%EC%96%B4%3A%20client%20%EA%B3%A0%EA%B0%9D),%EA%B3%BC%20%EC%84%B1%EB%8A%A5%EC%9D%84%20%EA%B0%80%EC%A7%80%EA%B3%A0%20%EC%9E%88%EC%97%88%EB%8B%A4>).
  - https://velog.io/@hahan/클라이언트Client-vs-서버Server
- `JSON`
  - https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/JSON
  - https://velog.io/@minong/Javascript-객체에-해당-key값이-존재하는지-확인하는-방법

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

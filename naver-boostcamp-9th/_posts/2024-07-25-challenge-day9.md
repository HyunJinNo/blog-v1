---
layout: post
title: 챌린지 9일차 학습 정리
description: >
  네이버 부스트캠프 9기 챌린지 9일차 학습 정리 페이지입니다.
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
  - [Design Pattern](#design-pattern)
  - [Observer Pattern](#observer-pattern)
  - [Publisher-Subscriber Pattern](#publisher-subscriber-pattern)
  - [Singleton Pattern](#singleton-pattern)
  - [비동기 프로그래밍](#비동기-프로그래밍)
  - [프로세스 동기화](#프로세스-동기화)
  - [Race Condition(경쟁 상태)](#race-condition경쟁-상태)
  - [Critical Section(임계 영역)](#critical-section임계-영역)
  - [Dead-Lock(교착 상태)](#dead-lock교착-상태)
  - [Callback(콜백)](#callback콜백)
  - [Promise 객체](#promise-객체)
  - [Worker Threads](#worker-threads)
  - [Event Emitter](#event-emitter)
- [Comments](#comments)

## 학습한 내용

### Design Pattern

- 소프트웨어 공학의 소프트웨어 설계에서 공통으로 발생하는 문제에 대해 자주 쓰이는 설계 방법을 정리한 패턴
- 프로그램을 설계할 때 발생했던 문제점들을 객체 간의 상호 관계 등을 이용하여 해결할 수 있도록 하나의 ‘규약’ 형태로 만들어 놓은 것을 의미합니다.
- 디자인 패턴 유형
  - `생성(Creational)`: 객체 인스턴스 생성에 관여, 클래스 정의와 객체 생성 방식을 구조화, 캡슐화를 수행하는 패턴 (Prototype, Factory Method, Singleton, …)
  - `구조(Structural)`: 더 큰 구조 형성 목적으로 클래스나 객체의 조합을 다루는 패턴 (Bridge, Decorator, Facade, …)
  - `행위(Behavioral)`: 클래스나 객체들이 상호 작용하는 방법과 역할 분담을 다루는 패턴 (Iterator, Template Method, Observer, Publisher-Subscriber, …)

### Observer Pattern

- 주체가 어떤 객체의 상태 변화를 관찰하다가 상태 변화가 있을 때마다 메서드 등을 통해 옵저버 목록에 있는 옵저버들에게 변화를 알려주는 디자인 패턴입니다.
- 사용 시기
  - 낮은 결합도가 필요할 때
  - 상호 의존성을 느슨하게 해야 할 때
  - 한 객체에서 상태 변화가 일어날 때 다른 객체의 행동에 변화가 일어나야 할 때
- 특징
  - `Observer`와 `Observable`이 서로의 존재를 알고 있습니다.
  - 주로 `동기적`으로 동작합니다.
  - Observer 패턴은 주로 이벤트 기반 시스템에 사용하며 MVC(Model-View-Controller) 패턴에도 사용됩니다.

### Publisher-Subscriber Pattern

- 사용 시기
  - 상호 의존성을 느슨하게 해야 할 때
- 특징
  - `Publisher`는 `Subscriber`에 직접 메세지를 보내지 않으며 이 둘은 서로의 존재를 알지 못합니다.
  - Observer 패턴과 달리 중간에 `브로커(Event Broker 또는 Event Bus)`가 존재합니다. 브로커는 Publisher와 Subscriber 사이에서 메시지를 필터링하여 다시 배포하는 역할을 수행합니다.
  - `비동기적`으로 동작합니다.

### Singleton Pattern

- 클래스에 하나의 인스턴스만 생성하고 그 인스턴스를 어디에서든 참조할 수 있게 하는 패턴
- 사용 시기
  - 시스템 내에 오직 단 하나의 인스턴스만 필요할 때 (구현 방식에 따라 존재할 수 있는 인스턴스의 수를 제한할 수 있음.)
  - 주로 데이터베이스 연결 모듈에 많이 사용합니다.
- 특징
  - 클래스에서 만들 수 있는 인스턴스의 수를 하나로 제한합니다.
  - 해당 객체에 전역적으로 접근이 가능해 다른 클래스 간에 데이터 공유가 용이합니다.
  - 동일 클래스에 여러 객체를 생성하지 않기 때문에 메모리를 낭비하지 않습니다.
  - 싱글톤 패턴은 미리 생성된 하나의 인스턴스를 기반으로 구현하는 패턴이므로 테스트가 서로 독립적이어야 하는 단위 테스트를 하는 것이 어렵습니다. 즉, TDD(Test-Driven Development)를 진행하기 어렵습니다.

### 비동기 프로그래밍

- 동기(Synchronous)
  - 현재 작업의 응답이 끝남과 동시에 다음 작업이 요청됨
  - 함수를 호출하는 곳에서 호출하는 함수와 결과가 반환될 때까지 기다림
  - 작업 완료 여부를 계속 확인함
- 비동기(Asynchronous)
  - 현재 작업의 응답이 끝나지 않은 상태에서 다음 작업이 요청됨
  - 함수를 호출하는 곳에서 결과를 기다리지 않고, 다른 함수(Callback)에서 결과를 기다림
  - 작업 완료 여부를 확인하지 않음

### 프로세스 동기화

- 프로세스의 실행 순서를 제어해 하나의 자원을 하나의 프로세스가 접근하게 하는 기법
- 동시에 접근할 수 없는 자원에 하나의 프로세스만 접근하게 해 데이터의 일관성을 유지합니다.
- Race Condition에 의해 자원의 일관성이 깨지는 상황을 방지합니다.

### Race Condition(경쟁 상태)

- 여러 프로세스가 공유자원에 동시에 접근할 때 공유자원에 대한 접근 순서에 따라 실행 결과가 달라질 수 있는 상황

### Critical Section(임계 영역)

- 여러 프로세스가 자원을 공유해 둔 상황에서 문제가 발생하지 않도록 독점을 보장해야 하는 영역
- 임계 영역 문제
  - `Mutual Exclustion(상호 배타, 상호 배제)` : 임계 영역에는 하나의 프로세스만 진입해야 한다.
  - `Progress(진행)` : 임계 영역에 접근할 프로세스의 순서를 유한 시간 내에 결정해야 한다.
  - `Bounded Waiting(유한 대기)` : 프로세스는 유한한 시간 내에 임계 영역에 접근할 수 있어야 한다. 즉, 특정 프로세스가 영원히 임계 영역에 들어가지 못하면 안됩니다.
- 임계 영역 문제 해결 방법
  - `Mutex` : 임계 영역을 하나의 프로세스만 접근할 수 있도록 하는 상호 배제 기법. 프로세스나 스레드가 공유 자원을 lock()을 통해 잠금 설정하고 사용한 후에는 unlock()을 통해 잠금 해제합니다.
  - `Semaphore(세마포어)` : 복수의 프로세스가 임계 영역을 접근할 수 있게 하는 상호 배제 기법. 간단한 정수 값과 두 가지 함수 wait(=P 함수) 및 signal(=V 함수)로 공유 자원에 대한 접근을 처리합니다.
    - `wait()`: 자신의 차례가 올 때까지 기다리는 함수
    - `signal()`: 다음 프로세스로 순서를 넘겨주는 함수

### Dead-Lock(교착 상태)

- 교착 상태는 두 개 이상의 프로세스들이 서로가 가진 자원을 기다리며 중단된 상태를 말합니다.
- 교착 상태의 원인
  - `상호 배제`: 한 프로세스가 자원을 독점하고 있으며 다른 프로세스들은 접근이 불가능합니다.
  - `점유 대기`: 특정 프로세스가 점유한 자원을 다른 프로세스가 요청하는 상태입니다.
  - `비선점`: 다른 프로세스의 자원을 강제적으로 가져올 수 없습니다.
  - `환형 대기`: 프로세스 A는 프로세스 B의 자원을 요구하고, 프로세스 B는 프로세스 A의 자원을 요구하는 등 서로가 서로의 자원을 요구하는 상황을 말합니다.
- 교착 상태의 해결 방법
  1. 자원을 할당할 때 애초에 조건이 성립되지 않도록 설계합니다.
  2. 교착 상태 가능성이 없을 때만 자원 할당되며, 프로세스당 요청할 자원들의 최대치를 통해 자원 할당 가능 여부를 파악하는 `은행원 알고리즘` 을 사용합니다.
  3. 교착 상태가 발생하면 사이클이 있는지 찾아보고 이에 관련된 프로세를 한 개씩 지웁니다.
  4. 교착 상태는 매우 드물게 일어나기 때문에 이를 처리하는 비용이 더 커서 교착 상태가 발생하면 사용자가 작업을 종료합니다.

### Callback(콜백)

- 콜백은 실행 가능한 함수를 인자로 전달하여, 특정 상황이 발생할 때 호출되게 하는 방식을 말합니다.
- 콜백은 깊이가 깊어질 수록 코드의 가독성이 떨어지고, 에러 추적이 어려워집니다. **(=콜백 지옥)**

### Promise 객체

- **콜백 지옥(Callback Hell)** 문제를 해결하기 위해 2015년 ES6 버전에 도입되었습니다.
- 자바스크립트에서 `콜백(callback)` 대신 사용할 수 있는 방법으로 비동기 작업이 완료되면 결과를 반환하는 객체를 말합니다.
- 프로미스 객체는 상태를 가지고 있으며 처음에는 `대기 상태`였다가 작업이 완료되면 `성공` 또는 `실패 상태`가 됩니다.
- Promise 객체를 사용한 비동기 처리를 사용하는 방법은 다음과 같이 2가지 방식을 활용할 수 있습니다.
  - `then() & catch()`
    - 체이닝(chaining) 방식을 활용하여 로직을 처리하는 방식
    - 복잡한 로직과 예외 처리를 해야 하는 상황에서 사용하기 어려습니다.
  - `async/await`
    - 자바스크립트에서 도입한 비동기 처리 방식 중 하나입니다.
    - `async` 는 함수 앞에 붙이는 키워드이며, asynchronous(비동기)라는 의미입니다.
    - `async` 가 붙은 함수는 `Promise` 를 반환합니다.
    - `await` 는 `async` 가 붙은 함수 안에서만 사용할 수 있는 키워드로, `Promise` 객체의 실행이 끝나기를 기다립니다.
    - `async/await` 키워드를 사용하면 비동기 코드를 가독성 좋게, 디버깅하기 편하게 작성할 수 있습니다.

| 구분      | callback                                 | promise               | async/await             |
| --------- | ---------------------------------------- | --------------------- | ----------------------- |
| 에러 처리 | 콜백 함수 내에서 처리                    | catch() 메서드로 처리 | try-catch 블록으로 처리 |
| 가독성    | 간단한 경우에는 괜찮으나, 점점 복잡해짐. | 가독성 좋음           | 가독성 좋음             |
| 중첩 처리 | 콜백 함수 내에서 처리                    | then() 메서드를 사용  | await 키워드를 사용     |

### Worker Threads

- Node.js에서 스크립트 연산을 주 실행 스레드와 분리된 별도의 백그라운드 스레드에서 실행할 수 있도록 하는 API
- 하나의 프로세스에서 여러 개의 스레드를 구성하고 각 스레드 별로 하나의 이벤트 루프를 실행
- CPU 집약적인 작업의 퍼포먼스 향상을 목표
- 메인 스레드와 Worker 사이에서 메시지 시스템을 통해 데이터 송수신

```jsx
const { Worker, isMainThread, parentPort } = require('worker_threads');

if (isMainThread) { // 메인 스레드
   const worker = new Worker(__filename); // 같은 dir 폴더에 워커 생성

   worker.on('message', (value) => {
      console.log('워커로부터', value)
   })
   worker.on('exit', (value) => { // parentPort.close()가 일어나면 이벤트 발생
      console.log('워커 끝~');
   })

   worker.postMessage('ping'); // 워커스레드에게 메세지를 보낸다.

} else { // 워커스레드
	// 위에서 생성한 Worker 동작
   parentPort.on('message', (value) => {
      console.log("부모로부터", value);
      parentPort.postMessage('pong');
      parentPort.close(); // 워커스레드 종료라고 메인스레드에 알려줘야 exit이벤트 발생
   })
}
출처: https://inpa.tistory.com/entry/NODE-📚-workerthreads-모듈 [Inpa Dev 👨‍💻:티스토리]
```

### Event Emitter

- Node.js에서 이벤트를 발생시키고 처리하는 데 사용되는 가장 기본적인 이벤트 처리 방식 중 하나
- 이벤트 기반 프로그래밍에서 사용되는 객체
- 장점
  - 이벤트에 여러 개의 리스너를 등록할 수 있다.
  - 다른 객체들과 연동해 사용할 수 있다.
  - Promise에 비해 더욱 유연하다.
  - 비동기 작업의 결과를 처리하는 것 뿐 아니라 이벤트를 사용해 다양한 작업을 처리할 수 있다.
- 단점
  - 이벤트 리스너에 등록된 콜백 함수들이 처리되지 않을 경우 메모리 누수가 발생할 수 있다.
  - 자체적인 동기화 메커니즘이 없다.
  - 비동기 작업을 보다 쉽게 처리하는 최신 기능을 지원하지 않는다. (async/await 지원 X)

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

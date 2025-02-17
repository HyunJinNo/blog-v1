---
layout: post
title: 챌린지 8일차 학습 정리
description: >
  네이버 부스트캠프 9기 챌린지 8일차 학습 정리 페이지입니다.
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
  - [함수형 프로그래밍(Functional Programming)](#함수형-프로그래밍functional-programming)
    - [함수형 프로그래밍의 개념](#함수형-프로그래밍의-개념)
    - [함수형 프로그래밍의 특징](#함수형-프로그래밍의-특징)
  - [부수 효과(Side Effect)](#부수-효과side-effect)
  - [순수 함수(Pure Function)](#순수-함수pure-function)
  - [1급 객체(First-class Object)](#1급-객체first-class-object)
  - [참조 투명성](#참조-투명성)
  - [불변성](#불변성)
    - [자바스크립트에서 불변성을 유지하는 타입](#자바스크립트에서-불변성을-유지하는-타입)
    - [자바스크립트에서 불변성을 유지하지 않는 타입](#자바스크립트에서-불변성을-유지하지-않는-타입)
    - [불변성의 특징](#불변성의-특징)
- [참고 자료](#참고-자료)
- [Comments](#comments)

## 학습한 내용

### 함수형 프로그래밍(Functional Programming)

#### 함수형 프로그래밍의 개념

함수형 프로그래밍이란 어떻게 할 건지(How)를 나타내기보다 무엇(What)을 할 건지를 설명하는 방식인 `선언형 프로그래밍` 패러다임을 따르는, 자료 처리를 수학적 함수의 계산으로 취급하고 상태와 가변 데이터를 멀리하는 프로그래밍 패러다임의 하나이다.

#### 함수형 프로그래밍의 특징

함수형 프로그래밍의 특징을 간략하게 설명하자면 다음과 같습니다.

- **함수가 1등 시민 first-class citizen**
- **함수를 타입으로 지정할 수 있다.**
- **함수를 인자값으로 넘길 수 있다.**
- **함수를 리턴값으로 받을 수 있다.**

요약하자면, 함수형 프로그래밍에서 함수를 객체처럼 변수나 함수의 인자, 리터럴하게 다룰 수 있습니다.

또한 함수형 프로그래밍의 특징을 자세하게 설명하자면 다음과 같습니다.

> **부수 효과**가 없는 **순수 함수**를 **1급 객체**로 간주하여 파라미터나 반환값으로 사용할 수 있으며, **참조 투명성**을 지킬 수 있다.

### 부수 효과(Side Effect)

부수 효과란 다음과 같은 변화 또는 변화가 발생하는 작업을 말합니다.

- 변수값 변경
- 자료 구조를 제자리에서 수정
- 객체의 필드값 변경
- 예외, 오류 발생
- 콘솔 또는 파일 I/O 발생

### 순수 함수(Pure Function)

순수 함수는 입력 값이 동일하면 결과가 동일하게 리턴되는 수학 함수와 마찬가지로 함수형 프로그래밍 언어에서는 함수를 `순수 함수`라고 말한다. 순수 함수는 부수 효과를 제거한 함수에 해당합니다.

- Memory or I/O의 관점에서 Side Effect가 없는 함수
- 함수의 실행이 외부에 영향을 끼치지 않는 함수

### 1급 객체(First-class Object)

1급 객체란 변수나 데이터 구조 안에 담을 수 있으며, 파리미터로 전달할 수 있고, 리턴값으로 사용할 수 있는 객체를 말합니다. 함수형 프로그래밍에서는 함수는 1급 객체에 해당합니다.

### 참조 투명성

참조 투명성이란 컴퓨터 프로그램의 특성 중 하나로, 프로그램 동작의 변경없이 관련 값을 대채할 수 있다면 표현식을 참조 상 투명하다고 말할 수 있다. 즉, 프로그램의 행동에 영향을 받지 않고 직접적인 값으로 대체 가능한 것을 말한다.

순수 함수로 만들면 함수 외부의 값이나 객체를 참조할 때 의존적으로 동작하지 않기 때문에 참조 투명성을 가지고 부작용이 없다. 반대로 말하면 부작용이 있는 함수는 입력 값이 동일해도 함수 외부의 값에 따라서 다른 값이 리턴된다.

### 불변성

불변성이란 객체가 생성된 이후 그 상태를 변경할 수 없는 것을 말하며, 기존의 상태값을 유지하면서 새로운 상태값을 추가하는 것을 의미합니다. 이 때 `상태를 변경한다는 것`은 `값을 재할당하는 것`과 다릅니다.

#### 자바스크립트에서 불변성을 유지하는 타입

- `boolean`
- `number`
- `string`
- `null`
- `undefined`
- `Symbol`

#### 자바스크립트에서 불변성을 유지하지 않는 타입

- `Object`
- `Array`

#### 불변성의 특징

- **장점**
  - 생성자, 접근 메서드에 대한 방어 복사가 필요하지 않습니다.
  - 멀티스레드 환경에서 동기화 처리없이 객체를 공유할 수 있습니다. 이는 불변이기 때문에 thread-safe하여 객체가 안전하기 때문입니다.
- **단점**
  - 객체가 가지는 값마다 새로운 객체가 필요합니다. 따라서 메모리 누수와 새로운 객체를 계속 생성해야 하기 때문에 성능 저하가 일어날 수 있습니다.

## 참고 자료

- [[프로그래밍] 함수형 프로그래밍(Functional Programming) 이란?](https://mangkyu.tistory.com/111)
- [참조 투명성](https://ko.wikipedia.org/wiki/참조_투명성)
- [[오늘의 배움] Referential Transparency란?](https://velog.io/@sangmin7648/Referential-Transparency란)
- [Immutable Class (불변 클래스)](https://velog.io/@jsj3282/Immutable-Class-불변-클래스)
- [[JS] 불변성(Immutability)](https://velog.io/@co_mong/JS-불변성Immutability)
- [객체와 변경불가성(Immutability)](https://poiemaweb.com/js-immutability)
- [불변성이란?](https://velog.io/@rudans987/%EB%B6%88%EB%B3%80%EC%84%B1%EC%9D%B4%EB%9E%80#:~:text=%EC%82%AC%EC%A0%84%EC%A0%81%EC%9C%BC%EB%A1%9C%20%EB%B6%88%EB%B3%80%EC%84%B1%EC%9D%B4%EB%9E%80%20%EA%B0%92,%EC%9D%98%EB%AF%B8%ED%95%A9%E3%84%B4%EB%94%94%E3%85%8F.)

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

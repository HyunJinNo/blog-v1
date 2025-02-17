---
layout: post
title: 챌린지 4일차 학습 정리
description: >
  네이버 부스트캠프 9기 챌린지 4일차 학습 정리 페이지입니다.
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
  - [Stack](#stack)
  - [Heap](#heap)
  - [GVAR/BSS](#gvarbss)
  - [Text (Code)](#text-code)
  - [Stack Pointer](#stack-pointer)
  - [Program Counter (PC)](#program-counter-pc)
- [참고 자료](#참고-자료)
- [Comments](#comments)

## 학습한 내용

<img src="https://upload.wikimedia.org/wikipedia/commons/6/6e/C-memlayout.svg" alt="pic" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"/>

### Stack

- 함수의 호출과 관계되는 지역 변수와 매개변수가 저장되는 영역
- 함수의 호출과 함께 할당되며, 함수의 호출이 완료되면 소멸한다.
  - 함수를 호출할 때마다 지역 변수, 매개변수와 리턴 값 등이 쌓인다.
- 스택 영역은 메모리의 높은 주소에서 낮은 주소 방향으로 할당된다.
- 장점
  - 데이터를 빠르게 액세스할 수 있음. ⇒ [컴파일 타임에 접근할 주소 계산이 가능하기 때문에 주소 기반 접근인건 마찬가지여도 런타임 접근이 힙보다 빠름.](https://stackoverflow.com/questions/24057331/is-accessing-data-in-the-heap-faster-than-from-the-stack)
  - 변수를 명시적으로 할당, 해제할 필요가 없음. ⇒ OS가 직접 메모리를 관리해준다.
  - 메모리 단편화가 발생하지 않는다.
- 단점
  - 스택 크기가 제한되어 있음.
  - 변수의 크기를 조정할 수 없음.

### Heap

- 사용자가 시스템 콜 등을 통해 관리할 수 있는 메모리 영역
  - C처럼 사용자가 일일이 다 할당/미할당을 관리해야할 수도 있고, Java처럼 특정 구문에서 생성된 다음에 Garbage Collector이 자동으로 미할당하는 경우도 있다.
  - 동적으로 할당되는 메모리 공간으로 malloc 이나 new 명령으로 할당한다.
- 힙 영역은 메모리의 낮은 주소에서 높은 주소 방향으로 할당된다. 다만 OS의 할당/미할당 정책 알고리즘에 따라 세부사항은 달라질 수 있다.
- 장점
  - 스레드끼리 공유가 가능하다.
  - 변수를 전역적으로 액세스할 수 있다.
  - 스택 대비 자유롭게 사용자가 메모리 활용이 가능하다.
- 단점
  - 접근이 상대적으로 느리다. ⇒ [런타임 때 동적으로 주소가 결정되기 때문에 어느 주소에 접근할지 미리 계산하고 최적화 하는 것이 힘듦](https://stackoverflow.com/questions/24057331/is-accessing-data-in-the-heap-faster-than-from-the-stack)
  - 사용자가 수동으로 관리해야 하거나 (C) 너무 과하게 사용하지 않고 있는지 의식하고 있어야 한다. (Java 등 자동 할당의 경우)

### GVAR/BSS

- 범위가 정해지지 않는 전역 변수를 포함하는 영역
- GVAR는 초깃값을 0이 아닌 특정한 값으로 지정한 경우 사용함.
- Data 영역이 GVAR(Data)와 BSS 영역으로 나누어진다.
  - `BSS`: 초기화되지 않는 데이터가 저장됨.
  - `GVAR`: 초기화된 데이터가 저장됨.

### Text (Code)

- 프로그램에 있는 함수 코드, 제어문, 상수 등을 포함하는 영역
- 실행할 프로그램의 코드가 저장되는 영역으로, CPU가 해당 영역에 저장된 명령어를 하나씩 가져가서 처리한다. 위치 파악은 PC로 한다.
- 일반적으로 한 번 로딩하면 바뀌지 않는다.
  - JVM에서는 TEXT 영역을 사용하지 않는다.
  - Node.js나 브라우저에서는 TEXT 영역 대신 코드 영역이 별도로 존재한다.

### Stack Pointer

- 현재 실행되고 있는 함수와 관련된 스택 영역이 어디인지 가리키는 포인터.
- 레지스터에 보관. OS가 이를 참고해 접근
- 왜 스택은 힙이랑 다르게 절대 주소를 사용하지 않고, SP의 상대적인 위치를 기반으로 접근을 하는가?
  - 함수 호출에 따라 동적으로 변하는 자료구조이기 때문
    - 재귀 형태의 함수의 경우, 본인들의 스택 시작 위치를 주소 값으로 저장하면 매우 비효율적. 어차피 현재 실행되고 있는 함수의 SP만 알면 된다. 함수 동작에 필요한 지역변수, 그리고 끝났을 때의 반환 위치 등을 저장하는 곳이니까.
- 함수 호출 시 본인이 사용할 스택 영역을 먼저 확보하고, 함수 반환시 본인이 사용했던 스택 영역을 반환하며 이를 SP의 위치를 조절하는 형태로 구현한다. (컴파일러가 어셈블리를 만들 때 추가)

### Program Counter (PC)

- 현재 실행되고 있는 코드의 **다음** 실행될 주소의 위치를 가리킴
  - [현재 위치가 아닌 그 다음 위치를 가리키는 이유는 아키텍처 구조랑 어셈블러/링커의 동작 방식이랑 관련이 있다.](https://stackoverflow.com/questions/24091566/why-does-the-arm-pc-register-point-to-the-instruction-after-the-next-one-to-be-e)
- 과제에서도 코드의 다음 실행될 주소의 위치를 가리키게 할지는 고민해봐야 할 듯.

## 참고 자료

- [스택(Stack)과 힙(Heap) 차이점](https://junghyun100.github.io/%ED%9E%99-%EC%8A%A4%ED%83%9D%EC%B0%A8%EC%9D%B4%EC%A0%90/)
- [운영체제 메모리 구조](https://devfunny.tistory.com/650)
- [Why does the ARM PC register point to the instruction after the next one to be executed?](https://stackoverflow.com/questions/24091566/why-does-the-arm-pc-register-point-to-the-instruction-after-the-next-one-to-be-e)
- [Is accessing data in the heap faster than from the stack?](https://stackoverflow.com/questions/24057331/is-accessing-data-in-the-heap-faster-than-from-the-stack)

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

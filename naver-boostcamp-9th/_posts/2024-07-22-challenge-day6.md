---
layout: post
title: 챌린지 6일차 학습 정리
description: >
  네이버 부스트캠프 9기 챌린지 6일차 학습 정리 페이지입니다.
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
  - [클래스(Class)](#클래스class)
  - [객체(Object)](#객체object)
  - [인스턴스(Instance)](#인스턴스instance)
  - [객체지향 프로그래밍(OOP, Object-Oriented Proramming)](#객체지향-프로그래밍oop-object-oriented-proramming)
  - [TypeScript - Class](#typescript---class)
  - [TypeScript - Interface](#typescript---interface)
  - [TypeScript - Abstract class](#typescript---abstract-class)
- [Comments](#comments)

## 학습한 내용

### 클래스(Class)

클래스는 객체지향 프로그래밍에서 특정 객체를 생성하기 위해 변수와 메서드를 정의하는 일종의 틀(Template)을 말합니다. 클래스는 객체를 정의하기 위한 메서드와 변수로 구성됩니다.

### 객체(Object)

객체란 객체지향 프로그래밍의 가장 기본적인 단위입니다. 책상, 의자, 시계 등 우리가 주변에서 흔히 볼 수 있는 **모든 실재(實在)하는 대상**을 객체로 표현합니다. 객체는 `속성`과 `기능`을 가지고 있으며 이것은 주로 각각 `변수`와 `함수`라고 불립니다.

### 인스턴스(Instance)

인스턴스란 객체지향 프로그래밍에서 클래스에 소속된 개별적인 객체를 의미합니다.

### 객체지향 프로그래밍(OOP, Object-Oriented Proramming)

**객체지향 프로그래밍**이란 객체들의 집합으로 프로그램의 상호 작용을 표현하며 데이터를 객체로 취급하여 객체 내부에 선언된 메서드를 활용하는 방식을 말합니다.

객체지향 프로그래밍의 장점은 다음과 같습니다.

- 객체 지향적 설계를 통해서 프로그램을 보다 유연하고 변경이 용이하게 만들 수 있습니다.
- 각각의 부품들이 각자의 독립적인 역할을 수행하기 때문에 코드의 변경을 최소화하고 유지보수를 하는 데 유리합니다.
- 객체지향 프로그래밍에서 객체는 현실 세계에 존재하는 특정 대상에 해당되기 때문에 보다 인간 친화적이고 직관적인 코드를 작성할 수 있습니다.

객체지향 프로그래밍은 다음과 같이 4가지 특징이 있습니다.

- **추상화(Abstraction)**
  추상화란 복잡한 시스템으로부터 핵심적인 개념 또는 기능을 간추려내는 것을 의미합니다.
- **캡슐화(Encapsulation)**
  캡슐화란 객체의 속성과 메서드를 하나로 묶고 일부를 외부에 감추어 은닉하는 것을 말합니다.
  - **정보 은닉(Information Hiding)**
    정보 은닉이란 프로그램의 세부 구현을 외부로 드러나지 않도록 특정 모듈 내부로 감추는 것을 말합니다. 내부의 구현은 감추고 모듈 내에서의 응집도를 높이며, 외부로의 노출을 최소화하여 모듈 간의 결합도를 떨어뜨려 유연함과 유지보수성을 높일 수 있습니다.
    일반적으로 다음과 같이 3가지의 접근 제한자가 존재합니다.
    - `public`: 클래스의 외부에서 사용 가능하도록 노출시키는 것.
    - `protected`: 다른 클래스에게는 노출되지 않지만, 상속받은 자식 클래스에게는 노출되는 것.
    - `private`: 클래스의 내부에서만 사용되며 외부로 노출시키지 않는 것.
- **상속(Inheritance)**
  상속이란 상위 클래스의 속성과 기을 하위 클래스가 이어받아서 재사용하거나 추가, 확장하는 것을 말합니다. 상속은 코드의 재사용 측면, 계층적인 관계 형성, 유지 보수성 측면에서 중요합니다.
- **다형성(Polymorphism)**
  다형성은 하나의 메서드나 클래스가 다양한 방법으로 동작하는 것을 말합니다. 다형성을 대표하는 것으로는 다음과 같이 `오버로딩`, `오버라이딩`이 존재합니다.
  - **오버로딩(Overloading)**
    오버로딩은 같은 이름을 가진 메서드를 여러 개 두는 것을 말합니다. 메서드의 타입, 매개변수의 유형, 개수 등으로 여러 개를 둘 수 있으며 컴파일 중에 발생하는 `정적 다형성`입니다.
  - **오버라이딩(Overriding)**
    오버라이딩은 주로 `메서드 오버라이딩(Method Overriding)`을 말하며 상위 클래스로부터 상속받은 메서드를 하위 클래스에서 재정의하는 것을 말합니다. 오버라이딩은 런타임 중에 발생하는 `동적 다형성`입니다.

### TypeScript - Class

타입스크립트에서 클래스는 다음과 같이 사용할 수 있습니다.

```typescript
class Example extends SuperClass {
  num1: number;
  protected num2: number;
  private num3: number;

  constructor(num1: number, num2: number, num3: number, readonly num4: number) {
    this.num1 = num1;
    this.num2 = num2;
    this.num3 = num3;
  }
}
```

```typescript
class Example implements IExample {
  (...생략)
}
```

위의 코드를 설명하자면 다음과 같습니다.

- `extends`: 클래스를 상속받을 때 사용합니다.
- `implements`: 인터페이스를 구현할 때 사용합니다.
- 접근 제한자
  - `public`: `public` 키워드를 붙이거나 아무 것도 없는 경우에 해당.
  - `protected`
  - `private`
- 생성자 내에서 `readonly` 키워드를 사용하면 자동으로 클래스 내의 속성 값으로 선언됩니다.

### TypeScript - Interface

타입스크립트에서 인터페이스는 다음과 같이 사용할 수 있습니다.

```typescript
interface Example {
  num: number;
  name: string;
}
```

인터페이스 내에 선언된 모든 값과 함수들은 `public`으로 고정됩니다.

### TypeScript - Abstract class

타입스크립트에서 추상 클래스는 다음과 같이 사용할 수 있습니다.

```typescript
abstract class Example {
  (...생략)

  abstract work(): void;
}
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

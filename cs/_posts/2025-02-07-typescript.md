---
layout: post
title: 타입스크립트 (TypeScript) (작성 중)
description: >
  타입스크립트(TypeScript)에 대해 정리한 페이지입니다.
image:
  path: /assets/img/cs/study.jpg
related_posts:
  - None
sitemap: true
comments: false
---

<i>last modified at: 2025. 02. 12.</i>

<h2>목차</h2>

- [개요](#개요)
- [타입스크립트란?](#타입스크립트란)
- [타입스크립트의 이점](#타입스크립트의-이점)
- [타입스크립트 개발 환경 구축 방법](#타입스크립트-개발-환경-구축-방법)
- [타입스크립트 문법](#타입스크립트-문법)
  - [변수 선언](#변수-선언)
  - [타입 애너테이션](#타입-애너테이션)
  - [기본 타입](#기본-타입)
  - [배열과 튜플](#배열과-튜플)
  - [any](#any)
  - [unknown](#unknown)
  - [void](#void)
  - [never](#never)
  - [Union](#union)
  - [Intersection](#intersection)
  - [타입 가드](#타입-가드)
    - [typeof 연산자](#typeof-연산자)
    - [instanceof 연산자](#instanceof-연산자)
    - [in 연산자](#in-연산자)
    - [is - 사용자 정의 타입 가드](#is---사용자-정의-타입-가드)
    - [Non-null assertion 연산자](#non-null-assertion-연산자)
  - [타입 단언](#타입-단언)
    - [\<타입\> 문법](#타입-문법)
    - [as 문법](#as-문법)
  - [타입 별칭](#타입-별칭)
  - [리터럴 타입](#리터럴-타입)
  - [인터페이스](#인터페이스)
  - [클래스](#클래스)
    - [implements](#implements)
    - [abstract](#abstract)
    - [접근 제한자](#접근-제한자)
  - [readonly](#readonly)
  - [유틸리티 타입](#유틸리티-타입)
    - [Partial](#partial)
    - [Required](#required)
    - [Record](#record)
    - [Omit](#omit)
    - [Pick](#pick)
    - [Exclude](#exclude)
    - [ReturnType](#returntype)
    - [Parameters](#parameters)
    - [Readonly](#readonly-1)
  - [데코레이터](#데코레이터)
  - [satisfies](#satisfies)
  - [enum](#enum)
  - [타입 추론](#타입-추론)
- [참고 자료](#참고-자료)
- [Comments](#comments)

## 개요

<hr />

`타입스크립트(TypeScript)`에 대해 정리한 페이지입니다.

<b>타입스크립트 개념이 방대하므로 지속적으로 업데이트하는 중입니다.</b>

## 타입스크립트란?

<hr />

`타입스크립트(TypeScript)`란 자바스크립트에 타입을 추가한 언어입니다. 타입스크립트는 타입을 지정하지 않고 인터프리터가 타입을 유추하는 `동적 타입` 언어인 자바스크립트와 달리, 타입이 같지 않다면 컴파일 에러를 내는 `강 타입` 언어입니다. 확장자로는 `.ts`를 사용하며, 자바스크립트로 컴파일(트랜스파일)되는 언어입니다.

> `동적 타입`: 자바스크립트처럼 타입을 지정하지 않고 인터프리터가 타입을 유추하는 언어로, <b>런타임 시 타입이 결정</b>됩니다.  
> `정적 타입`: <b>컴파일 시 타입이 결정</b>되는 언어

> `약 타입`: <b>비슷한 타입 간에는 자동으로 변환</b>하는 언어 (Ex. Java - float 형 변수에 int 변수를 할당하는 경우 컴파일 에러가 나지 않고, 컴파일러가 내부적으로 float로 변경)  
> `강 타입`: <b>타입이 같지 않다면 컴파일 에러</b>를 내는 언어

## 타입스크립트의 이점

<hr />

타입스크립트를 사용하면 다음과 같은 이점이 있습니다.

- <b>컴파일 시점에 에러 확인</b>

  타입스크립트를 사용하면 런타임 시점 이전인 컴파일 시점에 에러를 확인할 수 있습니다.

  예를 들어 다음과 같이 두 수의 합을 반환하는 자바스크립트 코드가 있다고 가정합니다.

  ```javascript
  function add(a, b) {
    return a + b;
  }

  console.log(add(10, "20")); // "1020"
  ```

  자바스크립트 코드는 타입을 지정하지 않기 때문에 매개변수로 전달하는 값이 숫자인지 문자열인지 검사하지 않습니다. 원래 의도라면 `10 + 20 = 30`이었겠지만 실제로는 두 개의 문자열 "10"과 "20"을 합친 `"1020"`가 결과로 반환됩니다.

  반면에 다음과 같이 타입스크립트는 매개변수의 타입을 지정합니다.

  ```typescript
  function add(a: number, b: number) {
    return a + b;
  }

  console.log(add(10, "20")); // Argument of type 'string' is not assignable to parameter of type 'number'.ts(2345)
  ```

  따라서 해당 타입스크립트 코드 컴파일 시점에 컴파일 에러가 발생하기 때문에 오류를 쉽게 잡아낼 수 있습니다.

- <b>개발 도구(IDE나 컴파일러 등) 피드백</b>

  타입스크립트 코드 작성 시에 IDE에서 함수 정의와 함수 매개변수 타입을 알고 있기 때문에 <b>코드 자동 완성, 잘못된 변수/함수 사용에 대한 에러 알림</b>과 같은 피드백을 받을 수 있습니다. 이를 통해 순수 자바스크립트를 사용하는 것에 비해 생산성이 크게 향상됩니다.

## 타입스크립트 개발 환경 구축 방법

<hr />

터미널에서 다음 명령어를 입력하여 타입스크립트를 전역으로 설치합니다.

```bash
npm install -g typescript
```

이후 타입스크립트가 잘 설치되었는지 확인하기 위해 다음 명령어를 입력합니다.

```bash
tsc --version
```

<img src="/assets/img/cs/typescript/pic1.png" alt="pic1" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

타입스크립트를 실행할 폴더에서 tsconfig.json 파일을 생성합니다. `npx tsc --init` 명령어를 통해 tsconfig.json 파일을 초기화하여 생성할 수 있습니다.

<img src="/assets/img/cs/typescript/pic2.png" alt="pic2" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

tsconfig.json 파일이 생성되면 다음과 같은 타입스크립트 코드를 자바스크립트 코드로 컴파일할 수 있습니다.

```typescript
// test.ts

function add(a: number, b: number) {
  return a + b;
}

console.log(add(10, 20));
```

```bash
tsc test.ts
```

<img src="/assets/img/cs/typescript/pic3.png" alt="pic3" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

위의 방식은 한 번에 하나의 타입스크립트 파일을 컴파일하는 방식입니다. 만약 특정 폴더 내의 모든 타입스크립트 파일들을 컴파일하고 싶다면 다음과 같이 tsconfig.json의 `include` 옵션에 폴더를 지정할 수 있습니다. 또한 `outDir` 옵션에 컴파일 결과 폴더를 지정할 수 있습니다.

```json
{
  "compilerOptions": {
    // ...

    "outDir": "./dist"

    // ...
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

이후 다음 명령어를 입력하여 특정 폴더 내의 모든 타입스크립트 파일들을 컴파일할 수 있습니다.

```bash
tsc
```

<img src="/assets/img/cs/typescript/pic4.png" alt="pic4" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

위의 방식들은 모두 타입스크립트 파일을 자바스크립트 파일로 컴파일해 자바스크립트 파일을 실행하는 방식입니다. `ts-node` 패키지를 설치하면 컴파일 과정없이 바로 타입스크립트 코드를 실행할 수 있습니다. (참고로 `ts-node`도 내부적으로는 타입스크립트를 자바스크립트로 변환 후 실행합니다.)

```bash
npm install -g ts-node
```

이후 다음과 같이 타입스크립트 코드를 실행할 수 있습니다.

<img src="/assets/img/cs/typescript/pic5.png" alt="pic5" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

## 타입스크립트 문법

<hr />

### 변수 선언

자바스크립트와 마찬가지로 `var`, `let`, `const` 3가지 키워드로 변수를 선언할 수 있습니다.

```typescript
var a = 1;
let b = 2;
const c = 3;
```

### 타입 애너테이션

`타입 애너테이션(Type Annotation)`은 타입을 명시하는 방법을 의미합니다.

먼저 변수의 경우 다음과 같이 `[변수명]: [타입]` 형식으로 타입을 지정할 수 있습니다.

```typescript
const a: number = 1;
const b: string = "2";
const c: boolean = true;
```

함수의 경우 매개변수는 `[변수명]: [타입]` 형식으로, 반환값은 매개변수 뒤에 타입을 명시하는 방식으로 지정합니다.

```typescript
function add(a: number, b: number): number {
  return a + b;
}
```

객체의 경우 다음과 같이 타입을 지정할 수 있습니다.

```typescript
const student: { name: string; age: number; height: number } = {
  name: "HyunJinNo",
  age: 27,
  height: 182.8,
};
```

변수명 뒤에 ?를 붙여서 `선택적 속성(Optional Property)`으로 만들 수 있습니다.

```typescript
const student: {
  name: string;
  age: number;
  height: number;
  gender?: string; // 선택적 속성
} = {
  name: "HyunJinNo";
  age: 27;
  height: 182.8;
};

function add(a: number, b: number, c?: number): number {
  if (c) {
    return a + b + c;
  }

  return a + b;
}
```

### 기본 타입

타입스크립트는 7가지 기본 자료형을 갖고 있습니다.

| 타입      | 설명                                                                                            |
| --------- | ----------------------------------------------------------------------------------------------- |
| null      | null은 의도적으로 값이 없음을 나타낼 때 사용합니다.                                             |
| undefined | undefined는 변수에 값이 할당되지 않은 경우를 뜻합니다. 즉 의도하지 않은 값의 부재를 의미합니다. |
| boolean   | 참(true)과 거짓(false) 두 가지 값을 가지고 있는 타입입니다.                                     |
| string    | 문자열 타입을 나타냅니다.                                                                       |
| number    | 숫자를 나타내는 타입입니다. 정수, 부동 소수점, 2진수, 8진수, 16진수도 숫자 타입에 해당합니다.   |
| bigint    | 매우 큰 숫자도 할당할 수 있는 타입입니다. 숫자 뒤에 n을 붙여서 123n으로 표시합니다.             |
| symbol    | 불변이면서 유니크한 값을 표현하는 자료형입니다.                                                 |

```typescript
const myNull: null = null;
const myUndefined: undefined = undefined;
const myBoolean: boolean = true;
const myString: string = "string";
const myNumber: number = 1;
const myBigint: bigint = 123n;
const mySymbol: symbol = Symbol("mySymbol");
```

### 배열과 튜플

배열과 튜플은 여러 개의 데이터를 저장한다면 점에서 비슷하지만, 배열은 크기가 고정이 아니며, 튜플은 크기가 고정된다는 점이 다릅니다.

배열은 각 원소의 타입을 정의하며, 다음 두 가지 방법으로 타입을 선언할 수 있습니다.

```typescript
const arr1: number[] = [1, 2, 3];
const arr2: Array<number> = [1, 2, 3];
```

튜플은 배열과 달리 원소 개수만큼 타입을 정의해야 합니다. 튜플은 다음 방식으로 타입을 선언할 수 있습니다.

```typescript
const tuple: [string, number] = ["string", 123];
```

### any

`any` 타입은 `unknown` 타입을 제외하고 가장 상위에 있는 타입입니다. `any`는 타입을 모르거나 지정할 수 없을 때 유용할 수 있습니다. 보통 서드파티 라이브러리에서 코드에 타입이 없는 경우 `any`를 사용해 컴파일이 안되는 문제를 해결할 수 있습니다. `any`로 타입을 선언하는 경우 컴파일 시점이 아닌 런타임 시점에 에러가 날 가능성이 높으므로 `any` 타입 사용은 가능하면 지양하는 것이 좋습니다.

```typescript
let anyValue = 10;
anyValue = "Hello, World!";
anyValue = true;
```

### unknown

`any`와 같이 최상위 타입인 `unknown` 타입은 알 수 없는 타입을 의미합니다. `any`와 같이 `unknown`에는 어떤 타입의 값도 할당할 수 있지만, `unknown` 타입은 `any` 타입 외의 어떤 타입에도 할당할 수 없습니다. `unknown` 타입을 `any` 타입 외에 어떤 타입에 할당하려면 `타입 단언`이나 `타입 가드`가 필요하므로 `any` 타입보다 더 안전합니다.

```typescript
let a: any = 123;
let b: unknown = 123;

let c: number = a;
let d: number = b as number;
let e: number = b; // Error: Type 'unknown' is not assignable to type 'number'.ts(2322)
```

### void

`void`는 함수의 반환값에 지정하는 타입으로, 함수의 반환값이 없는 경우(실제로는 `undefined`를 반환)에 사용합니다.

```typescript
function sayHello(): void {
  console.log("Hello, World!");
}
```

### never

`never`는 함수의 반환값에 지정하는 타입으로, 의도적으로 값을 반환하지 않는 때 사용합니다. 예외를 발생시키는 함수이거나 무한 루프를 실행하는 함수가 이에 해당합니다.

```typescript
function throwError(message: string): never {
  throw new Error(message);
}
```

### Union

`유니온(Union)`은 2개 이상의 타입을 허용하는 것을 의미합니다. `|(Vertical Bar)` 기호를 사용하여 타입을 구분해 여러 타입을 정의합니다.

```typescript
let anyValue: number | string | boolean = 10;
anyValue = "Hello, World!";
anyValue = true;
```

### Intersection

`&`를 사용해 2개 이상의 타입을 조합하는 경우를 `Intersection`이라고 합니다. 여러 개의 타입 중 하나만 만족하면 되는 `Union`과 달리, `Intersection`은 조합한 모든 타입을 만족해야 합니다. `Union`이 `or`에 해당한다면, `Intersection`은 `and`에 해당합니다.

```typescript
type Cup = { size: number };
type Brand = { brandName: string };
type BrandedCup = Cup & Brand;

let brandedCup: BrandedCup = {
  size: 100,
  brandName: "BrandName",
};
```

`Intersection`을 잘못 사용하면 다음과 같이 값을 할당할 수 없는 타입을 만드는 실수를 할 수 있습니다. `Intersection`을 사용할 때 주의해야 합니다.

```typescript
type Impossible = number & string;
let impossible: Impossible = 10; // Error: Type '10' is not assignable to type 'never'.ts(2322)
```

### 타입 가드

`Type Narrowing`이란 타입의 범위를 좁히는 작업을 의미합니다. 타입의 범위를 좁히는 데 사용하는 검사 방법을 `타입 가드(Type Guard)`라 하며, 값 할당 또는 조건문으로 검사해 타입의 범위를 좁힙니다. 타입 가드를 선언하는 방법으로 `typeof`, `instanceof`, `in` 등이 있습니다.

#### typeof 연산자

`typeof` 연산자를 사용하면 원시 타입(`string`, `number`, `boolean`, `symbol`, `bigint`, `function`, `object`, `undefined`)를 구별할 수 있습니다. 단, `typeof`는 객체 타입(`object`, `array`, `null`)을 구별하기 어렵습니다.

```typescript
function printLength(value: string | number) {
  if (typeof value === "string") {
    console.log(value.length);
  } else {
    console.log(value.toFixed(2));
  }
}
```

#### instanceof 연산자

`instanceof` 연산자를 사용하면 객체가 특정 클래스의 인스턴스인지 확인할 수 있습니다. 단, `instanceof`는 클래스 기반 객체에서만 사용 가능하며, 인터페이스나 일반 객체에는 사용할 수 없습니다.

```typescript
class Cat {
  meow() {
    console.log("meow");
  }
}

class Dog {
  bark() {
    console.log("bark");
  }
}

function makeSound(animal: Cat | Dog) {
  if (animal instanceof Cat) {
    animal.meow();
  } else {
    animal.bark();
  }
}
```

#### in 연산자

`in` 연산자를 사용하면 객체가 특정 속성을 가지고 있는지 확인할 수 있습니다. `in` 연산자는 인터페이스, 객체 리터럴, 클래스 등 여러 곳에서 활용할 수 있습니다.

```typescript
type Person = { name: string; age: number };
type Robot = { model: string; battery: number };

function printInfo(entity: Person | Robot) {
  if ("name" in entity) {
    console.log(entity.name);
  } else {
    console.log(entity.model);
  }
}
```

#### is - 사용자 정의 타입 가드

`is` 키워드는 `사용자 정의 타입 가드(User-Defined Type Guards)`를 정의하는데 사용됩니다. 함수를 활용하여 타입 검사를 직접 수행하고 특정 타입임을 보장하는 방식으로, 더 복잡한 조건에서도 `Type Narrowing`을 적용할 수 있습니다.

```typescript
type Person = { name: string };
type Robot = { model: string };
type Entity = Person | Robot;

function isPerson(entity: Entity): entity is Person {
  return (entity as Person).name !== undefined;
}

function PrintInfo(entity: Entity) {
  if (isPerson(entity)) {
    console.log(entity.name);
  } else {
    console.log(entity.model);
  }
}

const person: Person = { name: "HyunJinNo" };
PrintInfo(person); // "HyunJinNo"
```

#### Non-null assertion 연산자

`!`를 사용하는 Non-null assertion 연산자를 사용하여 피연산자의 값이 `null`이나 `undefined`가 아님을 단언할 수 있습니다.

```typescript
function printLength1(str: string | undefined | null) {
  console.log(str.length); // Error: 'str' is possibly 'null' or 'undefined'.ts(18049)
}

function printLength2(str: string | undefined | null) {
  console.log(str!.length);
}
```

### 타입 단언

`타입 단언(Type Assertion)`은 컴파일러에게 특정 값의 타입을 알려주는 것을 말합니다. 타입을 강제 변환하는 것이 아니라, 컴파일러가 타입을 추론하는 방식에 개입하는 역할을 합니다.

타입 단언에는 두 가지 문법이 존재합니다.

#### <타입> 문법

`<타입>` 형식으로 타입 단언을 선언할 수 있습니다. 다만 해당 방식은 JSX 문법과 충돌할 수 있으며, 제네릭(Generic)과 헷갈릴 수 있으므로 사용이 지양됩니다.

```typescript
let value: any = "Hello, World!";
let strLength: number = (<string>value).length;
```

#### as 문법

`as` 문법을 통해 타입 단언을 선언할 수 있습니다.

```typescript
let value: any = "Hello, World!";
let strLength: number = (value as string).length;
```

### 타입 별칭

`type` 키워드를 사용하여 타입 별칭을 만들 수 있습니다.

```typescript
type Length = number | string;

let len: Length = 10;
len = "10km";
```

### 리터럴 타입

`리터럴 타입(Literal Type)`이란 기본 타입의 값들을 조합해서 한정적인 값들만 나타내는 타입입니다. 다음과 같이 `size`가 `10`, `20`, `30`만 있다고 가정할 때 `10`, `20`, `30`만 허용하는 리터럴 타입을 정의할 수 있습니다.

```typescript
type Size = 10 | 20 | 30;

let size1: Size = 10;
let size2: Size = 20;
let size3: Size = 30;
let size4: Size = 40; // Error: Type '40' is not assignable to type 'Size'.ts(2322)
```

### 인터페이스

`인터페이스(Interface)`는 `타입 별칭`보다 더 읽기 쉬운 오류 메세지, 더 빠른 컴파일러 성능, 클래스와 함께 사용할 수 있는 장점을 제공합니다.

`type`과 `interface`를 비교하면 다음과 같습니다.

| type                                                        | interface                                   |
| ----------------------------------------------------------- | ------------------------------------------- |
| 기존 타입 또는 새 타입을 생성하는데 사용                    | 객체 타입의 구조를 정의하는데 사용          |
| 다른 타입 또는 인터페이스를 상속하거나 구현할 수 없음       | 다른 인터페이스를 상속하거나 구현할 수 있음 |
| 리터럴 타입, Union 타입, Intersection 타입을 사용할 수 있음 | `extends` 키워드로 인터페이스 확장이 가능함 |
| 간단한 타입 별칭을 생성할 때 적합                           | 잘 정의된 구조의 객체 타입을 정의할 때 적합 |

`type`과 `interface`를 사용한 예시는 다음과 같습니다.

```typescript
type PersonType = { name: string; age: number };

interface Person {
  name: string;
  age: number;
}

let personType: PersonType = { name: "HyunJinNo", age: 27 };

let person: Person = { name: "HyunJinNo", age: 27 };
```

다음과 같이 `extends` 키워드를 사용하여 인터페이스 확장이 가능합니다.

```typescript
interface Person {
  name: string;
  age: number;
}

interface Man extends Person {
  height: number;
}

const man: Man = {
  name: "HyunJinNo",
  age: 27,
  height: 182.8,
};
```

### 클래스

`class` 키워드를 사용하여 클래스를 정의할 수 있습니다.

```typescript
class Person {
  constructor(private firstName: string, private lastName: string) {}

  getName() {
    return `${this.firstName} ${this.lastName}`;
  }
}

const person = new Person("HyunJin", "No");
console.log(person.getName()); // "HyunJin No"
```

위의 코드에서 `constructor(private firstName: string, private lastName: string) {}`는 다음 코드와 동일합니다.

```typescript
class Person {
  private firstName: string;
  private lastName: string;

  constructor(firstName: string, lastName: string) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  getName() {
    return `${this.firstName} ${this.lastName}`;
  }
}

const person = new Person("HyunJin", "No");
console.log(person.getName()); // "HyunJin No"
```

#### implements

`implements` 키워드를 사용하여 인터페이스를 구현할 수 있습니다.

```typescript
interface IPerson {
  firstName: string;
  lastName: string;
  getName: () => string;
}

class Person implements IPerson {
  constructor(public firstName: string, public lastName: string) {}

  getName() {
    return `${this.firstName} ${this.lastName}`;
  }
}

const person = new Person("HyunJin", "No");
console.log(person.getName()); // "HyunJin No"
```

#### abstract

`abstract` 키워드를 사용하여 추상 클래스를 정의할 수 있습니다.

```typescript
interface IPerson {
  firstName: string;
  lastName: string;
  getName: () => string;
}

abstract class Person implements IPerson {
  constructor(public firstName: string, public lastName: string) {}

  abstract getName(): string;
}

class Man extends Person {
  getName() {
    return `${this.firstName} ${this.lastName}`;
  }
}

const man = new Man("HyunJin", "No");
console.log(man.getName()); // "HyunJin No"
```

#### 접근 제한자

타입스크립트에는 클래스 내에 변수와 메서드에서 사용할 수 있는 `접근 제한자(Access Modifiers)`들이 있습니다. 접근 제한자가 있는 변수와 메서드는 클래스 외부에서 해당 클래스의 구현을 공개할지 여부를 결정합니다. 접근 제한자는 타입스크립트의 기능으로, 자바스크립트로 컴파일된 후에는 제거됩니다.

타입스크립트에서 사용할 수 있는 접근 제한자의 종류는 다음과 같습니다.

| 접근 제한자 | 설명                                         |
| ----------- | -------------------------------------------- |
| `public`    | 모든 곳에서 접근 가능(기본값)                |
| `protected` | 클래스 내부 또는 자식 클래스에서만 접근 가능 |
| `private`   | 클래스 내부에서만 접근 가능                  |

### readonly

`readonly` 키워드를 사용하면 처음 속성의 값이 결정되면 이후에는 변경할 수 없는 `읽기 전용 속성`을 정의할 수 있습니다.

```typescript
interface Person {
  readonly name: string;
  age: number;
}

let person: Person = { name: "HyunJinNo", age: 27 };

person.age = 28;
person.name = "HyunJin"; // Error: Cannot assign to 'name' because it is a read-only property.ts(2540)
```

### 유틸리티 타입

`유틸리티 타입(Utility Types)`은 기본 타입을 변환하거나 조작하는 데 사용할 수 있는 타입입니다. 유틸리티 타입을 사용하면 간결한 방식으로 타입을 정의하거나 이미 정의된 타입을 변환할 수 있습니다.

#### Partial

`Partial`을 사용하면 객체 내 모든 프로퍼티를 `선택적 속성(Optional Property)`으로 변경할 수 있습니다.

```typescript
interface Point {
  x: number;
  y: number;
}

const point: Partial<Point> = { x: 1 };
console.log(point.x); // 1
console.log(point.y); // undefined
```

#### Required

`Required`를 사용하면 객체 내 모든 프로퍼티를 필수 속성으로 변경할 수 있습니다.

```typescript
interface Point {
  x: number;
  y?: number;
}

const point: Required<Point> = { x: 1, y: 2 };
console.log(point.x); // 1
console.log(point.y); // 2
```

#### Record

`Record`를 사용하면 특정 key 타입과 value 타입을 갖는 객체 타입을 정의할 수 있습니다.

```typescript
// Record<string, number> === { [key: string]: number }
const peopleAgeInfo: Record<string, number> = {
  Alice: 21,
  Bob: 22,
};
```

#### Omit

`Omit`을 사용하면 한 객체 타입에서 특정 프로퍼티를 제외한 새로운 타입을 정의할 수 있습니다.

```typescript
interface Person {
  name: string;
  age: number;
  location: string;
  gender: string;
}

const person: Omit<Person, "location" | "gender"> = {
  name: "HyunJinNo",
  age: 27,
};
```

#### Pick

`Pick`을 사용하면 한 객체 타입에서 특정 프로퍼티만 존재하는 객체 타입을 정의할 수 있습니다.

```typescript
interface Person {
  name: string;
  age: number;
  location: string;
  gender: string;
}

const person: Pick<Person, "name" | "age"> = {
  name: "HyunJinNo",
  age: 27,
};
```

#### Exclude

`Exclude`를 사용하면 `Union` 타입에서 특정 타입을 제외한 새로운 `Union` 타입을 정의할 수 있습니다.

```typescript
type Type1 = 10 | 20 | 30;
type Type2 = Exclude<Type1, 20>; // type Type2 = 10 | 30;

type Type3 = string | number | boolean;
let value: Exclude<Type3, number> = "string";
value = true;
```

#### ReturnType

`ReturnType`을 사용하면 특정 함수의 반환 타입으로 구성된 타입을 정의할 수 있습니다.

```typescript
const myFunc = () => {
  return "a";
};

type MyType = ReturnType<typeof myFunc>;
const value1: MyType = "a";
const value2: ReturnType<typeof myFunc> = "b";
```

#### Parameters

`Parameters`를 사용하면 함수의 매개변수 타입들의 튜플 타입을 정의할 수 있습니다.

```typescript
type MyType1 = (s: string, flag: boolean) => void;
type MyType2 = (num: number) => number;

type MyType3 = Parameters<MyType1>;
const value: MyType3 = ["string", true];
```

#### Readonly

`Readonly`를 사용하면 객체 내 모든 프로퍼티를 `읽기 전용(readonly)`으로 변경할 수 있습니다.

```typescript
interface Person {
  name: string;
  age: number;
}

const person: Readonly<Person> = {
  name: "HyunJinNo",
  age: 27,
};

person.name = "HyunJin"; // Error: Cannot assign to 'name' because it is a read-only property.ts(2540)
```

### 데코레이터

// TODO

### satisfies

// TODO

### enum

// TODO

### 타입 추론

// TODO

## 참고 자료

<hr />

- <a href="https://velog.io/@jee/%EA%B0%95%ED%83%80%EC%9E%85%EA%B3%BC-%EC%95%BD%ED%83%80%EC%9E%85-%ED%98%B9%EC%9D%80-%EC%A0%95%EC%A0%81%ED%83%80%EC%9E%85%EA%B3%BC-%EB%8F%99%EC%A0%81%ED%83%80%EC%9E%85" target="_blank">강타입과 약타입 혹은 정적타입과 동적타입</a>
- <a href="https://www.heropy.dev/p/WhqSC8" target="_blank">한눈에 보는 타입스크립트 | HEROPY.DEV</a>
- <a href="https://www.yes24.com/Product/Goods/118379776" target="_blank">Node.js 백엔드 개발자 되기 - 예스24</a>
- <a href="https://www.w3schools.com/typescript/index.php" target="_blank">TypeScript Tutorial</a>
- <a href="https://devwiki.co.kr/post/typescript-is" target="_blank">TypeScript "is" Keyword (User-Defined Type Guards)</a>
- <a href="https://typescript-kr.github.io/" target="_blank">소개 · GitBook</a>

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

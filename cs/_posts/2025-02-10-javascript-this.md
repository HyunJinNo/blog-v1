---
layout: post
title: 자바스크립트 this
description: >
  자바스크립트 this에 대해 정리한 페이지입니다.
image:
  path: /assets/img/cs/study.jpg
related_posts:
  - None
sitemap: true
comments: false
---

<i>last modified at: 2025. 02. 11.</i>

<h2>목차</h2>

- [개요](#개요)
- [자바스크립트에서 this란?](#자바스크립트에서-this란)
- [동적으로 결정되는 this](#동적으로-결정되는-this)
  - [전역 공간에서의 this](#전역-공간에서의-this)
  - [일반 함수에서의 this](#일반-함수에서의-this)
  - [객체 메서드에서의 this](#객체-메서드에서의-this)
  - [화살표 함수에서의 this](#화살표-함수에서의-this)
  - [콜백 함수에서의 this](#콜백-함수에서의-this)
  - [생성자 함수에서의 this](#생성자-함수에서의-this)
- [this 바인딩](#this-바인딩)
  - [call 메서드](#call-메서드)
  - [apply 메서드](#apply-메서드)
  - [bind 메서드](#bind-메서드)
- [참고 자료](#참고-자료)
- [Comments](#comments)

## 개요

<hr />

자바스크립트 this에 대해 정리한 페이지입니다.

## 자바스크립트에서 this란?

<hr />

대부분의 객체지향 언어에서 `this`는 클래스로 생성한 인스턴스 객체를 의미하지만, 자바스크립트에서는 `this`를 어디서든 사용할 수 있습니다.

자바스크립트에서 `this`는 함수 내부에서 자기 자신이 속한 객체를 참조하는 키워드로, 기본적으로 실행 컨텍스트가 생성될 때 함께 결정됩니다. 실행 컨텍스트는 함수를 호출할 때 생성된다는 점을 고려하면, `this`는 <b>함수를 호출할 때 결정</b>된다는 것을 알 수 있습니다.

## 동적으로 결정되는 this

<hr />

자바스크립트에서 `this`는 정적으로 결정되지 않고, 런타임에 함수 호출 방식에 따라 this에 바인딩할 객체가 동적으로 결정됩니다.

### 전역 공간에서의 this

전역 공간에서 this는 전역 객체를 가리킵니다. 브라우저에서는 `window` 객체와 연결되고 Node.js에서는 `global` 객체와 연결됩니다. `globalThis`를 사용하면 코드가 실행 중인 현재 컨텍스트와 관계없이 항상 전역 객체를 얻을 수 있습니다.

<br />

<b>브라우저</b>

<img src="/assets/img/cs/javascript-this/pic1.png" alt="pic1" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

<br />

<b>Node.js</b>

<img src="/assets/img/cs/javascript-this/pic2.png" alt="pic2" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

<br />

위의 코드들을 보면 알 수 있듯이 <b>전역 변수를 선언하면 자바스크립트 엔진은 이를 전역 객체의 프로퍼티로 할당</b>합니다. 이는 <b>자바스크립트의 모든 변수는 실제로는 특정 객체의 프로퍼티로 동작</b>하기 때문입니다. 여기서 특정 객체란 실행 컨텍스트에 `LexicalEnvironment`에 해당합니다. 전역 실행 컨텍스트의 경우 LexicalEnvironment는 전역 객체를 그대로 참조합니다.

### 일반 함수에서의 this

어떤 함수가 어떤 객체에도 속하지 않는 경우 일반 함수로 동작합니다. 일반 함수로서 호출되는 경우 `this`는 `전역 객체`를 가리킵니다. 이는 실행 컨텍스트 활성화 당시에 this가 지정되지 않은 경우 this는 전역 객체를 가리키기 때문입니다.

<img src="/assets/img/cs/javascript-this/pic3.png" alt="pic3" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

주의해야 할 점으로, 어떤 함수를 객체의 프로퍼티에 할당하는 경우 무조건 메서드로서 동작하는 것은 아닙니다. 아래와 같이 <b>함수 이름 앞에 객체가 명시되어 있는 경우에만 메서드로서 동작하는 것이고, 그렇지 않은 경우 모두 일반 함수로서 동작합니다.</b>

```javascript
const myFunc = function () {
  console.log(this);
};

const obj = {
  myMethod: myFunc,
};

myFunc(); // 일반 함수로서 호출

console.log();

obj.myMethod(); // 메서드로서 호출
```

<img src="/assets/img/cs/javascript-this/pic4.png" alt="pic4" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

### 객체 메서드에서의 this

객체의 메서드로서 호출하는 경우, <b>메서드를 호출한 객체가 this에 바인딩</b>됩니다.

```javascript
const person = {
  name: "HyunJinNo",
  sayHello() {
    console.log(`Hello, My name is ${this.name}.`);
  },
};

person.sayHello(); // "Hello, My name is HyunJinNo."
```

[일반 함수에서의 this](#일반-함수에서의-this)에서 언급하였듯이, 함수가 객체에 속해있더라도 항상 메서드로서 호출되는 것은 아닙니다. 아래 코드와 같이 다른 변수에 할당하여 단독으로 호출하는 경우 일반 함수로서 동작합니다.

```javascript
const person = {
  name: "HyunJinNo",
  sayHello() {
    console.log(`Hello, My name is ${this.name}.`);
  },
};

const sayHello = person.sayHello;
sayHello(); // "Hello, My name is undefined."
```

### 화살표 함수에서의 this

ES6에서 도입된 `화살표 함수(Arrow Function)`는 `this`를 바인딩하지 않습니다. 화살표 함수는 실행 컨텍스트를 생성할 때 this 바인딩 과정 자체가 없어서, 상위 스코프(LexicalEnvironment)의 this를 그대로 활용할 수 있습니다.

```javascript
const person = {
  name: "HyunJinNo",
  sayHello1() {
    console.log(`Hello, My name is ${this.name}.`);
  },
  sayHello2: () => {
    console.log(`Hello, My name is ${this.name}.`);
  },
};

person.sayHello1(); // "Hello, My name is HyunJinNo."
person.sayHello2(); // "Hello, My name is undefined."
```

위의 코드와 같이 상위 스코프의 this가 전역 객체인 경우 undefined가 출력될 수 있습니다. 따라서 객체의 메서드를 정의할 때는 `화살표 함수` 대신 `일반 함수`를 사용하는 것이 좋습니다.

### 콜백 함수에서의 this

<b>콜백 함수에서의 this는 콜백 함수의 제어권을 가지는 함수(메서드)가 결정</b>합니다. 아래 코드와 같이 특별히 정의하지 않는 경우에는 기본적으로 전역 객체를 가리킵니다.

```javascript
setTimeout(function () {
  console.log(this); // 전역 객체
}, 1000);

[1, 2, 3, 4, 5].forEach(function (value) {
  console.log(this, value); // 전역 객체
});
```

반면에 아래와 같이 DOM 이벤트 핸들러 함수에서의 this는 기본적으로 이벤트가 발생한 DOM 요소를 가리킵니다.

```javascript
const button = document.getElementById("myButton");
button.addEventListener("click", function () {
  console.log(this); // 클릭된 button 요소
});
```

### 생성자 함수에서의 this

생성자 함수는 어떤 공통된 성질을 지니는 객체들을 생성하는 데 사용하는 함수입니다. 객체지향 언어에서는 생성자를 `클래스(class)`, 클래스를 통해 만든 객체를 `인스턴스(instance)`라고 합니다.

자바스크립트는 함수에 생성자 역할을 부여했으며, `new` 연산자와 함께 함수를 호출하면 해당 함수가 생성자로서 동작하게 됩니다.

생성자 함수(또는 클래스의 생성자) 내부에서의 this는 <b>`new` 연산자에 의해 새로 생성된 인스턴스를 가리킵니다.</b> 즉, 어떤 함수가 생성자 함수로서 호출된 경우 내부에서의 this는 새로 생성된 인스턴스 자신이 됩니다.

```javascript
function Person(name) {
  this.name = name;
  console.log(this);
}

const person1 = Person("Person 1"); // 전역 객체

const person2 = new Person("Person 2"); // Person { name: 'Person 2' }
console.log(person2.name); // "Person 2"
```

## this 바인딩

<hr />

`call`, `apply`, `bind` 메서드들을 사용하면 <b>함수 호출 시 명시적으로 `this`를 지정</b>할 수 있습니다.

### call 메서드

`call` 메서드는 첫 번째 인자로 this로 사용할 객체를 전달하고, 나머지 인자들을 콤마로 구분해 전달하여 함수 또는 메서드를 호출할 수 있습니다. 아래 코드와 같이 함수를 그냥 실행하면 this는 전역 객체를 가리키지만, `call` 메서드를 사용하면 임의의 객체를 this로 지정할 수 있습니다.

```javascript
function myFunc(a, b, c) {
  console.log(this, a, b, c);
}

myFunc(1, 2, 3); // 전역 객체 1 2 3
myFunc.call({ x: 1 }, 1, 2, 3); // { x: 1 } 1 2 3
```

또한 아래 코드와 같이 객체 메서드를 그냥 실행하면 this는 메서드를 호출한 객체가 바인딩되지만, `call` 메서드를 사용하면 임의의 객체를 this로 지정할 수 있습니다.

```javascript
const obj = {
  a: 1,
  myFunc(b, c) {
    console.log(this.a, b, c);
  },
};

obj.myFunc(2, 3); // 1 2 3
obj.myFunc.call({ a: 10 }, 2, 3); // 10 2 3
```

### apply 메서드

`apply` 메서드는 `call` 메서드와 기능적으로 완전히 동일합니다. 차이점이 있다면, `call` 메서드는 첫 번째 인자를 제외한 나머지 인자들을 콤마로 구분해 전달한다면, `apply` 메서드는 첫 번째 인자를 제외한 나머지 인자들을 배열로 전달한다는 차이점이 있습니다.

```javascript
function myFunc(a, b, c) {
  console.log(this, a, b, c);
}

myFunc(1, 2, 3); // 전역 객체 1 2 3
myFunc.apply({ x: 1 }, [1, 2, 3]); // { x: 1 } 1 2 3

const obj = {
  a: 1,
  myFunc(b, c) {
    console.log(this.a, b, c);
  },
};

obj.myFunc(2, 3); // 1 2 3
obj.myFunc.apply({ a: 10 }, [2, 3]); // 10 2 3
```

### bind 메서드

`bind` 메서드는 `call` 메서드와 비슷하지만, 함수 또는 메서드를 즉시 호출하는 것이 아니라 this 및 함수에 넘길 인자들을 일부 지정해서 새로운 함수를 반환하는 메서드입니다.

```javascript
function myFunc(a, b, c) {
  console.log(this, a, b, c);
}

const newFunc1 = myFunc.bind({ x: 1 }, 10);
newFunc1(2, 3); // { x: 1 } 10 2 3
newFunc1(1, 2, 3); // // { x: 1 } 10 1 2

const newFunc2 = myFunc.bind({ x: 2 });
newFunc2(2, 3); // { x: 2 } 2 3 undefined
newFunc2(1, 2, 3); // { x: 2 } 1 2 3
```

`bind` 메서드를 사용하여 새로 만든 함수는 다음 코드와 같이 `name` 프로퍼티에 동사 bind의 수동태인 `bound`라는 접두어가 붙는 특징이 있습니다. 어떤 함수의 `name` 프로퍼티가 `bound xxx`라면 해당 함수는 `bind` 메서드를 적용한 새로운 함수라는 의미이므로 기존의 `call` 메서드나 `apply` 메서드보다 코드를 추적하기 쉽습니다.

```javascript
function myFunc(a, b, c) {
  console.log(this, a, b, c);
}

const newFunc1 = myFunc.bind({ x: 1 }, 10);
const newFunc2 = myFunc.bind({ x: 2 });

console.log(myFunc.name); // myFunc
console.log(newFunc1.name); // bound myFunc
console.log(newFunc2.name); // bound myFunc
```

## 참고 자료

<hr />

- <a href="https://www.yes24.com/Product/Goods/78586788" target="_blank">코어 자바스크립트 - 예스 24</a>
- <a href="https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-this-%EC%B4%9D%EC%A0%95%EB%A6%AC#%ED%95%A8%EC%88%98_%ED%98%B8%EC%B6%9C_%EB%B0%A9%EC%8B%9D%EA%B3%BC_this_%EB%B0%94%EC%9D%B8%EB%94%A9" target="_blank">[JS] 📚 자바스크립트 this 💯 완전 정복</a>
- <a href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/this" target="_blank">this - JavaScript | MDN</a>

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

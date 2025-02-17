---
layout: post
title: 정규 표현식 (Regular Expression)
description: >
  정규 표현식에 대해 설명하는 페이지입니다.
image:
  path: /assets/img/cs/cs.png
related_posts:
  - None
sitemap: true
comments: false
---

<h2>목차</h2>

- [정규 표현식(Regular Expression)이란?](#정규-표현식regular-expression이란)
- [정규 표현식 문법](#정규-표현식-문법)
  - [문자 클래스](#문자-클래스)
  - [메타 문자](#메타-문자)
  - [이스케이프 시퀀스](#이스케이프-시퀀스)
  - [플래그 (Flag)](#플래그-flag)
  - [정규 표현식 예제](#정규-표현식-예제)
- [자바스크립트 정규 표현식](#자바스크립트-정규-표현식)
  - [정규 표현식 생성](#정규-표현식-생성)
  - [정규 표현식 메서드](#정규-표현식-메서드)
    - [test](#test)
    - [exec](#exec)
  - [문자열 메서드](#문자열-메서드)
    - [match](#match)
    - [matchAll](#matchall)
    - [replace](#replace)
    - [split](#split)
    - [search](#search)
- [참고 자료](#참고-자료)
- [Comments](#comments)

## 정규 표현식(Regular Expression)이란?

`정규 표현식(Regular Expression, RegEx 또는  RegExp)`이란 문자열에서 특정 패턴을 찾거나, 치환하거나, 추출하는 데 사용되는 일종의 형식 언어를 말합니다. 주로 텍스트 검색, 텍스트 처리, 데이터 검증 등에 사용됩니다.

## 정규 표현식 문법

### 문자 클래스

문자 클래스는 대괄호 `[ ]` 안에 포함된 문자들 중 하나와 일치하는 패턴을 정의합니다. 다음은 문자 클래스를 사용하는 예시입니다.

- `[abc]`: `a`, `b`, 또는 `c` 중 하나와 일치합니다.
- `[a-z]`: 소문자 알파벳 중 하나와 일치합니다.
- `[0-9]`: 숫자 `0`부터 `9`까지 중 하나와 일치합니다.
- `[^abc]`: `a`, `b`, `c`를 제외한 모든 문자와 일치합니다. `^`가 대괄호 안에서 사용될 경우 `부정(not)`을 의미합니다.

### 메타 문자

| 메타 문자 | 의미                                     |
| --------- | ---------------------------------------- |
| ^         | 특정 문자열로 시작                       |
| $         | 특정 문자열로 종료                       |
| .         | 개행 문자를 제외한 모든 단일 문자와 일치 |
| \*        | 앞의 패턴이 0번 이상 반복                |
| +         | 앞의 패턴이 1번 이상 반복                |
| ?         | 앞의 패턴이 0번 또는 1번 나타납니다.     |
| {n}       | 앞의 패턴이 n번 반복                     |
| {n,}      | 앞의 패턴이 n번 이상 반복                |
| {n,m}     | 앞의 패턴이 n번 이상, m번 이하로 반복    |
| abc\|def  | or 연산자로, 둘 중 하나의 패턴과 일치    |
| ()        | 소괄호 안의 패턴을 그룹화                |

### 이스케이프 시퀀스

`이스케이스 시퀀스(Espace Sequences)`란 정규 표현식에서 기본적으로 정의된 문자 집합을 의미합니다. 이스케이스 시퀀스는 백슬래시(\\)로 시작합니다.

| 이스케이프 시퀀스 | 의미                                         |
| ----------------- | -------------------------------------------- |
| \\d               | 모든 숫자와 일치                             |
| \\D               | 숫자가 아닌 모든 문자와 일치                 |
| \\w               | 영문자, 숫자, 밑줄과 일치                    |
| \\W               | 영문자, 숫자, 밑줄이 아닌 문자와 일치        |
| \\s               | 모든 공백 문자(스페이스, 탭, 개행 등)와 일치 |
| \\S               | 공백 문자가 아닌 문자와 일치                 |
| \\n               | 개행 문자와 일치                             |
| \\0               | 8진수 값과 일치                              |
| \\x               | 16진수 값과 일치                             |

### 플래그 (Flag)

플래그는 정규 표현식을 사용할 때 고급 검색을 위한 전역 옵션을 설정하는 기능입니다. 플래그는 패턴 구분자가 끝나면 그 뒤에 쓰는 것으로, 패턴을 일괄적으로 변경합니다. **플래그는 각 프로그래밍 언어마다 사용 법이 상이하다는 점을 주의해야 합니다.**

| Flag | 의미        | 설명                                                                     |
| ---- | ----------- | ------------------------------------------------------------------------ |
| i    | Ignore Case | 대소문자를 구별하지 않고 검사                                            |
| g    | Global      | 문자열 내의 모든 패턴을 검사 (전역 검사), 패턴을 문자열에서 여러 번 검색 |
| m    | Multi Line  | 문자열의 행이 바뀌더라도 계속 검사                                       |
| s    | dotAll      | 메타 문자가 줄 바꿈 문자('\n')도 포함하도록 설정                         |
| u    | Unicode     | 유니코드 전체를 지원                                                     |
| y    | Sticky      | 문자 내 특정 위치에서 검색을 진행하는 "Sticky" 모드를 활성화             |

### 정규 표현식 예제

- `^[0-9]*$`: 숫자
- `^[a-zA-Z]\*$`: 영문자
- `/^[a-z]*$/i`: 영문자
- `^[가-힣]*$`: 현대 한글
- `^[ㄱ-ㅎㅏ-ㅣ가-힣]*$`: 한글 자모 낱자를 포함한 모든 현대 한글
- `^[a-zA-Z0-9]*$`: 영문/숫자
- `^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$`: 이메일
- `^\d{3}-\d{3,4}-\d{4}$`: 전화번호

## 자바스크립트 정규 표현식

### 정규 표현식 생성

자바스크립트에서 정규 표현식은 두 가지 방법으로 만들 수 있습니다.

- `정규 표현식 리터럴`
  ```javascript
  const re = /ab+c/;
  ```
- `RegExp 객체의 생성자 호출`
  ```javascript
  const re = new RegExp("ab+c");
  ```

### 정규 표현식 메서드

#### test

`test` 메서드는 패턴이 문자열과 일치하는 지 확인하고, 일치하면 true, 일치하지 않는 경우 false를 반환합니다.

```javascript
const regex = /hello/i;
const result = regex.test("Hello World");
console.log(result); // true
```

#### exec

`exec` 메서드는 패턴이 문자열과 일치할 때, 일치하는 배열을 반환하며, 일치하지 않으면 `null`을 반환합니다. 전역 플래그('g')를 사용하면 `exec` 메서드로 반복 호출을 통해 문자열의 모든 일치 항목을 찾을 수 있습니다.

```javascript
const regex = /world/;
const result = regex.exec("Hello World");
console.log(result[0]); // 'World'
```

### 문자열 메서드

#### match

`match` 메서드는 문자열에서 정규 표현식과 일치하는 부분을 찾습니다.

```javascript
const str = "The rain in Spain stays mainly in the plain";
const result = str.match(/ain/g);
console.log(result); // ['ain', 'ain', 'ain']
```

#### matchAll

`matchAll` 메서드는 전역 일치를 반환하며, 모든 결과를 반복 가능(iterable)한 객체로 반환합니다.

```javascript
const str = "The rain in Spain stays mainly in the plain";
const result = str.matchAll(/ain/g);
console.log([...result]); // [['ain'], ['ain'], ['ain']]
```

#### replace

`replace` 메서드는 정규 표현식을 사용해 문자열의 일부를 다른 문자열로 치환합니다.

```javascript
const str = "The quick brown fox jumps over the lazy dog.";
const result = str.replace(/dog/, "cat");
console.log(result); // 'The quick brown fox jumps over the lazy cat.'
```

#### split

`split` 메서드는 정규 표현식을 사용해 문자열을 분할하여 배열로 반환합니다.

```javascript
const str = "The quick brown fox jumps over the lazy dog.";
const result = str.replace(/dog/, "cat");
console.log(result); // 'The quick brown fox jumps over the lazy cat.'
```

#### search

`search` 메서드는 문자열에서 정규 표현식과 일치하는 첫 번째 부분의 인덱스를 반환합니다. 일치하는 부분이 없는 경우 `-1`을 반환합니다.

```javascript
const str = "The quick brown fox jumps over the lazy dog.";
const index = str.search(/fox/);
console.log(index); // 16
```

## 참고 자료

- [https://namu.wiki/w/%EC%A0%95%EA%B7%9C%20%ED%91%9C%ED%98%84%EC%8B%9D](https://namu.wiki/w/%EC%A0%95%EA%B7%9C%20%ED%91%9C%ED%98%84%EC%8B%9D)
- [https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EC%A0%95%EA%B7%9C%EC%8B%9D-RegExp-%EB%88%84%EA%B5%AC%EB%82%98-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-%EC%89%BD%EA%B2%8C-%EC%A0%95%EB%A6%AC#%EC%A0%95%EA%B7%9C%EC%8B%9D\_%EA%B5%AC%EC%84%B1](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EC%A0%95%EA%B7%9C%EC%8B%9D-RegExp-%EB%88%84%EA%B5%AC%EB%82%98-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-%EC%89%BD%EA%B2%8C-%EC%A0%95%EB%A6%AC#%EC%A0%95%EA%B7%9C%EC%8B%9D_%EA%B5%AC%EC%84%B1)
- [https://regexr.com/](https://regexr.com/)
- [https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Regular_expressions](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Regular_expressions)

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

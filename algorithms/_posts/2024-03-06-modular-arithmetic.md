---
layout: post
title: Modular Arithmetic
description: >
  Modular Arithmetic 알고리즘에 대해 설명하는 페이지입니다.
image: 
  path: /assets/img/algorithms/computer.jpg
  srcset:
    1060w: /assets/img/algorithms/computer.jpg
    530w:  /assets/img/algorithms/computer.jpg
    265w:  /assets/img/algorithms/computer.jpg
related_posts:
  - None
sitemap: true
comments: false
---
<i>Environment</i> 
- <i>OS: Windows 11</i>

## 목차
- [목차](#목차)
- [Introduction](#introduction)
- [How to Use](#how-to-use)
- [Examples](#examples)
- [Comments](#comments)

## Introduction
- **Definition**
  - **모듈라 연산(Modular Arithmetic)** 은 모듈로(Modulus) M에 도달하면 다시 0으로 돌아가는 정수들을 가지고 하는 연산을 말한다.

## How to Use
- **모듈라 덧셈, 뺄셈, 곱셈**
    ```java
    // 덧셈
    (a + b) % M = ((a % M) + (b % M)) % M
    ```
    ```java
    //뺄셈
    (a - b) % M = ((a % M) - (b % M)) % M
    ```
    ```java
    // 곱셈
    (a x b) % M = ((a % M) x (b % M)) % M
    ```
- **모듈라 나눗셈**
  - 모듈라 연산에서의 나눗셈 (a / b) 는 b로 나누는 대신 b의 **곱셈 역원(Multiplicative inverse)** 을 곱하는 방식으로 이루어진다.
  - b의 곱셈 역원은 항상 존재하는 것이 아니라, **b가 M과 서로소**일 때만 존재한다.
  - 프로그래밍 대회에서 출제되는 대부분의 문제에서는 M이 소수라는 사실을 고려하자.
    ```java
    // 나눗셈
    modInv(b, M) = b^(M - 2) % M
    (a / b) % M = (a x modInv(b, M)) % M
    ```

## Examples
- <a href="https://github.com/HyunJinNo/Algorithm/tree/main/%EB%B0%B1%EC%A4%80/Gold/11401.%E2%80%85%EC%9D%B4%ED%95%AD%E2%80%85%EA%B3%84%EC%88%98%E2%80%853" target="_blank">11401. 이항 계수 3</a>

<br />
<br />
<br />

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
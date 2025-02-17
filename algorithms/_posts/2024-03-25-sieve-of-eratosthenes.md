---
layout: post
title: Sieve of Eratosthenes
description: >
  Sieve of Eratosthenes 알고리즘에 대해 설명하는 페이지입니다.
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
  - **에라토스테네스의 체(Sieve of Eratosthenes)** 는 주어진 수 n 이하의 모든 소수를 찾아내는데 자주 사용되는 방법이다.

## How to Use
1. 먼저 2부터 n까지의 수를 전부 한 목록에 쓴다.
2. 그 다음에 이 목록에서 지워지지 않은 수들을 순회하며 각 수의 배수를 지우는 과정을 반복한다. 이 때 지워지지 않은 수를 찾을 때 n이 아니라 sqrt(n)까지만 찾는다.
3. i의 배수들을 모두 지울 때 2 x i 에서 시작하는 것이 아니라 i x i 부터 시작하면 계산하는데 시간을 단축할 수 있다.
4. 이와 같은 과정을 반복하고 나면 결과적으로 남은 수들은 모두 소수가 된다.

## Examples
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/%EB%B0%B1%EC%A4%80/Silver%20III/1929.%E2%80%85%EC%86%8C%EC%88%98%E2%80%85%EA%B5%AC%ED%95%98%EA%B8%B0/%EC%86%8C%EC%88%98%E2%80%85%EA%B5%AC%ED%95%98%EA%B8%B0.kt" target="_blank">소수 구하기.kt</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Number%20Theory/Sieve%20of%20Eratosthenes/Sieve_of_Eratosthenes.js" target="_blank">비트마스크를 사용하는 에라토스테네스의 체</a>

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
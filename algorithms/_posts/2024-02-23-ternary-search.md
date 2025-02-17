---
layout: post
title: Ternary Search
description: >
  Ternary Search 알고리즘에 대해 설명하는 페이지입니다.
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
- [Examples](#examples)
- [Comments](#comments)

<br>

## Introduction
- **Definition**
  - **삼분 탐색(Ternary search)** 은 **이분법**이 매 반복마다 답의 후보 구간을 절반으로 잘라 각 위치에서 함수의 값을 계산하는 것과 비슷하게, **답의 후보 구간을 삼등분하는 두 위치에서 함수의 값을 계산하는 탐색 방식이다.**
  - 삼분 탐색은 **볼록 함수(Convex function) 또는 오목 함수(Concave function)** 에서 **극값, 또는 최대/최소 값**을 찾을 때 유용하게 사용되는 기법이다.
  - 삼분 탐색은 한 번 반복할 때마다 **후보 구간의 크기를 2/3**로 줄여나간다.
  - 그래프를 갖는 함수의 최대점을 찾는 문제는 여러 가지 방법으로 풀 수 있다. 함수를 **직접 미분**하거나, **국소 탐색 알고리즘** 등으로 해결할 수 있다. 그러나 **삼분 탐색은 미분할 수 없는 함수에도 사용할 수 있으며, 국소 탐색에 비해 훨씬 빠르게 동작하고 수렴 판정이 용이**하기 때문에 더 자주 사용된다.
    > **국소 탐색(Local search)** 은 임의의 답을 하나 만들어 놓고 이 값을 조금씩 갱신하면서 답이 더 좋아지는 쪽으로 움직이는 알고리즘이다.

## Examples
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Ternary%20search/FOSSIL.java" target="_blank">FOSSIL</a>
- <a href="https://github.com/HyunJinNo/Algorithm/tree/main/%EB%B0%B1%EC%A4%80/Gold/11664.%E2%80%85%EC%84%A0%EB%B6%84%EA%B3%BC%E2%80%85%EC%A0%90" target="_blank">11664. 선분과 점</a>

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
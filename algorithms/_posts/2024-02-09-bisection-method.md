---
layout: post
title: Bisection Method
description: >
  Bisection Method 알고리즘에 대해 설명하는 페이지입니다.
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

## Introduction
- **Definition**
  - **이분법(Bisection method)** 은 주어진 범위 [lo, hi] 내에서 어떤 함수 f(x)의 값이 0이 되는 지점을 수치적으로 찾아내는 기법을 말한다. 이분법은 매 반복마다 [lo, hi] 구간의 크기를 절반으로 줄여 나간다.
  - 이분법에서 가장 중요한 부분은 반복문의 종료 조건이다. 반복문을 많이 수행할 수록 오차가 줄어들지만, 수행 시간이 길어질 수 밖에 없다. 종료 조건과 관련하여 가장 유용한 방법은 반복문이 **항상 정해진 횟수**만큼 실행되도록 하는 것이다.

## Examples
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Bisection%20method/DARPA.java" target="_blank">DARPA</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Bisection%20method/ARCTIC.java" target="_blank">ARCTIC</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Bisection%20method/CANADATRIP.java" target="_blank">CANADATRIP</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Bisection%20method/WITHDRAWAL.java" target="_blank">WITHDRAWAL</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Bisection%20method/ROOTS.java" target="_blank">ROOTS</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Bisection%20method/LOAN.java" target="_blank">LOAN</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Bisection%20method/RATIO.java" target="_blank">RATIO</a>

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
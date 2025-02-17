---
layout: post
title: Brute-force
description: >
  Brute-force 알고리즘에 대해 설명하는 페이지입니다.
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
  - **Brute-force**는 컴퓨터의 빠른 계산 능력을 이용해 **가능한 경우의 수**를 일일이 나열하면서 답을 찾는 방법을 의미한다.
  - 가능한 방법을 전부 만들어보는 알고리즘들을 가리켜 **완전 탐색(Exhaustive search)**이라고 부른다.
  - **재귀 호출(Recursion)**
    - **재귀 함수(Recursive function)**, 혹은 **재귀 호출(Recursion)** 은 Brute-force 구현 시 유용하게 사용된다.
    - 재귀 함수란 자신이 수행할 작업을 유사한 형태의 여러 조각으로 쪼갠 뒤 그 중 한 조각을 수행하고, 나머지를 자기 자신을 호출해 실행하는 함수를 가리킨다.
    - 쪼개지지 않는 가장 작은 작업들을 가리켜 재귀 호출의 기저 사례(Base case)라고 한다. 기저 사례를 선택할 때는 존재하는 모든 입력이 항상 기저 사례의 답을 이용해 계산될 수 있도록 신경써야 한다.
    - 재귀 호출을 논의할 때 **문제**란 항상 수행해야 할 작업과 그 작업을 적용할 자료의 조합을 의미한다.
  - **최적화 문제(Optimization problem)**
    - 문제의 답이 하나가 아니라 여러 개이고, 그 중에서 어떤 기준에 따라 가장 좋은 답을 찾아 내는 문제들을 통칭해 **최적화 문제**라고 부른다.
    - 최적화 문제를 해결하는 방법은 **동적 계획법, 조합 탐색** 등 여러 가지가 있는데, 그 중 가장 기초적인 것이 **완전 탐색**이다.
- **Time complexity**
  - 완전 탐색 알고리즘의 시간 복잡도를 계산하기 위해선 가능한 후보의 수를 전부 세어보면 된다.
  - 완전 탐색 알고리즘이 모든 경우에 시간 안에 동작함을 확인하기 위해서 후보의 최대 수를 계산하면 된다.

## How to Use
1. 완전 탐색은 존재하는 모든 답을 하나씩 검사하므로, 걸리는 시간은 가능한 답의 수에 비례한다. 최대 크기의 입력을 가정했을 때 답의 개수를 계산하고 이들을 모두 제한 시간 안에 생성할 수 있을지를 가늠한다. 만약 시간 안에 계산할 수 없다면 다른 설계 패러다임을 적용해야 한다.
2. 가능한 모든 답의 후보를 만드는 과정을 여러 개의 선택으로 나눈다. 각 선택은 답의 후보를 만드는 과정의 한 조각이 된다.
3. 그 중 하나의 조각을 선택해 답의 일부를 만들고, 나머지 답을 재귀 호출을 통해 완성한다.
4. 조각이 하나 밖에 남지 않은 경우, 혹은 하나도 남지 않은 경우에는 답을 생성했으므로, 이것을 기저 사례로 선택해 처리한다.

## Examples
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Brute-force/BOGGLE.java" target="_blank">BOGGLE</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Brute-force/PICNIC.java" target="_blank">PICNIC</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Brute-force/BOARDCOVER.java" target="_blank">BOARDCOVER.java</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Brute-force/CLOCKSYNC.java" target="_blank">CLOCKSYNC</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Brute-force/WILDCARD.java" target="_blank">WILDCARD</a>
- 모든 순열(Permutation) 만들기
- 모든 조합(Combination) 만들기
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Brute-force/ZIMBABWE.java" target="_blank">ZIMBABWE</a>

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
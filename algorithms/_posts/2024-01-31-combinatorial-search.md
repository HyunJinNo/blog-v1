---
layout: post
title: Combinatorial Search
description: >
  Combinatorial Search 알고리즘에 대해 설명하는 페이지입니다.
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
  - 완전 탐색을 포함해, 유한한 크기의 **탐색 공간(Search space)** 을 뒤지면서 답을 찾아내는 알고리즘들을 **조합 탐색(Combinatorial search)** 이라고 부른다.
    > **탐색 공간(Search space)**: 부분 답과 완성된 답의 잡합
  - 조합 탐색에는 다양한 최적화 기법이 있으며, 각 기법들은 접근 방법이 다르지만 기본적으로 모두 **최적해가 될 가능성이 없는 답들을 탐색하는 것을 방지하여 만들어 봐야 할 답의 수를 줄이는 것**을 목표로 한다.
  - **휴리스틱(Heuristic)** 이란 "경험에 의거한" 문제 풀이 방법을 의미하며, **휴리스틱 알고리즘**들은 항상 최적의 답을 찾아내지는 못하지만, 어느 정도 현실에 가까운 답을 빨리 찾기 위한 용도로 자주 사용된다.
    - 휴리스틱의 반환 값이 항상 남아있는 탐색 대상의 값보다 작거나 같은 것을 **과소평가(underestimate)하는 휴리스틱**, 또는 **낙관적인 휴리스틱(Optimism heuristic)** 이라고 한다.
  - **제약 충족 문제(Constraint Satisfaction Problem)**
    - **제약 충족 문제(Constraint Satisfaction Problem)** 란 특정한 제약에 해당하는 답을 찾는 문제들을 말한다.
    - **제약 전파(Constraint propagation)** 란 답의 일부를 생성하고 나면 문제의 조건에 의해 다른 조각에 답에 대해 알게 되는 것을 의미한다. 제약 전파는 답의 일부를 생성한 뒤에는 여기에서 얻을 수 있는 정보를 최대한 많이 찾아내는 것이 중요하며, 제약 전파를 이용하면 실제 탐색해야 할 탐색 공간의 수가 크게 줄어든다.
- **Optimization Techniques**
  - **가지치기(Pruning) 기법**은 탐색 과정에서 최적해로 연결될 가능성이 없는 부분들을 잘라내는 기법을 말한다. 예를 들어, 지금까지 찾아낸 최적해보다 부분해가 이미 더 나빠졌다면 현재 상태를 마저 탐색하지 않고 종료해버린다. 가지치기 기법을 사용하면 존재하는 답 중의 일부는 아예 만들지 않기 때문에 프로그램의 동작 속도가 빨라진다.
  - 좋은 답을 빨리 찾아내는 기법들을 **탐색의 순서를 바꾸거나, 탐색 시작 전에 탐욕법을 이용해 적당히 좋은 답을 우선 찾아낸다**.

## Examples
- BOARDCOVER2
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Combinatorial%20search/ALLERGY.java" target="_blank">ALLERGY</a>
- KAKURO2

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
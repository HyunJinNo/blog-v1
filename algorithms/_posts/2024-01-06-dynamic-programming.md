---
layout: post
title: Dynamic Programming
description: >
  Dynamic Programming 알고리즘에 대해 설명하는 페이지입니다.
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
  - **동적 계획법(Dynamic programming)** 은 처음 주어진 문제를 더 작은 문제들로 나눈 뒤 각 조각의 답을 계산하고, 이 답들로부터 원래 문제에 대한 답을 계산해낸다. 이는 **분할 정복**과 같은 접근 방식이다.
  - **중복되는 부분 문제**
    - 동적 계획법에서 어떤 **부분 문제(Overlapping subproblems)** 는 두 개 이상의 문제를 푸는데 사용될 수 있다. 즉, 계산 결과를 재활용한다.
    - 이미 계산한 값을 저장해두는 메모리 장소를 **캐시(Cache)** 라고 한다.
    - 두 번 이상 계산되는 부분 문제를 **중복되는 부분 문제(Overlapping subproblems)** 라고 한다.
  - **메모이제이션(Memoization)**
    - **메모이제이션(Memoization)** 이란 함수의 결과를 저장하는 장소를 마련해 두고, 한 번 계산한 값을 저장해 두었다가 재활용하는 최적화 기법을 말한다.
    - 메모이제이션은 **참조적 투명 함수(Referential transparent function)** 의 경우에만 적용할 수 있다.
      > **참조적 투명성(Referential transparency)**: 함수의 반환 값이 입력 값만으로 결정되는지의 여부를 말한다.  
      > **참조적 투명 함수(Referential transparent function)**: 입력이 고정되어 있을 때 그 결과가 항상 같은 함수들을 말한다.
- **Time Complexity**
  - 동적 계획법의 시간 복잡도는 다음 식을 계산하여 간단하게 계산할 수 있다. 단, 아래 식은 수행 시간의 상한을 간단히 계산할 수 있는 방법일 뿐이며, 항상 정확하지는 않는다는 점을 주의할 것.
    - **(존재하는 부분 문제의 수) X (한 부분 문제를 풀 때 필요한 반복문의 수행 횟수)**
- **재귀적 동적 계획법 vs 반복적 동적 계획법**
  - **재귀적 동적 계획법**
    - 장점
      - 좀 더 직관적인 코드를 짤 수 있음.
      - 부분 문제 간의 의존 관계나 계산 순서에 대해 고민할 필요가 없음.
      - 전체 부분 문제 중 일부의 답만 필요한 경우 더 빠르게 동작함.
    - 단점
      - Sliding Window 테크닉을 사용할 수 없음.
      - Stack overflow 가능성에 대해 신경써야 함.
  - **반복적 동적 계획법**
    - 장점
      - 재귀적 동적 계획법보다 보통 구현 코드 길이가 더 짧음.
      - 재귀 호출에 필요한 부하가 없기 때문에 좀 더 빠르게 동작함.
      - Sliding Window 테크닉을 사용할 수 있음.
    - 단점
      - 구현이 좀 더 비직관적임.
      - 부분 문제 간의 의존 관계를 고려해 계산되는 순서를 신경써야 한다.

## How to Use
- **동적 계획법**
  1. 메모이제이션을 활용한 함수를 구현할 때 항상 **기저 사례(Base case)** 부터 제일 먼저 처리한다. 입력이 범위를 벗어난 경우 등을 기저 사례로 처리하면 된다.
  2. 주어진 문제를 완전 탐색을 이용해 해결한다.
  3. 중복된 부분 문제를 한 번만 계산하도록 메모이제이션을 적용한다.
- **최적화 문제 동적 계획법**
  1. 모든 답을 만들어 보고 그 중 최적해의 점수를 반환하는 완전 탐색 알고리즘을 설계한다.
  2. 전체 답의 점수를 반환하는 것이 아니라, 앞으로 남은 선택들에 해당하는 점수만을 반환하도록 부분 문제 정의를 바꾼다.
  3. 재귀 호출의 입력에 이전에 선택에 관련된 정보가 있다면 꼭 필요한 것만 남기고 줄인다. 문제에 최적 부분 구조가 성립할 경우에는 이전 선택에 관련된 정보를 완전히 없앨 수 있다. 여기서 목표는 가능한 한 중복되는 부분 문제를 만드는 것이다. 입력의 종류가 줄어들면 줄어들 수록 더 많은 부분 문제가 중복되고, 따라서 메모이제이션을 최대한도로 활용할 수 있다.
  4. 입력이 배열이거나 문자열인 경우 가능하다면 적절환 변환을 통해 메모이제이션할 수 있도록 한다.
  5. 메모이제이션을 적용한다.
- **경우의 수 계산 방법**
  1. 모든 답을 직접 만들어서 세어보는 완전 탐색 알고리즘을 설계한다. 이 때 경우의 수를 제대로 세기 위해서 재귀 호출의 각 단계에서 고르는 각 선택지에 다음과 같은 속성이 성립해야 한다.
      - 모든 경우는 이 선택지들에 포함됨
      - 어떤 경우도 두 개 이상의 서택지에 포함되지 않음
  2. 최적화 문제를 해결할 때처럼 이전 조각에서 결정한 요소들에 대한 입력을 없애거나 변형해서 줄인다. 재귀 함수는 앞으로 남아 있는 조각들을 고르는 경우의 수만을 반환해야 한다.
  3. 메모이제이션을 적용한다.
- **최적화 문제 답 계산 방법**
  1. 재귀 호출의 각 단계에서 최적해를 만들었던 선택을 별도의 배열에 저장한다.
  2. 별도의 재귀 함수를 이용해 이 선택을 따라가며 각 선택지를 저장하거나 출력한다.
- **K번째 답 계산 방법**
  1. 답들을 사전 순서대로 만들며 경우의 수를 세는 완전 탐색 알고리즘을 설계하고, 메모이제이션을 적용해 경우의 수를 세는 동적 계획법 알고리즘으로 바꾼다.
  2. 모든 답들을 사전순으로 생성하며 skip개를 건너뛰고 첫 번째 답을 반환하는 재귀 호출 함수를 구현한다. 재귀 함수는 각 조각들에 들어갈 수 있는 값을 하나씩 고려하면서 이 값을 선택했을 때 만들어지는 답의 수 M과 건너 뛰어야 할 답의 수 skip을 비교한다.
     1. M <= skip: M개의 답은 모두 우리가 원하는 답보다 앞에 있으므로, 이들을 건너뛴다. 대신 skip을 M만큼 줄인다.
     2. M > skip: M개의 답 중에 우리가 원하는 답이 있으므로, 이 값을 선택한다. M개의 답 중에 skip개를 건너띈 것이 원하는 답이다. 이 값을 답에 추가하고 재귀 호출로 답의 나머지 부분을 만든다.

## Examples
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Dynamic%20programming/JUMPGAME.java" target="_blank">JUMPGAME</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Dynamic%20programming/WILDCARD.java" target="_blank">WILDCARD</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Dynamic%20programming/TRIANGLEPATH.java" target="_blank">TRIANGLEPATH</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Dynamic%20programming/LIS.java" target="_blank">LIS</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Dynamic%20programming/JLIS.java" target="_blank">JLIS</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Dynamic%20programming/PI.java" target="_blank">PI</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Dynamic%20programming/QUANTIZE.java" target="_blank">QUANTIZE</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Dynamic%20programming/TILING2.java" target="_blank">TILING2</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Dynamic%20programming/TRIPATHCNT.java" target="_blank">TRIPATHCNT</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Dynamic%20programming/SNAIL.java" target="_blank">SNAIL</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Dynamic%20programming/ASYMTILING.java" target="_blank">ASYMTILING</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Dynamic%20programming/POLY.java" target="_blank">POLY</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Dynamic%20programming/NUMB3RS.java" target="_blank">NUMB3RS</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Dynamic%20programming/PACKING.java" target="_blank">PACKING</a>
- OCR
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Dynamic%20programming/MORSE.java" target="_blank">MORSE</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Dynamic%20programming/KLIS.java" target="_blank">KLIS</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Dynamic%20programming/DRAGON.java" target="_blank">DRAGON</a>
- ZIMBABWE
- RESTORE
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Dynamic%20programming/TICTACTOE.java" target="_blank">TICTACTOE</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Dynamic%20programming/NUMBERGAME.java" target="_blank">NUMBERGAME</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Dynamic%20programming/BLOCKGAME.java" target="_blank">BLOCKGAME</a>
- <a href="https://github.com/HyunJinNo/Algorithm/blob/main/Dynamic%20programming/SUSHI.java" target="_blank">SUSHI</a>
- GENIUS

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
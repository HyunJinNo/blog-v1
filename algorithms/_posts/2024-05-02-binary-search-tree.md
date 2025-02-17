---
layout: post
title: Binary Search Tree
description: >
  Binary Search Tree에 대해 설명하는 페이지입니다.
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
- [Comments](#comments)

## Introduction
- <b>Definition</b>
  - <b>이진 탐색 트리(Binary Search Tree)</b>는 <b>탐색 트리(Search Tree)</b> 중 가장 흔하게 사용되는 트리이다.
  - 탐색 트리를 활용하면 원소의 추가 및 삭제만이 아니라 특정 원소의 존재 여부 확인 등의 다양한 연산을 빠르게 수행할 수 있다.
  - 이진 탐색 트리의 각 노드는 하나의 원소를 갖고 있으며, 각 노드의 왼쪽 서브트리에는 해당 노드의 원소보다 작은 원소를 가진 노드들이, 오른쪽 서브트리에는 보다 큰 원소를 가진 노드들이 들어간다.
  - 이진 탐색 트리에서 원하는 값을 찾는 과정은 배열에서의 <b>이진 탐색</b>과 유사하다. 이진 탐색 트리에서는 특정 원소의 존재 여부를 쉽게 확인할 수 있으며, 실질적으로는 이진 탐색으 비슷한 속도로 찾을 수 있다.
  - 이진 탐색 트리를 <b>중위 순회(Inorder Traverse)</b>하면 크기 순서로 정렬된 원소의 목록을 얻을 수 있다.
  - 이진 탐색 트리는 집합에 원소를 추가하거나 삭제하는 조작 연산을 쉽게 수행할 수 있다.
  - <b>주요 용어</b>
    - <b>탐색 트리(Search Tree)</b>: 연결 리스트나 큐처럼 자료들을 담는 컨테이너로서, 자료들을 일정한 순서에 따라 정렬한 상태로 저장해둔다.
    - <b>이진 트리(Binary Tree)</b>는 각 노드가 왼쪽과 오른쪽, 최대 두 개의 자식 노드만을 가질 수 있는 트리를 의미한다.   
    - <b>편향 트리(Skewed Tree)</b>: 왼쪽 또는 오른쪽 서브트리만 갖는, 한 쪽으로 기울어진 트리를 의미한다. 이런 트리 구조는 연결 리스트를 사용하는 것과 차이가 없다.   
    - <b>균형 잡힌 이진 탐색 트리(Balanced binary Search Tree)</b>: 노드의 수가 <b>N</b>개일 때, 트리의 높이가 항상 <code>O(lg<b>N</b>)</code>이 되는 트리를 말한다. 대표적인 예로 <b>레드-블랙 트리(red-black tree)</b>가 있다.
  
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
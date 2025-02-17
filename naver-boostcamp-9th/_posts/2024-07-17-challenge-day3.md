---
layout: post
title: 챌린지 3일차 학습 정리
description: >
  네이버 부스트캠프 9기 챌린지 3일차 학습 정리 페이지입니다.
image:
  path: /assets/img/naver-boostcamp-9th/naver-boostcamp-9th.jpg
  srcset:
    1060w: /assets/img/naver-boostcamp-9th/naver-boostcamp-9th.jpg
    530w: /assets/img/naver-boostcamp-9th/naver-boostcamp-9th.jpg
    265w: /assets/img/naver-boostcamp-9th/naver-boostcamp-9th.jpg
related_posts:
  - None
sitemap: true
comments: false
---

> - 누군가 작성한 것을 그대로 쓰는 것이 아니라 **나만의 언어로 재구조화하여 작성**해야 합니다.
> - 기술 키워드에 대한 상세 내용도 좋고, 미션 해결 과정에서 기능 구현을 성공한 사례도, 트러블 슈팅 경험도 좋습니다.

<h2> 목차 </h2>

- [학습한 내용](#학습한-내용)
  - [XML이란?](#xml이란)
    - [XML의 장점](#xml의-장점)
    - [XML의 용도](#xml의-용도)
    - [XML 문법](#xml-문법)
  - [JSON이란?](#json이란)
    - [JSON 구조](#json-구조)
- [Comments](#comments)

## 학습한 내용

### XML이란?

XML은 e**X**tensible **M**arkup **L**anguage의 약자로 여러 특수 목적의 마크업 언어를 만드는 용도에서 권장되는 다목적 마크업 언어입니다.

#### XML의 장점

XML의 장점은 다음과 같습니다.

- **데이터 무결성 유지**
  데이터 설명과 함께 데이터를 전송하므로 데이터 무결성을 유지할 수 있습니다.
- **검색 효율성 향상**
  검색 엔진의 경우 다른 형식의 문서보다 효율적으로 XML 파일을 정렬하고 분류할 수 있으므로 XML을 사용하면 검색 효율성을 향상시킬 수 있습니다.
- **유연한 애플리케이션 설계**

#### XML의 용도

XML은 다목적 마크업 언어로 다양한 목적을 위해 사용할 수 있습니다. XML의 주요 용례는 다음과 같습니다.

- **데이터 전송**
  XML을 사용하면 동일한 데이터를 서로 다른 형식으로 저장하는 두 시스템 간에 데이터를 전송할 수 있습니다.
- **웹 애플리케이션**
  웹 애플리케이션을 개발할 때 프로그램 설정 파일 용도로 사용할 수 있습니다.
- **설명서**
  XML을 사용하여 기술 문서의 구조 정보를 지정할 수 있습니다.
- **데이터 유형**
  여러 프로그래밍 언어에서 XML을 데이터 유형으로 지원하므로 XML 파일과 직접 작동하는 프로그래밍 언어를 통해 프로그램을 작성하는 것이 가능합니다.

#### XML 문법

주요 XML 문법은 다음과 같습니다.

- 모든 XML 요소는 반드시 종료 태그를 가져야 합니다.
  HTML에서는 다음과 같이 종료 태그를 생략할 수 있습니다.

  ```
  <h1>HTML
  <hr>
  ```

  이와 달리 XML에서는 종료 태그가 없으면 오류가 발생합니다. XML 요소는 종료 태그가 반드시 있어야 하며, 빈 태그의 경우 `슬래시(/)`를 사용한 `self-closing`을 해야 합니다.

  ```xml
  <h1>XML</h1>
  <hr />
  ```

- XML 태그는 대소문자를 구분합니다.
- XML 태그를 사용할 때 `시작 태그`와 `종료 태그`의 대소문자가 모두 같아야 합니다.
- 속성 값은 반드시 `따옴표`로 감싸야 합니다.
- XML은 띄어쓰기를 인식합니다.

### JSON이란?

JSON이란 **J**ava**S**cript **O**bject **N**otation의 약자로 일반적으로 클라이언트와 서버 사이에서 데이터를 주고 받을 때 사용하는 양식입니다.

#### JSON 구조

JSON 구조는 다음과 같이 이루어집니다.

```json
{
  "키": "값",
  "types": "사용할 수 있는 자료형은 string, number, boolean, null, object, array 6개가 존재합니다.",
  "string": "문자열 값",
  "number": 20240717,
  "boolean": true,
  "null": null,
  "object": {
    "key1": 3.14159265358979323846264338,
    "key2": false,
    "key3": {
      "key4": "value",
      "key": "value2"
    }
  },
  "array": [
    "12351",
    {
      "key1": 1,
      "key2": "value"
    },
    ["1", "2", "3"]
  ]
}
```

JSON 구조의 특징은 다음과 같습니다.

- JSON 구조는 위와 같이 **키-값** 쌍으로 이루어져 있습니다.
- 키와 값은 `:`으로 구분합니다.
- 값으로 사용할 수 있는 자료형은 `string, number, boolean, null, object, array`의 6가지가 존재합니다.
- 날짜 및 시간 데이터를 지원하지 않습니다.
- 주석을 사용할 수 없습니다.
- 객체 내에 또 다른 객체나 배열을 사용할 수 있습니다. 마찬가지로 배열 내에 또 다른 배열을 사용할 수 있습니다.

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

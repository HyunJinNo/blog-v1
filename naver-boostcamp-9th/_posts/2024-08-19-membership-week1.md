---
layout: post
title: 멤버십 1주차 학습 정리
description: >
  네이버 부스트캠프 9기 멤버십 1주차 학습 정리 페이지입니다.
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
  - [HTML](#html)
    - [개념](#개념)
    - [Hyper Text](#hyper-text)
    - [HTML의 요소 구조](#html의-요소-구조)
    - [HTML 문서 기본 구조](#html-문서-기본-구조)
  - [CSS](#css)
    - [개념](#개념-1)
    - [Cascading](#cascading)
    - [주요 개념](#주요-개념)
  - [Client-Side Rendering (CSR)](#client-side-rendering-csr)
    - [개념](#개념-2)
    - [동작 방식](#동작-방식)
    - [장점](#장점)
    - [단점](#단점)
  - [Server-Side Rendering (SSR)](#server-side-rendering-ssr)
    - [개념](#개념-3)
    - [동작 방식](#동작-방식-1)
    - [장점](#장점-1)
    - [단점](#단점-1)
- [참고 자료](#참고-자료)
- [Comments](#comments)

## 학습한 내용

### HTML

#### 개념

HTML이란 **H**yper **T**ext **M**arkup **L**anguage의 약어로 웹 페이지를 작성하는 데 사용되는 마크업 언어입니다. 웹 페이지의 구조와 내용을 정의하는 데 사용되며, 브라우저가 이 언어를 해석하여 사용자에게 웹 페이지를 보여줍니다.

#### Hyper Text

`하이퍼텍스트(Hyper Text)`란 순차적으로 하나씩 접근하는 기존의 문서와 달리, 링크에 따라 다른 페이지로 이동하거나 동일 페이지 내 다른 위치로 즉시 접근할 수 있는 텍스트를 말합니다.

#### HTML의 요소 구조

```html
<tag attributeName="attributeValue">Content</tag>
```

- `태그(Tag)`

  태그는 HTML 요소를 정의하는 부분으로, 대부분의 태그는 여는 태그와 닫는 태그로 이루어져 있습니다. 일부 태그는 내용없이 사용되며, 이 경우에는 닫는 태그 필요하지 않습니다.

- `속성(Attribute)`

  속성은 HTML 태그에 추가적인 정보를 제공하는 것을 말합니다. 속성은 보통 태그 안에 위치하며, 이름과 값으로 구성됩니다.

- `컨텐츠(Contents)`

  여는 태그와 닫는 태그에 들어가는 요소로, 텍스트나 다른 HTML 요소가 중첩될 수 있습니다.

#### HTML 문서 기본 구조

HTML 문서의 기본 구조는 다음과 같습니다.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>제목</title>
  </head>
  <body>
    <h1>html 문서 내용</h1>
  </body>
</html>
```

- `<!DOCTYPE html>`

  해당 문서가 HTML5로 작성되었음을 선언하는 부분입니다.

- `<html>`

  HTML 문서의 시작과 끝을 나타냅니다.

- `<head>`

  메타데이터를 포함하여, 브라우저에 의해 사용자에게 직접 표시되지 않습니다. 보통 문서의 제목, 문자 인코딩, 그리고 CSS나 스크립트 파일의 참조가 포함됩니다.

- `<body>`

  사용자가 실제로 보게 될 컨텐츠가 들어가는 부분입니다. 텍스트, 이미지, 링크, 리스트, 테이블 등 다양한 요소를 포함할 수 있습니다.

### CSS

#### 개념

`CSS(Cascading Style Sheets)`란 HTML로 작성된 웹 페이지의 외관과 레이아웃을 꾸미기 위해 사용되는 스타일링 언어입니다. CSS는 웹 페이지의 색상, 글꼴, 여백, 위치, 크기 등을 정의하고, HTML의 구조와 컨텐츠로부터 디자인을 분리하여 유지 관리와 재사용을 용이하게 합니다.

#### Cascading

CSS의 `Cascading` 개념은 여러 스타일 규칙이 충돌할 때 어떤 규칙이 적용될지 결정하는 우선순위 체계를 의미합니다. 일반적으로 다음 순서로 우선순위가 적용됩니다.

`inline css > internal css > external css > browser default`

#### 주요 개념

**선택자 (Selector)**

CSS에서 `선택자 (Selector)`는 스타일을 적용할 HTML 요소를 선택하는 데 사용됩니다. 선택자를 사용하는 방법은 다음과 같습니다.

- `태그 선택자`

  특정 HTML 태그에 스타일을 적용합니다.

  ```css
  p {
    color: blue;
  }
  ```

- `클래스 선택자`

  특정 클래스 속성을 가진 요소에 스타일을 적용합니다.

  ```css
  .myClass {
    color: blue;
  }
  ```

- `아이디 선택자`

  특정 아이디 속성을 가진 요소에 스타일을 적용합니다.

  ```css
  #myId {
    color: blue;
  }
  ```

- `복합 선택자`

  여러 선택자를 조합하여 특정 요소를 더 정교하게 선택합니다.

  ```css
  div > p {
    color: blue;
  }
  ```

**속성(Property)과 값(Value)**

CSS에서 속성은 스타일을 지정할 항목을 의미하고, 값은 그 스타일의 구체적인 세부 사항을 정의합니다.

아래 예시에서 `color` 속성은 글자 색상을 정의하고, `font-size`는 글자 크기를 정의합니다.

```css
p {
  color: blue;
  font-size: 1rem;
}
```

### Client-Side Rendering (CSR)

#### 개념

`클라이언트 사이드 렌더링(CSR)`이란 웹 애플리케이션의 UI를 브라우저에서 직접 렌더링하는 방식입니다. 이 방법은 `싱글 페이지 애플리케이션(Single Page Application, SPA)`에서 많이 사용됩니다.

#### 동작 방식

CSR의 동작 방식은 다음과 같습니다.

1. 사용자가 웹 사이트를 처음 방문하면 브라우저는 서버에 요청을 보냅니다.
2. 서버는 HTML 파일, CSS 파일, JavaSCript 파일과 같은 정적 리소스를 클라이언트(브라우저)로 보냅니다.
3. 브라우저에서 HTML을 로드한 후, JavaScript 파일을 다운로드하고 실행합니다.
4. JavaScript 코드는 웹 애플리케이션의 로직을 처리하고 API를 통해 서버로부터 데이터를 가져옵니다.
5. 가져온 데이터와 JavaScript 로직에 따라 브라우저는 `DOM(Document Object Model)`을 동적으로 업데이트하고, 이를 통해 사용자가 보는 UI를 생성합니다.
6. JavaScript 코드를 통해 사용자는 애플리케이션과 상호작용하게 됩니다.

#### 장점

- `빠른 사용자 경험`

  웹 페이지의 초기 로드 후에는 페이지 간의 전환 속도가 빠릅니다. 서버로부터 전체 페이지를 다시 로드하는 대신, 필요한 데이터만 요청하여 UI를 동적으로 업데이트하기 때문에 더 빠른 사용자 경험을 제공합니다.

- `싱글 페이지 애플리케이션(SPA)`

  전체 애플리케이션이 하나의 HTML 페이지 내에서 작동합니다. 또한 사용자는 페이지 전환 없이 콘텐츠를 탐색할 수 있습니다.

#### 단점

- `초기 로딩 속도`

  JavaScript 파일이 큰 경우, 다운로드되고 실행되기 전까지 사용자에게 빈 화면이 보여질 수 있습니다. 즉, 초기 페이지 로딩 시간이 길어질 수 있습니다.

- `검색 엔진 최적화(SEO)`

  검색 엔진이 JavaScript를 제대로 실행하지 못하면 CSR 기반 웹 사이트의 콘텐츠를 크롤링하거나 인덱싱하는 데 어려움을 겪을 수 있습니다. 최근에서 Google을 비롯한 주요 검색 엔진이 JavaScript를 실행할 수 있도록 개선되었지만, 여전히 SSR(Server-Side Rendering)만큼 효율적이지 않습니다.

### Server-Side Rendering (SSR)

#### 개념

`서버 사이드 렌더링(SSR)`이란 웹 페이지의 콘텐츠를 서버에서 렌더링한 후 완성된 HTML 페이지를 클라이언트(브라우저)로 전송하는 방식입니다. `클라이언트 사이드 렌더링(CSR)`과 다르게 초기 페이지 로딩 속성과 검색 엔진 최적화(SEO) 측면에서 장점을 가지고 있습니다.

#### 동작 방식

SSR의 동작 방식은 다음과 같습니다.

1. 사용자가 웹 사이트를 처음 방문하면 브라우저는 서버에 요청을 보냅니다.
2. 서버는 요청을 처리하면서 필요한 데이터를 데이터베이스나 API에서 가져옵니다.
3. 서버는 가져온 데이터를 이용해 템플릿 엔진(Ex. EJS, PUG, Handlebars) 또는 프레임워크(Ex. Next.js, Nuxt.js)를 사용하여 HTML 페이지를 생성합니다.
4. 모든 콘텐츠가 포함된 완성된 HTML 페이지를 브라우저로 보냅니다.
5. 브라우저에서 서버로부터 받은 HTML 페이지를 렌더링하여 사용자에게 화면을 표시합니다.
6. JavaScript 파일을 다운로드한 후, 필요한 동적 기능이 활성화됩니다. 이를 통해 사용자는 애플리케이션과 상호작용할 수 있게 됩니다.

#### 장점

- `빠른 초기 페이지 로드`

  서버에서 이미 렌더링된 HTML 페이지를 전달하기 때문에, 클라이언트는 이를 즉시 렌더링하여 화면에 표시할 수 있습니다. 이를 통해 사용자는 빠르게 페이지 내용을 볼 수 있습니다.

- `검색 엔진 최적화(SEO)`

  SSR은 검색 엔진이 페이지의 콘텐츠를 쉽게 크롤링하고 인덱싱할 수 있습니다. 이는 검색 엔진 최적화에 매우 유리합니다.

#### 단점

- `서버 부하 증가`

  서버에서 모든 요청에 대해 HTML 페이지를 렌더링해야 하므로, 서버에 부하가 증가할 수 있습니다. 특히 트래픽이 많은 경우 서버 성능이 문제가 될 수 있습니다.

- `페이지 전환 속도`

  SSR에서는 새로운 페이지로 이동할 때마다 서버에 요청을 보내고, 서버에서 새 HTML을 생성하여 전송하기 때문에 페이지 전환 속도가 느릴 수 있습니다.

## 참고 자료

- `HTML`
  - <a href="https://velog.io/@strivepdev/HTML%EC%9D%B4%EB%9E%80" target="_blank">HTML이란?</a>
  - <a href="https://namu.wiki/w/HTML" target="_blank">https://namu.wiki/w/HTML</a>
  - <a href="https://ko.wikipedia.org/wiki/%ED%95%98%EC%9D%B4%ED%8D%BC%ED%85%8D%EC%8A%A4%ED%8A%B8" target="_blank">하이퍼텍스트</a>
- `CSS`
  - <a href="https://www.w3schools.com/css/css_intro.asp" target="_blank">https://www.w3schools.com/css/css_intro.asp</a>
  - <a href="https://developer.mozilla.org/ko/docs/Web/CSS/CSS_selectors" target="_blank">https://developer.mozilla.org/ko/docs/Web/CSS/CSS_selectors</a>
- `CSR & SSR`
  - <a href="https://velog.io/@jhyun_k/%EC%84%9C%EB%B2%84%EC%82%AC%EC%9D%B4%EB%93%9C%EB%A0%8C%EB%8D%94%EB%A7%81-vs-%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8-%EC%82%AC%EC%9D%B4%EB%93%9C%EB%A0%8C%EB%8D%94%EB%A7%81-SSR%EA%B3%BC-CSR" target="_blank">https://velog.io/@jhyun_k/%EC%84%9C%EB%B2%84%EC%82%AC%EC%9D%B4%EB%93%9C%EB%A0%8C%EB%8D%94%EB%A7%81-vs-%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8-%EC%82%AC%EC%9D%B4%EB%93%9C%EB%A0%8C%EB%8D%94%EB%A7%81-SSR%EA%B3%BC-CSR</a>

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

---
layout: post
title: 챌린지 1일차 학습 정리
description: >
  네이버 부스트캠프 9기 챌린지 1일차 학습 정리 페이지입니다.
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
  - [Git 기초 명령어](#git-기초-명령어)
  - [JSDoc 기초 사용법](#jsdoc-기초-사용법)
  - [자바스크립트 표준 입력 방법](#자바스크립트-표준-입력-방법)
- [참고 자료](#참고-자료)
- [Comments](#comments)

## 학습한 내용

### Git 기초 명령어

| Feature                   | Command                            | Description                                        |
| ------------------------- | ---------------------------------- | -------------------------------------------------- |
| [Create a Repository]     |                                    |                                                    |
|                           | git clone [repository_url]         | Download from an existing repository               |
| [Observe your Repository] |                                    |                                                    |
|                           | git status                         | List new or modified files not yet committed       |
|                           | git diff <commit-ish> <commit-ish> | Show the changes between two commits ids           |
|                           | git blame [file]                   | List the change dates and authors for a file       |
| [Working with Branches]   |                                    |                                                    |
|                           | git branch new_branch              | Create a new branch called new_branch              |
| [Make a change]           |                                    |                                                    |
|                           | git commit -am "commit message"    | commit all your tracked files to versioned history |
|                           | git reset --hard                   | Revert everything to the last commit               |
|                           | git revert -m <commit-ish>         | Revert merge commit                                |
| [Synchronize]             |                                    |                                                    |
|                           | git fetch                          | Get the latest changes from origin                 |
|                           | git pull --rebase                  | Get the latest changes from origin and rebase      |

### JSDoc 기초 사용법

JSDoc 주석은 `/** ... */` 사이에 기술합니다.

```javascript
/**
 * [설명 부분]
 *
 * @param {number} param1 [파라미터 설명]
 * @param {string} param2 [파라미터 설명]
 * @returns {boolean} [반환 값 설명 부분]
 */
const solution = (param1, param2) => {
  ...
  return true;
}
```

- `설명 부분`: 함수나 클래스 등에 대한 설명을 지정하는 부분입니다.
- `@param`: 파라미터의 타입과 설명을 지정하는 부분입니다.
- `@returns`: 반환 값의 타입과 설명을 지정하는 부분입니다.

### 자바스크립트 표준 입력 방법

```javascript
const readline = require("readline");

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

rl.on("line", (line) => {
  ...
  rl.close();
});

rl.on("close", () => {
  console.log("프로그램 종료");
});
```

- `"line"`: 표준 입력을 한 줄로 입력받는 부분입니다.
- `"close"`: 표준 입력이 종료될 경우 실행하는 코드를 담는 부분입니다.
- `rl.close()`: 입력 중단을 선언하는 부분입니다. 해당 코드가 존재하지 않으면 사용자가 `Ctrl + C`를 누르기 전까진 반복해서 표준 입력을 받습니다.

## 참고 자료

- <a href="https://inpa.tistory.com/entry/NODE-%F0%9F%93%9A-%EB%85%B8%EB%93%9C-%EC%BD%98%EC%86%94%EC%B0%BD%EC%97%90%EC%84%9C-%EC%9E%85%EC%B6%9C%EB%A0%A5-%ED%95%98%EB%8A%94%EB%B2%95" target="_blank">NODE-📚-노드-콘솔창에서-입출력-하는법</a>
- <a href="https://velog.io/@zaman17/node.js-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%BD%98%EC%86%94%EB%A1%9C-%EA%B0%92-%EC%9E%85%EB%A0%A5%EB%B0%9B%EA%B8%B0" target="_blank">node.js-자바스크립트-콘솔로-값-입력받기</a>
- <a href="https://poiemaweb.com/jsdoc-type-hint" target="_blank">JSDoc을 사용하여 자바스크립트에 타입 힌트 제공하기</a>

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

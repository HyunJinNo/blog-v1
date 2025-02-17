---
layout: post
title: 챌린지 13일차 학습 정리
description: >
  네이버 부스트캠프 9기 챌린지 13일차 학습 정리 페이지입니다.
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
  - [VCS 버전관리 시스템](#vcs-버전관리-시스템)
  - [로컬 버전 관리](#로컬-버전-관리)
  - [중앙집중식 버전 관리 시스템](#중앙집중식-버전-관리-시스템)
  - [분산 버전 관리 시스템(Version Control System, VCS)](#분산-버전-관리-시스템version-control-system-vcs)
  - [버전 관리의 장점](#버전-관리의-장점)
- [Git](#git)
  - [내부 동작](#내부-동작)
  - [GitHub](#github)
  - [Repository](#repository)
  - [File](#file)
  - [SHA](#sha)
  - [파일 상태 용어](#파일-상태-용어)
  - [Git 명령어](#git-명령어)
  - [Git 개체 (Objects)](#git-개체-objects)
  - [참고 자료](#참고-자료)
- [Comments](#comments)

## 학습한 내용

### VCS 버전관리 시스템

- 문서나 설계도, 소스 코드 등의 변경 사항, 즉 버전을 관리해주는 소프트웨어
- 파일 변경 사항을 시간에 따라 기록하고, 필요할 때 특정 버전을 다시 호출할 수 있는 시스템
- 소스 코드의 변경 사항을 추적하며, 파일이 언제, 누가, 어떻게 변경되었는지를 기록한다.

### 로컬 버전 관리

- 버전을 관리하기 위해 파일을 복사하는 방식
- 사용자가 파일을 직접 복사하기 때문에 잘못된 결과가 나타날 수 있음(파일 잘못 복사, 삭제)
- Revision Control System이 대표적인 툴
- 서버 없이 로컬 컴퓨터 내에서 버전을 관리하는 방식
- 간단한 데이터베이스만으로도 구현이 가능하며 단순하고 개인적인 프로젝트에 적합하다.
- 협업하기 힘들다.
- 컴퓨터가 고장나는 등 내부 정보가 날아가면 복구할 수 없다.

### 중앙집중식 버전 관리 시스템

- 기존의 로컬 버전 관리 시스템을 개선하기 위한 방식
- 파일을 관리하는 별도의 서버를 둔다.
- 서버에 최종 버전이 올라가 있으며, 사용자들은 이 중 수정을 원하는 파일만 로컬에 받아 수정한 후 서버에 올리는 방식으로 버전 관리가 이루어진다.
- 중앙 서버가 고장나는 경우 사용할 수 없는 단점이 존재합니다.

### 분산 버전 관리 시스템(Version Control System, VCS)

- 파일을 저장하는 서버가 있으며, 수정을 위해 프로젝트 전체를 로컬 컴퓨터에 다운 받은 뒤 수정하는 방식으로 버전 관리가 이루어진다.
- 중앙 서버에 장애가 발생하더라도 개별 사용자는 각자 작업할 수 있으며, 로컬 컴퓨터에 있는 작업물로 복구가 가능하다.
- Git이 대표적인 툴이다.

### 버전 관리의 장점

- `변경점 관리`
  - 어떤 내용을 누가 작성해서 어느 시점에 들어갔는지 확인할 수 있다.
- `버전 관리`
  - 특정 시점에 Tag를 달아 버전을 표시해주고, 브랜치 등으로 동시에 여러 버전을 개발할 수 있다.
- `백업 & 복구`
  - 무언가가 잘못되었을 때 다시 특정 시점으로 돌아가게 해주고, 사고로 내용이 날아간 경우에도 복구할 수 있다.
- `협업`
  - 같이 일하는 사람들에게 수정 사항을 쉽게 공유할 수 있다.

## Git

- 대표적인 분산형 버전 관리 시스템 중 하나입니다.
- 장점
  - 서로의 코드를 검증하는 피어 리뷰에 용이하다.
  - 오프라인 작업이 가능하다.
  - 속도가 빠르다.
  - 중앙 서버에 장애가 있어도 로컬에서 작업을 계속할 수 있다.

### 내부 동작

용어 정리

- 로컬 - 현재 프로젝트 폴더에 존재하는 파일들 그 자체
- 인덱스 - 커밋이 이뤄질 준비가 된 파일의 내용들이 위치하는 영역
  - .git/index에 위치
  - **`git add`** 명령어를 수행하면 변동 사항을 인덱스 영역에 반영
  - Blob 파일의 주소를 기록한다.
- 저장소 - 깃이 버전 관리를 하기 위해 필요로 하는 데이터들을 저장하는 곳
  - .git/object/
  - 여러 버전들에 해당하는 파일들의 내용이 Blob 파일로서 이곳에 저장
  - 이곳에 저장된 파일들을 오브젝트 파일이라고 부름

오브젝트 파일

- Blob - 버전 관리하는 파일들 각각의 내용을 저장한 것,
  - SHA1 해싱 기법을 적용하여 Blob 파일의 이름을 얻게 된다.
  - 같은 파일들은 하나의 Blob 파일로서 저장
- Commit - 하나의 버전을 생성하는 것은 하나의 commit 파일을 만드는 것
  - Tree 파일의 주소와 직전 버전에 해당하는 Commit 파일의 주소가 기록
- Tree - 커밋 시점의 파일들 각각에 대해 그 파일명과 해당 파일의 내용을 담고 있는 Blob파일의 주소가 기록

### GitHub

- Git으로 관리하는 프로젝트를 업로드할 수 있는 플랫폼이다.

### Repository

- 프로그램 소스 코드를 Git으로 관리하는 저장소이다.
- 종류
  - `Local Repository`
    - 본인의 컴퓨터에 저장된 로컬 버전의 프로젝트 저장소
    - 내 PC에 저장소를 새로 만들거나, 이미 만들어져 있는 원격 저장소를 로컬 저장소로 복사해 올 수 있다.
  - `Remote Repository`
    - 로컬 Repository와는 반대로 내 컴퓨터가 아닌 (일반적으로 원격 서버) 버전의 프로젝트 저장소
    - 파일이 원격 저장소 전용 서버에서 관리되며 여러 사람이 함께 공유할 수 있다.
- 장점
  - 프로젝트 코드를 공유할 수 있고, 다른 사람의 코드를 확인할 수 있다.
  - 로컬 버전의 프로젝트와 병합하고, 변경 사항을 적용할 수 있다.

### File

- `Untracked`
  - 파일이 Git에 의해서 변경 사항이 전혀 추적되고 있지 않는 상태입니다.
  - 파일을 생성하고 그 파일을 한 번도 git add 하지 않은 상태를 의미합니다.
- `Tracked`
  - 파일이 Git에 의해 변경 사항이 추적되고 있는 상태입니다.
  - `Tracked` 상태는 3가지 상태로 나뉩니다.
    - `Unmodified`
      - `staging area` 에 있는 파일을 커밋한 후의 해당 파일들의 상태를 의미합니다.
      - 기존의 커밋했던 파일을 수정하지 않은 상태입니다.
    - `Modified`
      - 기존에 커밋했던 파일을 수정한 상태입니다.
    - `Staged`
      - 수정한 파일을 `git add` 명령어를 통해 `staging area` 에 추가된 상태입니다.

### SHA

- 미국 NSA가 제작하고 NIST에서 표준으로 채택한 암호학적 해시함수
- SHA는 여러 버전이 있고, git은 SHA-1으로 해시한 식별자를 통해 관리

**해시함수**

- 임의의 길이 데이터를 고정된 길이의 데이터로 전환하는 함수
- 동일한 입력은 항상 동일한 해시 값을 갖게 된다.

**해시함수 특징**

1. 단방향성 - 입력 데이터에서 해시 값으로의 변경은 쉽지만, 역으로는 거의 불가능
2. 해시 충돌 - 서로 다른 입력에 대해 동일한 해시값을 출력할 수 있다. 따라서 높은 보안성을 위해 충돌 확률이 낮아야한다.
3. 고정된 결과값의 길이 - 해시 함수는 항상 일정한 길이의 결과값을 출력

### 파일 상태 용어

- `staged`
  - 저장소에 커밋하기 전에 커밋을 준비하는 위치에 해당합니다.
  - 커밋이 가능한 영역으로, 커밋하기 전 파일을 담아두는 상자라고 비유할 수 있다.
  - `git add` 명령어를 통해 파일을 `Staging area` 에 추가할 수 있다.
- `tracked`
  - Git의 관리를 받을 수 있는 영역에 해당합니다.
  - 해당 영역에 존재하는 파일들은 Git의 관리 대상에 해당합니다.
- `modified`
  - 기존에 커밋했던 파일을 수정한 상태에 해당합니다.

### Git 명령어

```bash
git remote add origin [repository_url]
git log
git clone [repository_url]
```

| Feature                   | Command                         | Description                                        |
| ------------------------- | ------------------------------- | -------------------------------------------------- |
| [Create a Repository]     |                                 |                                                    |
|                           | git clone [repository_url]      | Download from an existing repository               |
| [Observe your Repository] |                                 |                                                    |
|                           | git status                      | List new or modified files not yet committed       |
|                           | git diff                        | Show the changes between two commits ids           |
|                           | git blame [file]                | List the change dates and authors for a file       |
| [Working with Branches]   |                                 |                                                    |
|                           | git branch new_branch           | Create a new branch called new_branch              |
| [Make a change]           |                                 |                                                    |
|                           | git commit -am "commit message" | commit all your tracked files to versioned history |
|                           | git reset --hard                | Revert everything to the last commit               |
|                           | git revert -m                   | Revert merge commit                                |
| [Synchronize]             |                                 |                                                    |
|                           | git fetch                       | Get the latest changes from origin                 |
|                           | git pull --rebase               | Get the latest changes from origin                 |

### Git 개체 (Objects)

- Git에서 변경 사항을 추적하기 위해 사용하는 개체
- 다음과 같이 4가지 타입이 존재합니다.
  - `Blob`
    - **B**inary **L**arge **OB**ject의 줄임말
    - 각 버전의 파일이 blob으로 표현될 수 있다.
    - 바이너리 파일이며, Git DB에서는 SHA1 hash로 이름이 지정됩니다.
  - `Tree`
    - Git의 디렉토리를 표현합니다.
    - blob 또는 다른 tree (하위 디렉토리)를 참조합니다.
    - 디렉토리의 각 항목 당 한 줄로 구성됩니다.
      - 각 줄에는 오브젝트 유형, SHA1 해시, 파일 이름, 파일 실행 권한이 포함됩니다.
  - `Commit`
    - repository의 현재 상태를 가지고 있습니다.
    - 디렉토리와 파일의 상태를 기록하고 있는 Tree 오브젝트를 가리킵니다.
    - 작성자, committer, 커밋 메세지, 부모 커밋에 대한 링크를 가지고 있습니다.
  - `Tag`
    - 태그 작성자, 태그되는 사람의 이름, 타임 스탬프를 포함합니다.
    - 표시된 버전 릴리즈에서 사용되는 기록의 한 지점을 캡쳐하기 위해 사용합니다.

### 참고 자료

- [https://conceptbug.tistory.com/entry/Git-버전-관리-시스템Version-Control-System의-역사#:~:text=버전 관리 시스템(VCS%3B Version,Revision Control System)이라고도 한다](<https://conceptbug.tistory.com/entry/Git-%EB%B2%84%EC%A0%84-%EA%B4%80%EB%A6%AC-%EC%8B%9C%EC%8A%A4%ED%85%9CVersion-Control-System%EC%9D%98-%EC%97%AD%EC%82%AC#:~:text=%EB%B2%84%EC%A0%84%20%EA%B4%80%EB%A6%AC%20%EC%8B%9C%EC%8A%A4%ED%85%9C(VCS%3B%20Version,Revision%20Control%20System)%EC%9D%B4%EB%9D%BC%EA%B3%A0%EB%8F%84%20%ED%95%9C%EB%8B%A4>).
- [https://namu.wiki/w/버전 관리 시스템](https://namu.wiki/w/%EB%B2%84%EC%A0%84%20%EA%B4%80%EB%A6%AC%20%EC%8B%9C%EC%8A%A4%ED%85%9C)
- [https://namu.wiki/w/Git](https://namu.wiki/w/Git)
- [https://namu.wiki/w/GitHub](https://namu.wiki/w/GitHub)
- [https://dev-jacob.tistory.com/entry/Git-Repository란](https://dev-jacob.tistory.com/entry/Git-Repository란)
- [https://inpa.tistory.com/entry/GIT-⚡️-개념-원리-쉽게이해](https://inpa.tistory.com/entry/GIT-⚡️-개념-원리-쉽게이해)
- [https://medium.com/sjk5766/git-3가지-상태와-간단-명령어-정리-a80161aacec1](https://medium.com/sjk5766/git-3가지-상태와-간단-명령어-정리-a80161aacec1)
- [https://ittrue.tistory.com/94](https://ittrue.tistory.com/94)
- [https://it-eldorado.tistory.com/4](https://it-eldorado.tistory.com/4)
- [https://www.codestates.com/blog/content/블록체인-해시함수](https://www.codestates.com/blog/content/%EB%B8%94%EB%A1%9D%EC%B2%B4%EC%9D%B8-%ED%95%B4%EC%8B%9C%ED%95%A8%EC%88%98)
- [https://namu.wiki/w/SHA](https://namu.wiki/w/SHA)
- [https://shafiul.github.io/gitbook/1_the_git_object_model.html](https://shafiul.github.io/gitbook/1_the_git_object_model.html)
- [https://jake-seo-dev.tistory.com/528#:~:text=디렉토리와 파일의 상태,대한 링크도 가지고 있다](https://jake-seo-dev.tistory.com/528#:~:text=%EB%94%94%EB%A0%89%ED%86%A0%EB%A6%AC%EC%99%80%20%ED%8C%8C%EC%9D%BC%EC%9D%98%20%EC%83%81%ED%83%9C,%EB%8C%80%ED%95%9C%20%EB%A7%81%ED%81%AC%EB%8F%84%20%EA%B0%80%EC%A7%80%EA%B3%A0%20%EC%9E%88%EB%8B%A4).

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

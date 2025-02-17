---
layout: post
title: GitHub Actions로 CI/CD 구축 방법
description: >
  GitHub Actions로 CI/CD 구축 방법에 대해 설명하는 페이지입니다.
image:
  path: /assets/img/raspberry-pi/raspberry-pi.jpg
  srcset:
    1060w: /assets/img/raspberry-pi/raspberry-pi.jpg
    530w: /assets/img/raspberry-pi/raspberry-pi.jpg
    265w: /assets/img/raspberry-pi/raspberry-pi.jpg
related_posts:
  - None
sitemap: true
comments: false
---

<i>Environment</i>

- <i>OS: Raspberry Pi OS (64 bit)</i>

<h2>목차</h2>

- [개요](#개요)
- [CI/CD란?](#cicd란)
  - [CI (Continous Integration)](#ci-continous-integration)
  - [CD (Continuous Deployment)](#cd-continuous-deployment)
- [GitHub Actions란?](#github-actions란)
  - [GitHub Actions 구성 요소](#github-actions-구성-요소)
- [Step 1 - GitHub Actions 시작하기](#step-1---github-actions-시작하기)
  - [GitHub Repository에서 만들기](#github-repository에서-만들기)
  - [VSCode에서 만들기](#vscode에서-만들기)
- [Step 2 - Workflow 작성하기](#step-2---workflow-작성하기)
  - [workflow 이름 설정하기](#workflow-이름-설정하기)
  - [이벤트 설정하기](#이벤트-설정하기)
  - [Job 설정하기](#job-설정하기)
- [Step 3 - CI/CD 테스트하기](#step-3---cicd-테스트하기)
- [참고 자료](#참고-자료)
- [Comments](#comments)

## 개요

이번 글에서는 `GitHub Actions`로 CI/CD 구축 방법에 대해 설명하겠습니다.

## CI/CD란?

CI/CD는 소프트웨어 개발 및 배포 프로세스를 자동화하고 효율화하는 방법론으로, `Continuous Integration(지속적 통합)`과 `Continuous Deployment(지속적 배포)`를 의미합니다.

### CI (Continous Integration)

CI는 새로운 코드 변경 사항을 정기적으로 빌드 및 테스트되어 공유 Repository에 통합되는 것을 의미합니다. CI의 주요 목표는 코드 변경 사항을 빠르게 병합하고, 자동화된 빌드 및 테스트를 통해 코드 품질을 보장하는 것입니다. CI를 활용하면 코드 품질이 높아지고, 버그가 조기에 발견되므로 코드의 안정성이 증가한다는 장점이 있습니다. 또한 변경 사항이 작아 충돌이 적고, 개발자 간 협업이 원활해진다는 장점도 있습니다.

### CD (Continuous Deployment)

CD는 코드가 통합된 후 프로덕션 환경에 배포하는 것을 자동화하는 것을 의미합니다. CD를 활용하면 개발자가 코드를 푸시할 때마다 자동으로 프로덕션에 배포되어, 사용자가 변경 사항을 바로 확인할 수 있습니다. 또한 배포 과정의 오류가 줄어들고, 배포 속도가 빨라진다는 장점이 있습니다.

## GitHub Actions란?

`GitHub Actions`란 GitHub에서 제공하는 `CI/CD(Continuous Integration/Continuous Deployment)` 서비스로, 프로젝트 내의 workflow를 자동화할 수 있게 도와줍니다. 이를 통해 코드 빌드, 테스트, 배포 등의 작업을 GitHub Repository 내에서 직접 설정하고 실행할 수 있습니다. GitHub Actions는 GitHub의 YAML 기반 설정 파일을 통해 다양한 이벤트(Ex. Push, Pull Request 등)에 따라 실행되도록 workflow를 구성할 수 있습니다.

### GitHub Actions 구성 요소

GitHub Actions 구성 요소는 다음과 같습니다.

- `Workflow`

  자동화된 작업의 집합입니다. `.github/workflows/` 디렉토리 아래에 YAML 파일로 설정하며, 여러 개의 작업(Ex. 빌드, 테스트, 배포 등)을 단계별로 정의할 수 있습니다.

- `Event`

  `workflow`를 트리거하는 이벤트로, 주로 코드 푸시(Push), PR 생성(Pull Request), release, issue 생성 등의 GitHub 활동이 있습니다. 특정 이벤트가 발생할 때 workflow가 자동으로 실행됩니다.

- `Job`

  `workflow` 안에서 병렬로 실행할 수 있는 작업 단위입니다. 각 job은 여러 단계로 구성되어 있고, 서로 다른 환경에서 병렬 실행이 가능합니다.

- `Step`

  `job` 내부에서 순차적으로 실행되는 개별 작업입니다. Shell 명령어를 실행하거나, GitHub에서 제공하는 액션(Action)을 사용하여 설정할 수 있습니다.

- `Action`

  GitHub Actions에서 제공하는 재사용 가능한 작업 단위입니다. Node.js 설치, AWS S3에 파일 업로드 등을 액션으로 사용할 수 있습니다.

## Step 1 - GitHub Actions 시작하기

GitHub Actions을 사용하려면 먼저 yml 설정 파일을 생성해야 합니다. yml 설정 파일은 GitHub Repository에서 직접 만들어도 되고, 아니면 VSCode에서 만들어도 됩니다.

### GitHub Repository에서 만들기

먼저 다음과 같이 GitHub Repository의 Actions 탭을 클릭합니다.

<img src="/assets/img/raspberry-pi/github-actions-ci-cd/pic1.png" alt="pic1" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

해당 페이지로 들어가면 위와 같이 GitHub에서 여러 가지 템플릿을 제공해 줍니다. 원하는 템플릿을 검색하여 configure 버튼을 클릭하거나, 템플릿 없이 진행하려면 `set up a workflow yourself`를 클릭합니다.

<img src="/assets/img/raspberry-pi/github-actions-ci-cd/pic3.png" alt="pic3" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

### VSCode에서 만들기

먼저 다음과 같이 VSCode에서 `GitHub Actions` Extensions을 설치합니다.

<img src="/assets/img/raspberry-pi/github-actions-ci-cd/pic2.png" alt="pic2" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

이후 프로젝트 최상단 디렉토리에서 `.github` 폴더를 생성하고 그 안에 `workflows`라는 폴더를 생성한 후 yml 설정 파일을 생성합니다. 파일명은 원하는 대로 지으면 됩니다. 예를 들어 배포 관련 workflow 설정 파일을 생성한다면 `.github/workflows/deploy.yml` 구조가 될 수 있습니다.

<img src="/assets/img/raspberry-pi/github-actions-ci-cd/pic10.png" alt="pic10" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

## Step 2 - Workflow 작성하기

다음 예시는 Next.js 애플리케이션을 Raspberry Pi에 배포하고자 CI/CD를 구축한 예시입니다.

<img src="/assets/img/raspberry-pi/github-actions-ci-cd/pic11.png" alt="pic11" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

위의 workflow 설정에 대해 설명하자면 다음과 같습니다.

### workflow 이름 설정하기

```yml
# workflow 이름 설정
name: GitHub Actions로 Next.js 앱 CI/CD 구축 예시
```

먼저 위와 같이 workflow 이름을 설정해야 합니다. workflow 이름은 `name` 필드에서 설정합니다. 이 이름은 GitHub의 Actions 탭에 표시됩니다.

<img src="/assets/img/raspberry-pi/github-actions-ci-cd/pic5.png" alt="pic5" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

### 이벤트 설정하기

```yml
# workflow를 트리거하는 이벤트 설정
on:
  push:
    # main 브랜치에 푸시할 때 실행
    branches: ["main"]
```

`on` 필드에서는 workflow를 트리거하는 이벤트를 정의합니다. 가장 많이 사용하는 이벤트로는 push, pull_request 등이 있습니다. 위의 예시는 main 브랜치에 코드 푸시가 이루어질 때 workflow를 트리거합니다.

### Job 설정하기

<img src="/assets/img/raspberry-pi/github-actions-ci-cd/pic12.png" alt="pic12" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

`jobs` 필드는 workflow 내에서 실행될 개별 작업을 정의합니다. 각 job은 여러 단계를 포함할 수 있으며 병렬로 실행이 가능합니다.

먼저 다음과 같이 정의할 job의 이름을 설정해야 합니다. 이름은 자유롭게 지을 수 있습니다. 저의 경우에는 빌드 후 배포를 하는 job을 정의하였기 때문에 이름을 `build-and-deploy`라고 지었습니다.

```yml
# 실행할 jobs 설정
jobs:
  # job 이름 설정
  build-and-deploy:
```

다음으로 해당 job이 실행될 환경을 지정합니다. GitHub에서 제공하는 기본 `runs-on`는 `ubuntu-latest`, `macos-latest`, `windows-latest` 등이 있습니다.

```yml
# Ubuntu 환경에서 실행
runs-on: ubuntu-latest
```

`steps` 필드에는 job 내부에서 순차적으로 실행되는 개별 작업을 정의합니다. Shell 명령어를 실행하거나, GitHub에서 제공하는 액션(Action)을 사용하여 설정할 수 있습니다.

저는 Next.js로 만든 애플리케이션을 Raspberry Pi에 배포하는 작업을 정의해보도록 하겠습니다. 배포 과정은 다음과 같이 정의하였습니다.

1. GitHub Repository에 push된 코드 가져오기
2. Node.js 설치하기
3. Next.js 애플리케이션 빌드하기
4. scp를 사용해 빌드 파일을 Raspberry Pi로 전송하기
5. ssh를 사용해 Raspberry Pi에 원격 접속한 후 pm2를 사용해 백그라운드로 실행하기

<br />

**1. GitHub Repository에 push된 코드 가져오기**

먼저 GitHub Repository에 있는 코드를 가져와야 합니다. GitHub에서 제공하는 액션으로 [actions/checkout](https://github.com/marketplace/actions/checkout)이 있으며, 해당 액션을 사용해서 Repository의 코드를 가져올 수 있습니다.

```yml
steps:
  - name: Checkout code
    uses: actions/checkout@v4
```

`name` 필드는 각 단계의 이름을 지정하는 부분으로, job 실행 시 로그에 표시됩니다. 이름은 해당 단계가 하고자 하는 작업을 이해할 수 있게 작성하면 됩니다. `uses` 필드는 GitHub에서 제공하는 액션 또는 서드파티 액션을 사용할 때 지정합니다. 예를 들어, [actions/checkout@v4](https://github.com/marketplace/actions/checkout)는 Code Repository를 체크아웃(=코드 가져오기)하는 액션입니다.

<br />

**2. Node.js 설치하기**

코드를 가져온 후에는 애플리케이션을 빌드할 수 있도록 Node.js를 설치해야 합니다. GitHub에서는 다음과 같이 [Node.js를 설치할 수 있는 액션](https://github.com/marketplace/actions/setup-node-js-environment)을 제공합니다. 이 때, 특정 액션에 추가 인수를 전달할 때 사용하는 `with` 필드를 사용하여 설치하고자 하는 Node.js 버전을 지정해야 합니다.

```yml
- name: Set up Node.js
  uses: actions/setup-node@v4
    with:
      # Node.js 버전 설정
      node-version: 20
```

<br />

**3. Next.js 애플리케이션 빌드하기**

다음으로 애플리케이션을 빌드하기 위해 `npm install` 명령어를 실행하여 의존성 설치를 진행한 후 애플리케이션을 빌드합니다. 여기서 사용한 `run`은 셸 명령어를 실행할 때 사용합니다. 이 명령어는 정의된 환경에서 실행됩니다.

```yml
- name: Install dependencies
  run: npm install

- name: Build Next.js Project
  run: npm run build
```

<br />

**4. scp를 사용해 빌드 파일을 Raspberry Pi로 전송하기**

애플리케이션을 빌드한 후에는 scp를 사용하여 빌드 파일을 Raspberry Pi로 전송해야 합니다. 저는 [appleboy/scp-action@v0.1.7](https://github.com/appleboy/scp-action)라는 서드파티 액션을 사용하였습니다.

<img src="/assets/img/raspberry-pi/github-actions-ci-cd/pic13.png" alt="pic13" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

Next.js에서는 빌드 시 `.next` 폴더가 생성되므로 해당 폴더를 source로 지정하면 됩니다. 또한 위에서 Raspberry Pi의 IP 주소나 비밀번호, 포트 번호 등은 중요한 정보이므로 yml 파일에 직접 작성하지 않습니다. 대신 `GitHub Secrets`를 사용하였습니다.
`GitHub Secrets`는 민감한 정보를 안전하게 관리하기 위한 기능으로, yml 파일 내에서 `secrets.<SECRET_NAME>` 형태로 참조할 수 있습니다.

GitHub Secrets를 사용하려면 먼저 다음과 같이 GitHub Repository 내에서 `settings > Secrets and variable > Actions`로 이동합니다.

<img src="/assets/img/raspberry-pi/github-actions-ci-cd/pic6.png" alt="pic6" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

이후 `New repository secret`을 클릭하여 `secrets`를 생성하면 됩니다.

<img src="/assets/img/raspberry-pi/github-actions-ci-cd/pic7.png" alt="pic7" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

<br />

**5. ssh를 사용해 Raspberry Pi에 원격 접속한 후 pm2를 사용해 백그라운드로 실행하기**

빌드 파일을 전송한 후에는 ssh를 사용하여 Raspberry Pi에 원격 접속한 후 이전에 실행되고 있던 프로덕션 서버를 종료한 후 `pm2`를 사용해 새롭게 백그라운드로 실행할 수 있도록 합니다. 저는 [appleboy/ssh-action@v1.1.0](https://github.com/appleboy/ssh-action)라는 서드파티 액션을 사용하였습니다.

<img src="/assets/img/raspberry-pi/github-actions-ci-cd/pic14.png" alt="pic14" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

위에 스크립트를 설명하자면 먼저 빌드 파일이 있는 폴더로 이동한 후 의존성 업데이트를 진행합니다. 이후 이전에 실행되고 있던 프로덕션 서버를 종료한 후 새로운 프로덕션 서버를 백그라운드로 실행합니다.

## Step 3 - CI/CD 테스트하기

CI/CD 테스트 결과는 다음과 같습니다.

<img src="/assets/img/raspberry-pi/github-actions-ci-cd/pic8.png" alt="pic8" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

<img src="/assets/img/raspberry-pi/github-actions-ci-cd/pic9.png" alt="pic9" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

## 참고 자료

- <a href="https://docs.github.com/ko/actions" target="_blank">GitHub Actions 설명서</a>
- <a href="https://github.com/marketplace?type=actions" target="_blank">GitHub Marketplace</a>
- <a href="https://velog.io/@sgwon1996/GitHub-Action%EC%9C%BC%EB%A1%9C-CICD-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0" target="_blank">GitHub Action으로 CI/CD 구축하기</a>
- <a href="https://supersfel.tistory.com/entry/%EB%9D%BC%EC%A6%88%EB%B2%A0%EB%A6%ACgitHub-Action-%EC%9E%90%EB%8F%99%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0-CICD" target="_blank">[라즈베리]gitHub Action 자동배포하기 (CI/CD)</a>
- <a href="https://velog.io/@sangwoong/CICD-GitHub-Action%EC%9C%BC%EB%A1%9C-CICD-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0" target="_blank">[CICD] GitHub Action으로 CI/CD 구축하기</a>
- <a href="https://github.com/appleboy/scp-action" target="_blank">scp-action</a>
- <a href="https://github.com/appleboy/ssh-action" target="_blank">ssh-action</a>

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

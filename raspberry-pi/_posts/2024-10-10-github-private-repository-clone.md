---
layout: post
title: GitHub Private Repository Clone 방법
description: >
  GitHub Private Repository Clone 방법에 대해 설명하는 페이지입니다.
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
- [Step 0 - Private Repository Clone 시도하기](#step-0---private-repository-clone-시도하기)
- [Step 1 - Personal Access Token 발급받기](#step-1---personal-access-token-발급받기)
- [Step 2 - Private Repository clone하기](#step-2---private-repository-clone하기)
- [Comments](#comments)

## 개요

이번 글에서는 깃허브(GitHub)에서 Private Repository를 clone하는 방법에 대해 설명하겠습니다.

## Step 0 - Private Repository Clone 시도하기

먼저 다음과 같이 Private Repository를 그대로 clone하려고 하면 다음과 같은 오류가 발생합니다.

<img src="/assets/img/raspberry-pi/github-private-repository-clone/pic1.png" alt="pic1" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

이는 2021년 8월 13일 이후로 비밀번호 인증을 통해 git clone하는 방식이 더 이상 지원되지 않기 때문입니다. 따라서 GitHub에서 Private Repository를 clone하기 위해선 다음과 같은 과정을 거쳐야 합니다.

## Step 1 - Personal Access Token 발급받기

Private Repository를 clone하기 위해선 먼저 Personal Access Token을 발급 받아야 합니다.

먼저 GitHub에서 로그인 한 후, 본인의 프로필을 클릭해 `Settings > Developer settings` 로 이동합니다.

<img src="/assets/img/raspberry-pi/github-private-repository-clone/pic2.png" alt="pic2" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

<img src="/assets/img/raspberry-pi/github-private-repository-clone/pic3.png" alt="pic3" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

<br />

다음으로 `Personal access tokens > Tokens (classic) > Generator new token > Generate new token (classic)` 을 순서대로 클릭합니다.

<img src="/assets/img/raspberry-pi/github-private-repository-clone/pic4.png" alt="pic4" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

<img src="/assets/img/raspberry-pi/github-private-repository-clone/pic5.png" alt="pic5" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

<br />

아래와 같은 페이지에서 `Personal Access Token`을 발급 받을 수 있습니다. `Note`에는 토큰을 사용하려는 목적을 간단하게 작성하면 됩니다. `Expiration`에는 토큰의 유효 기간을 지정합니다. 마지막으로 `select scopes`에는 해당 토큰을 가지고 접근할 수 있는 권한을 설정하면 됩니다. Private Repository에 대한 권한이 필요하기 때문에 `repo` 항목을 체크하면 됩니다.

<img src="/assets/img/raspberry-pi/github-private-repository-clone/pic6.png" alt="pic6" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

<br />

토큰을 발급 받으면 다음과 같은 페이지가 표시됩니다. <b>주의할 점은 해당 페이지에서 보이는 토큰은 다시 볼 수 없으므로 토큰을 복사하여 보관해야 합니다.</b>

<img src="/assets/img/raspberry-pi/github-private-repository-clone/pic7.png" alt="pic7" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

<img src="/assets/img/raspberry-pi/github-private-repository-clone/pic8.png" alt="pic8" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

<br />

## Step 2 - Private Repository clone하기

이후에 발급 받은 토큰을 활용하여 다음과 같이 Private Repository를 clone할 수 있습니다.

```bash
git clone https://{GitHub 닉네임}:{토큰}@github.com/{GitHub 닉네임}/{clone하려는 Private Repository}.git
```

<img src="/assets/img/raspberry-pi/github-private-repository-clone/pic9.png" alt="pic9" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

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

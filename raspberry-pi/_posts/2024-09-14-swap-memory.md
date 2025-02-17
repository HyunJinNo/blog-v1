---
layout: post
title: 스왑 메모리(Swap Memory) 설정 방법
description: >
  스왑 메모리(Swap Memory) 설정 방법에 대해 설명하는 페이지입니다.
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

<h2> 목차 </h2>

- [스왑 메모리(Swap Memory)란?](#스왑-메모리swap-memory란)
- [Step 1 - 현재 스왑 메모리 확인하기](#step-1---현재-스왑-메모리-확인하기)
- [Step 2 - 스왑 파일 생성하기](#step-2---스왑-파일-생성하기)
- [Step 3 - 스왑 파일에 권한 설정하기](#step-3---스왑-파일에-권한-설정하기)
- [Step 4 - 스왑 메모리 활성화하기](#step-4---스왑-메모리-활성화하기)
- [Step 5 - 시스템 부팅 시 스왑 메모리 자동 활성화 설정](#step-5---시스템-부팅-시-스왑-메모리-자동-활성화-설정)
- [스왑 메모리 비활성화 및 삭제 방법](#스왑-메모리-비활성화-및-삭제-방법)
- [Comments](#comments)

## 스왑 메모리(Swap Memory)란?

`스왑 메모리(Swap Memory)`란 컴퓨터의 물리적인 RAM이 부족할 때 하드 디스크의 일부 디스크 공간을 가상 메모리로 사용하는 기술입니다. 즉, RAM이 가득 차서 더 이상 사용할 수 없을 때, 스왑 메모리를 사용하여 RAM의 일부 역할을 하게 합니다. 스왑 메모리를 사용하는 것은 실제 메모리가 아닌 하드 디스크를 이용하는 것이기 때문에 성능 면에서는 떨어지지만 시스템이 계속 동작하도록 유지할 수 있습니다.

## Step 1 - 현재 스왑 메모리 확인하기

다음 명령어를 입력하여 현재 스왑 메모리가 얼마나 설정되어 있는지 확인합니다.

```bash
free -h
```

<img src="/assets/img/raspberry-pi/swap/swap1.png" alt="swap1" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

위의 사진에서 `Mem`은 실제 메모리를 나타내고, `Swap`은 스왑 메모리를 나타냅니다. 현재 스왑 메모리가 199Mi 만큼 설정되어 있음을 확인할 수 있습니다.

## Step 2 - 스왑 파일 생성하기

먼저 다음 명령어를 입력하여 스왑 파일을 생성합니다.

```bash
sudo fallocate -l 6G /swapfile
```

위 명령어는 크기가 6GB인 스왑 파일을 `/swapfile`이라는 이름으로 생성합니다. 일반적으로 스왑 메모리 크기는 시스템의 실제 RAM 크기, 사용 목적, 시스템 성능 요구 사항에 따라 다르게 설정합니다. 보통 스왑 메모리는 기본 RAM 용량의 2배 정도를 잡는 것을 권장합니다. 저의 경우 기본 RAM 용량이 4GB이므로 스왑 메모리 용량을 6GB로 결정하였습니다.

## Step 3 - 스왑 파일에 권한 설정하기

스왑 파일을 생성한 후 해당 스왑 파일이 다른 사용자에 의해 수정되지 않도록 권한을 설정해야 합니다. 다음 명령어를 입력하여 파일 소유자만 읽고 쓸 수 있도록 권한을 설정합니다.

```bash
sudo chmod 600 /swapfile
```

## Step 4 - 스왑 메모리 활성화하기

스왑 파일에 권한을 설정한 후, 다음 명령어를 입력하여 스왑 파일을 스왑 영역으로 지정합니다.

```bash
sudo mkswap /swapfile
```

이후 다음 명령어를 입력하여 스왑 파일을 활성화합니다.

```bash
sudo swapon /swapfile
```

스왑 파일을 활성화하면 다음과 같이 `free -h` 명령어를 통해 스왑 메모리가 활성화되어 있음을 확인할 수 있습니다.

<img src="/assets/img/raspberry-pi/swap/swap2.png" alt="swap2" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

## Step 5 - 시스템 부팅 시 스왑 메모리 자동 활성화 설정

위에서 생성한 스왑 파일은 시스템을 부팅할 때마다 활성화해야 합니다. 시스템이 재부팅될 때마다 스왑 메모리를 자동으로 활성화하기 위해선 `/etc/fstab` 파일에 스왑 파일을 추가해야 합니다.

먼저 다음 명령어를 입력하여 `/etc/fatab` 파일을 엽니다.

```bash
sudo nano /etc/fstab
```

<img src="/assets/img/raspberry-pi/swap/swap3.png" alt="swap3" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

파일의 마지막 줄에 다음 내용을 추가한 후 파일을 저장합니다.

```bash
/swapfile none swap sw 0 0
```

<img src="/assets/img/raspberry-pi/swap/swap4.png" alt="swap4" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

## 스왑 메모리 비활성화 및 삭제 방법

스왑을 비활성화하고 삭제하려면 다음과 같은 절차를 거쳐야 합니다.

먼저 스왑 파일을 비활성화합니다.

```bash
sudo swapoff /swapfile
```

스왑 파일을 비활성화한 후, 스왑 파일을 삭제합니다.

```bash
sudo rm /swapfile
```

마지막으로 `/etc/fstab`에 추가한 스왑 항목을 제거한 후 파일을 저장합니다.

**수정 전**

<img src="/assets/img/raspberry-pi/swap/swap4.png" alt="swap4" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

**수정 후**

<img src="/assets/img/raspberry-pi/swap/swap3.png" alt="swap3" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); border-radius: 0.5rem"/>

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

---
title: "[Linux] Nodejs 설치하고, js 파일 실행시키기"
date: 2021-7-20 21:59:00 +0900
categories:
  - linux
toc: true
classes: wide
---

### 1 . 먼저 `curl`을 설치한 뒤, 설치하고자하는 버전이 명시된 깃 레포지토리를 설정한다. 여기서 curl은 ~

```bash
$ sudo apt-get install curl
```

<center><img src="http://dl.dropbox.com/s/682n1b4b98ye6ur/Linux-Nodejs%20%EC%84%A4%EC%B9%98%ED%95%98%EA%B3%A0%2C%20js%20%ED%8C%8C%EC%9D%BC%20%EC%8B%A4%ED%96%89%EC%8B%9C%ED%82%A4%EA%B8%B0-1.png"></center>

<br>

### 2. PRA을 추가한다.

```bash
$ curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
```

<br>

### 3. 이후 nodejs를 설치한다.

```bash
$ sudo apt-get install -y nodejs
```

<center><img src="http://dl.dropbox.com/s/2v6hxqelgqlz4b2/Linux-Nodejs%20%EC%84%A4%EC%B9%98%ED%95%98%EA%B3%A0%2C%20js%20%ED%8C%8C%EC%9D%BC%20%EC%8B%A4%ED%96%89%EC%8B%9C%ED%82%A4%EA%B8%B0-2.png"></center>

<br>

### 4. nodejs 버전을 검색해보며, 잘 깔렸는지 확인한다.

<center><img src="http://dl.dropbox.com/s/q4fass87453gmmo/Linux-Nodejs%20%EC%84%A4%EC%B9%98%ED%95%98%EA%B3%A0%2C%20js%20%ED%8C%8C%EC%9D%BC%20%EC%8B%A4%ED%96%89%EC%8B%9C%ED%82%A4%EA%B8%B0-3.png"></center>

<br>

### 5. clone 받은 레포지토리 내의 JavaScript 파일을 실행한다.

<center><img src="http://dl.dropbox.com/s/8x9270p95xzhobo/Linux-Nodejs%20%EC%84%A4%EC%B9%98%ED%95%98%EA%B3%A0%2C%20js%20%ED%8C%8C%EC%9D%BC%20%EC%8B%A4%ED%96%89%EC%8B%9C%ED%82%A4%EA%B8%B0-4.png"></center>
---
title: "Netflix Clone Coding(1/4)"
date: 2020-11-19 00:38:40 +0900
categories:
  - Clone Coding
toc: true
toc_sticky: true
---

# 들어가기에 앞서..

본 프로젝트는 [https://www.youtube.com/watch?v=XtMThy8QKqU&t=9951s](https://www.youtube.com/watch?v=XtMThy8QKqU&t=9951s) 의 NETFLIX clone coding 영상을 보며 따라 만들어본 프로젝트입니다.

# 시작에 앞서 준비해야 할 것들

## 1. TMDB

본 프로젝트는 영화들의 정보를 가져와 프론트에 뿌려주는 작업을 위해 **TMDB**를 사용합니다.

회원가입 후 API key 발급받아야 하는데, 방법은 아래와 같습니다.

회원가입을 한 후 [Settings] - [API]에 들어가 새로운 API key를 생성합니다. Developer를 클릭하고 Accept 버튼을 눌러 API key를 발급 받기 위한 정보를 작성합니다. url은 대충 작성해서 넘어가도 됩니다.

다 작성하고 나면 아래와 같이 발급반은 API key를 볼 수 있습니다.

![](/assets/images/api_key.png)

Postman을 통해 위 사진의 API 요청 예를 전송해 response 를 통해 제대로 generate 된 API key임을 확인합니다.

![](/assets/images/postman_result.png)

## 2. create-react-app을 통해 React 프로젝트 생성

아래의 명령어를 터미널에 입력해 React 프로젝트를 생성합니다.

```bash
$ npx create-react-app netflix-clone
```

## 3. Firebase

Firebase는 다음과 같은 특징이 가집니다.

- 웹 사이트를 호스팅 하기 위한 목적으로 사용합니다.
- 데이터베이스를 사용할 수 있습니다.
- 인증 목적으로 사용할 수 있습니다.

본 프로젝트에서 Firebase는 Backend의 배포를 통한 웹 사이트 hosting을 목적으로 사용됩니다. Firebase가 [https://netflix-clone-5966a.web.app](https://netflix-clone-5966a.web.app) 과 같은 real link를 줍니다.

1. Add project 버튼을 누르고 netflix-clone 이라는 이름의 프로젝트를 생성합니다.
2. 프로젝트가 생성되었다면, `웹` 버튼을 눌러 웹 앱에 Firebase 를 추가합니다.

   ![](/assets/images/firebase.png)

3. [Firebase SDK 추가]는 node module을 사용할 것이므로, 생략합니다.
4. [Firebase CLI] 설치를 위해 터미널에 아래의 명령어를 작성합니다.

   ```bash
   $ npm install -g firebase-tools
   ```

   -g 옵션을 위해 관리자 권한으로 실행해야 합니다.

5. 좌측 사이드바의 [Hosting] 메뉴를 선택 후 다음 버튼을 눌러 호스팅을 시작합니다.

## 불필요한 파일들 삭제

netflix-clone 프로젝트의 src 폴더에 있는 아래 세 개의 불필요한 파일을 삭제합니다.

![](/assets/images/unnecessary.png)

## axios 모듈 설치

axios 모듈을 설치합니다

```bash
yarn add axios
```

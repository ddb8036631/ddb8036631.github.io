---
title: "[React] node-sass 버전 에러"
date: 2021-1-26 01:38:00 +0900
categories:
  - React
# toc: true
classes: wide
---

`create-react-app` 모듈을 사용해 리액트 프로젝트를 생성하면, `css-loader` 모듈도 자동으로 같이 설치된다. 현재 `node-sass` 모둘의 최신 버전은 5.0.0 이다. 이 버전은 create-react-app 으로 생성한 프로젝트의 css-loader 모듈과 충돌 문제가 있다. 따라서 node-sass 모듈을 설치할 때 4.14.1 버전을 명시해서 설치해주면 오류가 해결된다.

```bash
$ yarn add node-sass@4.14.1
```

<br>

node-sass 모듈이 이미 설치되어 있는 경우엔 `yarn remove node-sass` 명령을 통해 제거한 뒤 위의 코드를 실행하자.

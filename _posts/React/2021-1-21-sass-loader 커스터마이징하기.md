---
title: "[React] sass-loader 설정 커스터마이징하기"
date: 2021-1-21 20:39:00 +0900
categories:
  - react
toc: true
classes: wide
---

# 상대 경로 입력 없이 파일 명만으로 임포트 하는 방법

scss 파일의 디렉토리 깊이가 깊어지면 아래와 같이 임포트 구문이 길어질 수 있습니다.

```sass
@import "src/components/title/TitleComponent.scss";
```

<br>

해당 scss 파일이 위치하는 디렉토리까지의 경로를 작성해주는 번거로움을 sass-loader 설정을 변경해줌으로써 해결할 수 있습니다.

먼저 `yarn eject` 명령어를 통해 프로젝트의 숨겨진 설정 파일들을 꺼내야 합니다. create-react-app 으로 만들어진 프로젝트는 기본적으로 Git 설정이 되어 있고, yarn eject 명령어를 사용하려면 git의 변경 사항이 없어야 하므로, add와 commit 단계를 거쳐 로컬 변경사항을 저장해줍니다.

```sass
$ git add .
$ git commit -m "Commit before yarn eject"
```

<br>

이후 yarn eject 명령어를 실행하면 아래와 같이 `config` 와 `scripts` 디렉토리가 생깁니다.

![/assets/images/sass-loader 커스터마이징하기1.png](/assets/images/sass-loader 커스터마이징하기1.png)

<br>

config 디렉토리 내부의 webpack.config.js 파일로 들어가서 `sassRegex` 라는 키워드를 검색하면 아래와 같은 설정 옵션들이 보입니다.

![/assets/images/sass-loader 커스터마이징하기2.png](/assets/images/sass-loader 커스터마이징하기2.png)

<br>

이 부분을 아래와 같이 변경하면 scss 파일의 위치가 어디든 상관없이 상대 경로를 입력하지 않고 파일 명만으로 불러올 수 있게 됩니다.

```javascript
{
  test: sassRegex,
  exclude: sassModuleRegex,
  use: getStyleLoaders({
    importLoaders: 3,
    sourceMap: isEnvProduction
      ? shouldUseSourceMap
      : isEnvDevelopment,
  }).concat({
    loader: require.resolve("sass-loader"),
    options: {
      sassOptions: {
        includePaths: [paths.appSrc + "/styles"],
      },
      sourceMap: isEnvProduction && shouldUseSourceMap,
    },
  }),
  // Don't consider CSS imports dead code even if the
  // containing package claims to have no side effects.
  // Remove this when webpack adds a warning or an error for this.
  // See https://github.com/webpack/webpack/issues/6571
  sideEffects: true,
},
```

<br>

# 임포트 구문도 없이 설정된 파일들을 디폴트로 불러오는 방법

위에서 변경한 sass-loader 옵션에 추가로 `prependData` 속성 값에 **임포트 구문**을 작성해 추가해주면 됩니다.

```js
{
  test: sassRegex,
  exclude: sassModuleRegex,
  use: getStyleLoaders({
    importLoaders: 3,
    sourceMap: isEnvProduction
      ? shouldUseSourceMap
      : isEnvDevelopment,
  }).concat({
    loader: require.resolve("sass-loader"),
    options: {
      sassOptions: {
        includePaths: [paths.appSrc + "/styles"],
      },
      sourceMap: isEnvProduction && shouldUseSourceMap,
			prependData: `@import 'utils';`,
    },
  }),
  // Don't consider CSS imports dead code even if the
  // containing package claims to have no side effects.
  // Remove this when webpack adds a warning or an error for this.
  // See https://github.com/webpack/webpack/issues/6571
  sideEffects: true,
},
```

이후에는 모든 scss 파일이 prependData 속성에 작성된 scss 파일들을 기본적으로 임포트해서 사용될 것입니다.

<br>

출처 - [리액트를 다루는 기술](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791160508796&orderClick=LEa&Kc=)

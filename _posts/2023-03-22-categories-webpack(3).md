---
title: "[Webpack] CRA없이 리액트 웹앱 번들링 후 배포하기"
excerpt: "CRA없이 Webpack으로 React 개발환경을 구축해보자 "

categories:
  - Webpack
tags:
  - [Webpack, React]

permalink: /Webpack/CRA없이 리액트 웹앱 번들링 후 배포하기

toc: true
toc_sticky: true

date: 2023-03-22
last_modified_at: 2023-03-22
---
<hr>

CRA(create-react-app)없이 리액트 개발환경을 세팅하면서 CRA가 어떤 라이브러리로 구성되어 있는지, 그 라이브러리들의 역할들에 대해 알아보고자 한다.

저번 시간에 배운 webpack을 이용해 번들링과 빌드를 할 수 있다.

>- **번들링**: 단순하게 여러개의 소스 코드들을 1개의 파일로 결합하는 과정
- **빌드**: 개발자가 작성한 코드를 웹 서버(인터넷)에 실행 가능한 형태로 제공하는 과정
<br><br> 
빌드 개념에 번들링 개념이 포함된다.

<hr>

## 1. 개발환경 초기화
<h3> 🛠️ npm init</h3>
npm init을 통해 package.json 파일을 생성한다.
```
npm init -y
```
- `-y` 옵션을 사용하면 프로젝트에 대한 기본양식을 설정할 필요 없이 default값으로 설정된 package.json 파일이 생성된다.

<hr class="sub">

<h3> 🛠️ 내부 폴더, 파일 생성</h3>
프로젝트 구성에 필요한 폴더들을 생성한다. 이번 과제에선 아고라스테이츠 레퍼런스 코드를 사용했다.
<center> <img src="/assets/images/posts_img/webpack(3)/tree.png" /> </center>

<hr>

## 2. 리액트 설치
```
npm i react react-dom
```
- **react:** 리액트 컴포넌트와 Hooks, 라이프 사이클에 대한 정보가 들어있는 코어 라이브러리
- **react-dom:** react 코드와 DOM을 연결 (ex. `ReactDom.render`)

<hr>

## 3. 웹팩 설치
```
npm i -D webpack webpack-cli
```
- `-D` (devDependency) 옵션은  옵션으로 실제 프로젝트에 사용하지 않는 개발용 라이브러리를 설치할 때 주로 쓰는 옵션이다.
- **webpack:** 웹팩 코어 패키지
- **webpack-cli:** 웹팩을 터미널에서 사용할 수 있게해 주는 라이브러리

<hr class="sub">

<h3> 🛠️ webpack.config.js 생성</h3>

```javascript
const path = require("path");

module.exports = {
  entry: './src/index.js',
  output: {
    filename: '[name].[hash].js',
    path: path.resolve(__dirname, 'docs'),
    clean: true,
  },
}
```
- **entry:** 빌드의 시작 위치
- **output:** 빌드 결과물 저장 위치


<hr>

## 3. Babel 설치

JSX문법으로 작성된 리액트 코드는 브라우저가 이해할 수 없다. 또한 최신 자바스크립트 코드 같은 경우 구버전의 브라우저에서 사용하기 어렵다.
때문에 JSX나 최신 문법을 브라우저가 이해할 수 있는 JavaScript로 변환해주는 과정이 필요하다. 이러한 역할을 해주는 트랜스파일러가 **Babel**이다.

```
npm i -D babel-loader @babel/core @babel/preset-env @babel/preset-react
```
- **babel-loader:** 바벨과 웹팩을 연결
- **@babel/core:** 바벨 코어 패키지
- **@babel/preset-env:** ES6+ 문법으로 작성된 코드를 ES5문법의 코드로 변환시켜준다.
- **@babel/preset-react:** JSX 문법을 사용할 수 있게 해준다.

<hr class="sub">

<h3> 🛠️ Babel 설정 </h3>

```javascript
module.exports = {
  // ... entry, output 생략
  module: {
    rules: [
      {
        test: /\.js|jsx$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: [
              ['@babel/preset-env'],
              ['@babel/preset-react', { runtime: 'automatic' }],
            ]
          }
        }
      }
    ]
  }
}
```
<hr>

## 4. css loader 설치
### 1️⃣ internal 방식
```
npm i -D style-loader css-loader
```
- **style-loader:** css를 internal 방식으로 번들링 해준다.
- **css-loader:** css 파일을 읽기위한 loader

<hr class="sub">

<h3> 🛠️ css internal 설정 </h3>

loader이므로 `module.rules`의 배열 안에 넣어준다.

```javascript
 module.exports = {
  // ... entry, output 생략
  module: {
    rules: [
      // ... babel loader 생략
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader'],
        exclude: /node_modules/,
      }
    ]
  }
}
```

그러나 이대로 build하면 css파일이 따로 생성되지 않고 internal방식으로 번들링된다. `header`의 `<style>`태그 안에 inline방식으로 생성되는 것이다.
![css-internal](/assets/images/posts_img/webpack(3)/css-internal.png)

이럴때는 css 관련 plugin을 설치해주면 된다.

<hr class="sub">

### 2️⃣ external 방식
```
npm i -D css-minimizer-webpack-plugin mini-css-extract-plugin
```
- **css-minimizer-webpack-plugin:** css를 external 방식으로 번들링 해준다.
- **mini-css-extract-plugin:** css 파일을 한 줄로 압축해준다.

<hr class="sub">

<h3> 🛠️ css external 설정 </h3>
plugin은 `optimization`에 입력한다.

```javascript
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const CssMinimizerPlugin = require('css-minimizer-webpack-plugin');

module.exports = {
  // ... entry, output 생략
  module: {
    rules: [
      // ... babel loader 생략
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, 'css-loader'],
        exclude: /node_modules/,
      }
    ]
  },
  optimization: {
    minimizer: [
      new CssMinimizerPlugin(), // css를 줄여주는 플러그인
    ],
  },
  plugins: [
      new MiniCssExtractPlugin(),
  ],
}
```

`optimization.minimizer`에 **new MiniCssExtractPlugin({ filename: 'style.css'})** 이런식으로 css파일명을 지정해 줄 수도 있다.
![css-external](/assets/images/posts_img/webpack(3)/css-external.png)

<hr>

## 5. html plugin 설치
```
npm install -D html-webpack-plugin clean-webpack-plugin
```
- **html-webpack-plugin:** 번들링된 css파일과 js파일을 html파일에 link, script 태그로 추가해 주어야 할 때 이를 자동화하여 html 파일을 생성해준다.
- **clean-webpack-plugin:** 빌드된 결과물을 정리해주는 플러그인으로, 이전 빌드 결과물을 제거해준다.

<hr class="sub">

<h3> 🛠️ html plugin 설정 </h3>

`template`속성에 적용할 html파일 경로를 적는다.

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  // ... entry, output 생략
  // ... module 생략
  // ... optimization 생략
  plugins: [
    // ... css plugin 생략
    new HtmlWebpackPlugin({
      template: './public/index.html',
    }),
  ],
}
```

<hr>

## 6. 개발환경 배포환경 분리
웹팩 설정파일에서 mode속성을 주면 개발환경과 배포환경을 분리할 수 있다.
- production(배포모드, 기본값): 코드를 최적화해서 용량을 줄여준다. 로드 시간을 줄이기 위해 번들 최소화, 가벼운 소스맵 및 애셋 최적화에 초점을 맞춰 번들링.
- development(개발모드): 개발 생산성을 높이기 위한 모드로 안전한 로컬환경에서 개발한다는 가정하에, 개발에 편리한 최적화되지 않은 번들 파일을 제공한다. 버그발생 위험이 있는 코드를 미리 경고해 주는 검증 코드도 포함되어 있다.

```
npm install -D webpack-merge webpack-dev-server
```
- **webpack-merge:** 공통 파일과 다른 파일을 연동시키기 위한 라이브러리
- **webpack-dev-server:** 웹팩 개발 서버를 구동할 수 있게 한다. 실시간 리로딩을 지원 (npm run dev)

<hr class="sub">

### 🛠️ config 파일 분리

**webpack.config.dev.js**

```javascript
const { merge } = require('webpack-merge');
const baseConfig = require('./webpack.config.base');
const path = require('path');

module.exports = merge(baseConfig, {
  mode: 'development',
  devServer: {
    static: {
      directory: path.resolve(__dirname, 'docs'),
    },
    port: 3000,
    hot: true,
  },
  optimization: {
    runtimeChunk: 'single',
  }
});
```
- **static.directory:** 빌드 결과물 경로
- **port:** 서버 포트
- **hot:** true일 때 웹팩으로 빌드한 결과물 실시간으로 반영
- **runtimeChunk:** single일 때 공통 모듈이 여러번 초기화 되는 일을 방지해준다.

<br>

**webpack.config.prod.js**
```javascript
const { merge } = require('webpack-merge');
const baseConfig = require('./webpack.config.base');

module.exports = merge(baseConfig, {
  mode: 'production',
});
```
<hr class="sub">

### 🛠️ 실행 scripts 작성
```json
"scripts": {
    "build": "webpack --config webpack.config.prod.js",
    "dev": "webpack server --open --config webpack.config.dev.js",
  },
```
- **npm run build:** 배포용 
- **npm run dev:** CRA환경에서 `npm run start`와 같은 기능, 로컬 환경에서 빌드한 결과물을 보여준다.

<hr>

## 7. react-refresh-webpack-plugin
webpack-dev-server처럼 저장할때마다 변경사항을 적용시며주고, 리액트의 상태를 유지해주는 플러그인이다.
```
npm i -D @pmmmwh/react-refresh-webpack-plugin react-refresh
```

**webpack.config.dev.js**
```javascript
const ReactRefreshPlugin = require('@pmmmwh/react-refresh-webpack-plugin');

module.exports = merge(baseConfig, {
  // ... mode 생략
  // ... devServer 생략
  // ... optimization 생략
  plugins: [new ReactRefreshPlugin()],
});
```
<hr>

## 8. 컨벤션 통일
코드 작성 스타일을 통일하고 오류 방지를 위해 `eslint`와 `prettier` 설정 해주는 것이 좋다.

<br>

### 🔍 eslint
코드에 문제가 없는지 검사한다.
```
npm install -D eslint eslint-plugin-react @babel/eslint-parser
```
웹 접근성에 대한 부분을 알려준다.
```
npm install -D eslint-plugin-jsx-a11y
```

**scripts 추가**
```json
"lint": "eslink ./src",
```
**.eslintrc.js**
```javascript
module.exports = {
  parser: '@babel/eslint-parser',
  env: {
    browser: true,
    commonjs: true,
    es6: true,
    node: true,
  },
  extends: [
    'eslint:recommended',
    'plugin:react/recommended',
    'plugin:jsx-a11y/recommended',
  ],
  parserOptions: {
    ecmaFeatures: {
      jsx: true,
    },
    ecmaVersion: 2018,
    sourceType: 'module',
  },
  settings: {
    react: {
      version: '18.2.0',
    },
  },
  plugins: ['react'],
  rules: {
    'react/react-in-jsx-scope': 0,
    'react/jsx-uses-react': 0,
    'react/prop-types': 0,
  },
};
```
<hr>

### ✨ prettier
**라이브러리 설치** <br>
```
# 코드형식을 통일해준다.

npm install -D prettier
```

**scripts 추가**
```json
"pretty": "prettier --write ./",
```

**.prettierrc.js**
```javascript
module.exports = {
  singleQuote: true,
  jsxSingleQuote: true,
};
```

<hr>

## 💝 마무리
[배포링크](https://ychae1997.github.io/react-webpack/) <br><br> 
이번 과제를 하면서 CRA환경에서 `npm run start`, `npm run build` 같은 명령어들이 어떻게 실행될 수 있었는지 알 수 있었고, 무엇보다 CRA의 소중함을 느꼈다..😂 나에게 꼭 맞는 개발환경을 구축할 수 있도록 웹팩에 대해 좀 더 공부해 봐야 겠다.

>추가할 것들!!
- 더미 데이터
- favicon
- manifest
- font
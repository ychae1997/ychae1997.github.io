---
title: "[Webpack] Webpack Config"
excerpt: "웹팩의 핵심개념에 대해 알아보자"

categories:
  - Webpack
tags:
  - [Webpack, bundling]

permalink: /Webpack/Webpack Config

toc: true
toc_sticky: true

date: 2023-03-22
last_modified_at: 2023-03-22
---
<hr>

[Webpack 공식문서](https://webpack.kr/concepts/){:target="_blank"}를 보면 아래 항목을 핵심개념으로 소개하고 있다. 

>- Entry(엔트리)
- Output(출력)
- Loaders(로더)
- Plugins(플러그인)
- Mode(모드)
- Browser Compatibility(브라우저 호환성)

`webpack.config.js` 파일 예시를 보면서 위에서 언급한 개념에 대해 하니씩 알아보자

```javascript
module.exports = {
  target: ["web", "es5"],
  entry: "./src/script.js",
  output: {
    path: path.resolve(__dirname, "docs"),
    filename: "app.bundle.js",
    clean: true
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, "css-loader"],
        exclude: /node_modules/,
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, "src", "index.html"),
    }),
    new MiniCssExtractPlugin(),
  ],
  optimization: {
    minimizer: [
      new CssMinimizerPlugin(),
    ]
  }
};
```

<hr>

## Target
Webpack은 다양한 환경과 target을 컴파일한다. target의 기본값은 web이다.
이 부분에는 web 외에도 다양한 환경을 컴파일 할 수 있는데, esX를 넣으면 지정된 ECMAScript 버전으로 컴파일할 수 있다.

```javascript
module.exports = {
  
  target: 'node',

};
```
위 예제에서 node webpack을 사용하면 Node.js와 유사한 환경에서 사용할 수 있도록 컴파일된다.

```javascript
module.exports = {
  
  target: ["web", "es5"],

};
```
해당 config파일에는 es5를 배열 안에 넣었다. <br>
따라서 이 config파일은 브라우저와 동일한 환경에서 사용하기 위해 컴파일한 것이며,<br>
작성된 코드를 es5버전으로 컴파일하겠다고 지정한 것임을 알 수 있다. <br>
`Browser Compatibility`와 연관된 속성으로 볼 수 있다.

<hr>

## Entry
애플리케이션이 실행되며 Webpack이 번들링을 시작하는 곳이다. <br>
Entry point라고도 하며, webpack이 내부의 [디펜던시 그래프](https://webpack.kr/concepts/dependency-graph/){:target="_blank"}를 생성하기 위해 사용해야 하는 모듈이다. Webpack은 이 Entry point를 기반으로 직간접적으로 의존하는 다른 모듈과 라이브러리를 찾아낸다. 기본값은 `./src/index.js`이다.

```javascript
module.exports = {
  
  entry: "./src/script.js",

};
```

<hr>

## Output
Output 속성은 번들 결과물을 내보낼 경로를 설정하고, 파일의 이름을 지정하는 방법을 webpack에 알려주는 역할을 한다.
기본 출력 파일의 경우에는 `./dist/main.js`로 , 생성된 기타 파일의 경우에는 `./dist` 폴더로 설정된다.

```javascript
const path = require('path');

module.exports = {

  output: {
    path: path.resolve(__dirname, "docs"), // 절대 경로로 설정을 해야 합니다. 
    filename: "app.bundle.js",
    clean: true
  },

};

```
- **path:** 번들링된 결과물의 위치를 설정할 수 있다.
- **filename:** 번들링된 결과물의 이름을 설정할 수 있다.

<hr>

## Loaders
Webpack은 기본적으로 JavaScript와 JSON파일만 이해한다. 로더를 사용하면 webpack이 다른 유형의 파일을 처리하거나, 이를 애플리케이션에서 사용할 수 있는 모듈로 변환할 수 있다.

즉, Javascript파일이 아닌 HTML, CSS 등 웹 자원들을 변환해 사용할 수 있게 된다. <br>
**babel-loader, css-loader, style-loader, file-loader** 등이 있다.

```javascript
module.exports = {

  module: {
    rules: [
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, "css-loader"],
        exclude: /node_modules/,
      },
    ],
  },

};
```
- **test:** loader를 적용시킬 파일 유형 명시, 정규 표현식을 사용한다. (필수 속성)
- **use:** 해당 파일에 적용할 loader 명시 (필수 속성)
- **exclude:** 컴파일 하지 않을 파일이나 폴더 명시

<hr>

## Plugins
로더는 특정 유형의 모듈을 변환하는 데 사용되지만, 플러그인을 활용하여 번들을 최적화하거나, 애셋을 관리하고, 또 환경 변수 주입등과 같은 광범위한 작업을 수행 할 수 있다.

JS를 난독화하는 등, 플러그인은 해당 결과물의 형태를 바꿔준다. <br>
[plugin interface](https://webpack.js.org/plugins/){:target="_blank"}

플러그인을 사용하려면, 먼저 require()를 통해 플러그인을 요청하고 plugins 배열에 사용하고자 하는 플러그인을 추가한다.<br>
또한 다른 목적으로 플러그인을 여러 번 사용하도록 설정할 수 있기 때문에 `new` 연산자를 사용해 호출하여 플러그인의 인스턴스를 만들어줘야 한다.

```javascript
const webpack = require('webpack');
const HtmlWebpackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, "src", "index.html"),
    }),
    new MiniCssExtractPlugin(),
  ],

};
```
- `html-webpack-plugin:` 생성된 모든 번들을 자동으로 삽입하여 애플리케이션용 HTML 파일을 생성
-  `mini-css-extract-plugin:` CSS를 별도의 파일로 추출해 CSS를 포함하는 JS 파일 당 CSS 파일을 작성해주게끔 지원

<hr>

## Optimization
Webpack은 버전 4부터는 선택한 항목에 따라 **최적화**를 실행한다.

```javascript
module.exports = {
  
  optimization: {
    minimizer: [
      new CssMinimizerPlugin(), // 관련 번들을 최소화
    ]
  }

};
```
대표적으로 `minimize`와 `minimizer`등을 사용한다.
- **minimize:** `TerserPlugin` 또는 `optimization.minimize`에 명시된 plugins로 bundle 파일을 최소화(=압축)시키는 작업 여부를 결정
- **minimizer:** `defualt minimizer`을 커스텀된 `TerserPlugin` 인스턴스를 제공해서 재정의할 수 있다.

<hr>

## Mode
mode 파라미터를 `development`, `production` 또는 `none`으로 설정하면 webpack에 내장된 환경별 최적화를 활성화 할 수 있다. 기본값은 `production` 이다.

|옵션|설명|
|:--:|:--|
|`development`|	`DefinePlugin`의 `process.env.NODE_ENV`를 `development`로 설정한다. 모듈과 청크에 유용한 이름을 사용할 수 있다.|
|`production`|	`DefinePlugin`의 `process.env.NODE_ENV`를 `production`으로 설정한다. 모듈과 청크, `FlagDependencyUsagePlugin`, `FlagIncludedChunksPlugin`, `ModuleConcatenationPlugin`, `NoEmitOnErrorsPlugin`, `TerserPlugin` 등에 대해 결정적 망글이름(mangled name)을 사용할 수 있다.
|`none`|	기본 최적화 옵션에서 제외|

<br>

**mode: 'development'**
```javascript
// webpack.development.config.js
module.exports = {
  mode: 'development',
};
```
**mode: 'production'**
```javascript
// webpack.production.config.js
module.exports = {
  mode: 'production',
};
```
**mode: 'none'**
```javascript
// webpack.custom.config.js
module.exports = {
  mode: 'none',
};
```

webpack.config.js 안의 mode 변수에 따라 동작을 변경하려면, 객체 대신 함수를 export해야한다.

```javascript
var config = {
  entry: './app.js',
  //...
};

module.exports = (env, argv) => {
  if (argv.mode === 'development') {
    config.devtool = 'source-map';
  }

  if (argv.mode === 'production') {
    //...
  }

  return config;
};
```

<hr>

## Browser Compatibility
Webpack은 ES5가 호환되는 모든 브라우저를 지원한다(IE8 이하는 미지원). Webpack은 `import()` 및 `require.ensure()`을 위한 `Promise`를 요구합니다. 구형 브라우저를 지원하려면 이러한 표현식을 사용하기 전에 [폴리필](https://webpack.kr/guides/shimming/){:target="_blank"}을 로드해야 한다.
- [Webpack, Babel 사용해서 React 개발 환경 구축](#webpack-babel-사용해서-react-개발-환경-구축)
  - [CRA 란?](#cra-란)
  - [라이브러리 설치](#라이브러리-설치)
  - [webpack.config.js](#webpackconfigjs)
  - [HTML 파일 빌드](#html-파일-빌드)
    - [html-loader](#html-loader)
    - [html-webpack-plugin](#html-webpack-plugin)
    - [index.html 생성](#indexhtml-생성)
  - [Babel 설정](#babel-설정)
    - [babel-loader](#babel-loader)
    - [.babelrc](#babelrc)
  - [CSS 파일 빌드](#css-파일-빌드)
    - [css-loader](#css-loader)
    - [mini-css-extract-plugin](#mini-css-extract-plugin)
  - [개발 서버](#개발-서버)
  - [Development Mode, Production Mode](#development-mode-production-mode)
  - [정리](#정리)
  - [추가](#추가)

# Webpack, Babel 사용해서 React 개발 환경 구축

Webpack에 대한 기본 개념을 익혔다면, React를 사용하기 위해 적용해보자.

- Webpack에 대해 잘 모른다면, 이전 **[Webpack 기본 개념](https://github.com/FE-Lex-Kim/-TIL/blob/master/Tool/Webpack.md)** 보고 오기.

<br>

**직접 Webpack을 사용해서 React 프로젝트를 시작하는 방법에대해 알아보자.**

<br>

## CRA 란?

간단한 프로젝트를 하거나 Webpack에 대해 공부를 하지 않았다면, **CRA을 사용해서 React 사용한다.**

**CRA 사용했을때, 어떤 Webpack 설정이 있는지 간단하게 알아보자.**

<br>

CRA는 Create React App 의 약자이다.

우리가 배우려는 Webpack, Babel 뿐만 아니라 **jest, eslint, loader .. 등등 React 프로젝트에 많이 사용되는 툴들을 포함하고 있다.**

<br>

**CRA를 사용하지 않고 직접 Webpack을 사용하는 이유는 무엇일까?(간단하게)**

- 프로젝트에 **불필요한 디펜던시를 포함한다.**
- 프로젝트가 **어떤 구조로 이루어져 있는지 파악하기 어렵다.**
- **능동적으로 자유롭게 개발환경을 세팅할 수 없다.**

<br>

어느 정도 React를 할줄안다면, **직접 환경설정을 해야하는 이유에 대해 알아보았다.**

이제는 직접 Webpack을 사용해보자.

<br>

## 라이브러리 설치

먼저 **React에 필요한 기본적인 라이브러리를 설치하자.**

```bash
npm i react react-dom
```

<br>

**Webpack 시작하기 위한 라이브러리를** 설치해자.

```bash
npm install -D webpack webpack-cli webpack-dev-server
```

<br>

- `-D` : Webpack, Babel은 **개발 단계에서만 사용하게 한다.**
- webpack : **webpack을 설치한다.**
- webpack-cli : **webpack을 커맨드라인 인터페이스로** 실행하기위해 설치한다.
- webpack-dev-server : **개발서버용을** 위해서 설치한다.

<br>

## webpack.config.js

webpack이 실행했을때, **webpack.config.js을 참고해서 빌드한다.**

**최상위 디렉토리에 작성한다.**

<br>

webpack.config.js

```jsx
const path = require("path");

module.exports = {
  entry: "./src/index.js",
  output: {
    filename: "main.js",
    path: path.join(__dirname, "/dist"),
  },
  resolve: {
    extensions: [".jsx", ".js"],
  },
  mode: "development",
};
```

<br>

- `const path = require("path")` : **절대경로를 참조하기 위해 path을 불러왔다.**
- `entry: "./src/index.js"` : 해당 파일을 **해석하고 번들한다.**
- `filename: "main.js"` : 번들한 **결과물의 이름**
- `path: path.join(__dirname + "/dist")` :
  - `path.join` : **인자로 받은 경로들을 하나로 합쳐서** 문자열 형태로 path를 리턴한다.
    - `path.join('/foo', 'bar')` → Returns : `'/foo/bar'`
  - `(__dirname, "/dist")` : \_\_dirname은 현재 실행하는 파일의 절대경로이다. 현재 파일의 절대 경로에 `/dist`를 조합해, `/dist`에 결과물을 생성하도록한다.
- `resolve : { extensions : ['.jsx', '.js'], }` : import로 파일을 불러올때, 파일 확장자를 생략하면 `.js` → `.jsx` 순으로 자동으로 붙여주어 사용한다.
- `mode: "development"` : development 모드로 빌드한다.(뒤에서 자세히 나온다.)

<br>

entry로 index.js를 설정했으니, **index.js를 만들어보자.**

<br>

index.js

```jsx
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";

ReactDOM.render(
  <>
    <App />
  </>,
  document.getElementById("root")
);
```

<br>

App 컴포넌트도 불러왔으니, App 컴포넌트도 간단하게 만들어보자.

<br>

App.jsx

```jsx
import React from "react";

const App = () => {
  return <h3>Hello, Alex!🖐</h3>;
};

export default App;
```

<br>

## HTML 파일 빌드

Webpack은 **Javascript, JSON만 모듈로 해석한다.**

**HTML 파일을 모듈로 인식하게** 하고 번들하는 방법에 대해 알아보자.

<br>

### html-loader

Webpack이 HTML 파일을 **모듈로 인식하고 읽어서 로드할 수 있도록 해준다.**

<br>

설치

```bash
npm i -D html-loader
```

<br>

html-loader 적용해보자.

<br>

webpack.config.js

```jsx
const path = require("path");
const HtmlWebPackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: "./src/index.js",
  output: {
    filename: "main.js",
    path: path.join(__dirname, "/dist"),
  },
  resolve: {
    extensions: [".jsx", ".js"],
  },
  mode: "development",
  module: {
    rules: [
      {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader",
            options: { minimize: true },
          },
        ],
      },
    ],
  },
};
```

<br>

- `test: /\.html$/` : **html 파일들을 대상으로 html-loader을 사용한다.**
- `loader: "html-loader"` : **html-loader 사용.**
- `options: { minimize: true }` : minimize 옵션을 사용해 **파일이 한줄로 나오게해 최적화를 해준다.**

<br>

### html-webpack-plugin

**html-loader만 사용한다면, html 파일을 읽기만 할 뿐이다.**

html 파일을 로드한후, **번들한 결과를 빌드 폴더에 넣어주어야한다.**

html-webpack-plugin을 통해 **빌드하게 해준다.**

<br>

설치

```bash
npm i -D html-webpack-plugin
```

<br>

html-webpack-plugin을 적용해보자.

<br>

webpack.config.js

```jsx
const path = require("path");
const HtmlWebPackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: "./src/index.js",
  output: {
    filename: "main.js",
    path: path.join(__dirname, "/dist"),
  },
  resolve: {
    extensions: [".jsx", ".js"],
  },
  mode: "development",
  module: {
    rules: [
      {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader",
            options: { minimize: true },
          },
        ],
      },
    ],
  },
  plugins: [
    // <------------------------ 추가된 코드
    new HtmlWebPackPlugin({
      template: "./public/index.html",
      filename: "index.html",
    }),
  ],
};
```

<br>

- `template: './public/index.html'` : 해당 경로의 **html 파일을 템플릿으로 기준을 삼아서 빌드해준다.(index.html이 빌드 폴더에 그대로 들어간다.)**
- `filename: 'index.html'` : html **파일이름이 된다.**

<br>

생성된 html 파일은 **/dist 경로에 생성된다.**

이때 **html 파일 내부 코드에 자동으로 webpack이 번들한 JS 파일을 넣어준다.**

- **index.html을 보면 `<script type="text/javascript" src="index_bundle.js"></script>` body안에 들어가 있다.**

<br>

### index.html 생성

html-webpack-plugin이 `./public/index.html` 에 있는 **파일을 템플릿으로 삼았으니 만들어주면 된다.**

<br>

index.html

```jsx
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="utf-8" />
    <title>WEBPACK4-REACT</title>
  </head>
  <body>
    <noscript>스크립트가 작동되지 않습니다!</noscript>
    <div id="root"></div>
  </body>
</html>
```

<br>

## Babel 설정

이제 JS, HTML을 로드, 번들한뒤 하나의 파일로 만들게 했다.

Javascript을 크로스 브라우징 하기 위해 Babel을 설정해보자.

<br>

**Babel 시작하기 위한 라이브러리를** 설치해자.

```bash
npm -D babel-loader @babel/core @babel/preset-react @babel/preset-env
```

<br>

- babel-loader : Babel을 **불러온다.**
- @babel/core : Babel을 **사용할 수 있게한다.**
- @babel/preset-react : Babel을 사용해 **react을 해석해 es5로 변형시켜준다.**
  - JSX 코드를 `React.CreateElement`호출 코드로 변환.
- @babel/preset-env : 타깃 환경에 필요한 **구문 변환(syntax transform)**, **브라우저 폴리필(browser polyfill)** 을 제공한다.(es6 → es6로 변환)

<br>

babel-loader을 통해 babel을 불러와 @babel/core, @babel/preset-react, @babel/preset-env을 사용한다.

<br>

※ 추가

Babel 7.4.0부터 `@babel/polyfill`은 deprecated 되었다.

`@babel/plugin-transform-runtime` 추가해서 es6 폴리필 해준다.

promise, 빌트인 생성자 함수 메서드 사용 가능(만약 이것을 설치하지 않는다면, es6 코드를 사용할 수 없다. 따라서 반드시 설치해야한다.)

```jsx
"plugins": [
    "babel-plugin-styled-components",
    [
      "@babel/plugin-transform-runtime",
      {
        "absoluteRuntime": false,
        "corejs": 3,
        "helpers": true,
        "regenerator": true,
        "version": "7.0.0-beta.0"
      }
    ]
  ]
```

옵션으로 `corejs: 3` 버전으로 사용 할수 있다.

아래의 바벨 플러그인을 추가해야한다.

```bash
npm install -D @babel/plugin-transform-runtime
npm install @babel/runtime-corejs3
```

<br>

### babel-loader

babel-loader을 적용해보자.

<br>

webpack.config.js

```jsx
const path = require("path");
const HtmlWebPackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: "./src/index.js",
  output: {
    filename: "main.js",
    path: path.join(__dirname, "/dist"),
  },
  resolve: {
    extensions: [".jsx", ".js"],
  },
  mode: "development",
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: "/node_modules",
        use: ["babel-loader"], // <------------------------ 추가된 코드
      },
      {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader",
            options: { minimize: true },
          },
        ],
      },
    ],
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: "./public/index.html",
      filename: "index.html",
    }),
  ],
};
```

<br>

- `test: /\.(js|jsx)$/` : js, jsx 파일을 대상으로 한다.
- `exclude: "/node_modules"` : node_modules은 제외한다.

<br>

### .babelrc

위에서 보여진 **preset은 plugin들의 집합이다.**

**보일러 플레이트**와 같은 매번 프로젝트마다 사용되는 plugin들을 모아두어 **한번에 추가하여 사용하고 관리가 수월하게 한다.**

<br>

**preset을 사용해 적용하려면 .babelrc에 설정해주어야한다.**

<br>

.babelrc

```json
{
  "presets": ["@babel/preset-env", "@babel/preset-react"]
}
```

<br>

또는 추가적인 옵션을 넣어줄 수 있다.

<br>

.babelrc

```json
{
  "presets": [
    ["@babel/preset-env", { "targets": { "browsers": ["last 2 versions", ">= 5% in KR"] } }],
    "@babel/react"
  ],
  "plugins": [
    "babel-plugin-styled-components",
    [
      "@babel/plugin-transform-runtime",
      {
        "absoluteRuntime": false,
        "corejs": 3,
        "helpers": true,
        "regenerator": true,
        "version": "7.0.0-beta.0"
      }
    ]
  ]
}
```

<br>

**브라우저의 상위 버전 두개와 한국(KR)에서 5% 이상의 점유율을 가지고 있는 브라우저에 대응하여 컴파일되도록 설정 했다.**

**모든 브라우저를 지원하면 babel이 느려지게 된다.**

따라서 서비스 시, **지원하려는 브라우저를 정하고 그에 알맞은 조건으로 넣어준다.**

<br>

※ **브라우저 조건** 쉽게 알 수 있게 하는 [**browserslist 사이트**](https://github.com/browserslist/browserslist#full-list)를 보면 참고하기 쉽다.

**※ babel/preset-env의 여러 옵션은 [Babel 공식 홈페이지의 해당 챕터](https://babeljs.io/docs/en/babel-preset-env#targets)를 추천한다.**

**※ @babel/preset 에는 플러그인들로 react, env 이외에도 [typescript](https://babeljs.io/docs/en/babel-preset-typescript), [flow](https://babeljs.io/docs/en/babel-preset-flow) 이 있다.**

<br>

## CSS 파일 빌드

지금까지 JS, HTML을 번들할 수 있게 설정했다.

마찬가지로 스타일을 입히기 위한 **CSS도 번들할 수 있게 해야한다.**

<br>

### css-loader

**css-loader을 사용해서 번들할 수 있게한다.**

<br>

설치

```bash
npm i -D css-loader
```

<br>

css-loader을 적용해보자.

<br>

webpack.config.js

```jsx
const path = require("path");
const HtmlWebPackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: "./src/index.js",
  output: {
    filename: "main.js",
    path: path.join(__dirname, "/dist"),
  },
  resolve: {
    extensions: [".jsx", ".js"],
  },
  mode: "development",
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: "/node_modules",
        use: ["babel-loader"],
      },
      {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader",
            options: { minimize: true },
          },
        ],
      },
      {
        test: /\.css$/, // <------------------------ 추가된 코드
        use: ["css-loader"],
      },
    ],
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: "./public/index.html",
      filename: "index.html",
    }),
  ],
};
```

<br>

### mini-css-extract-plugin

CSS 파일을 **css-loader을 통해 읽기만 가능하다.**

**빌드 폴더에 저장할 수 있게해야한다.**

```bash
npm i -D mini-css-extract-plugin
```

<br>

webpack.config.js

```jsx
const path = require("path");
const HtmlWebPackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  entry: "./src/index.js",
  output: {
    filename: "main.js",
    path: path.join(__dirname, "/dist"),
  },
  resolve: {
    extensions: [".jsx", ".js"],
  },
  mode: "development",
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: "/node_modules",
        use: ["babel-loader"],
      },
      {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader",
            options: { minimize: true },
          },
        ],
      },
      {
        test: /\.css$/, // <------------------------ 추가된 코드
        use: [MiniCssExtractPlugin.loader, "css-loader"],
      },
    ],
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: "./public/index.html",
      filename: "index.html",
    }),
    new MiniCssExtractPlugin({
      filename: "style.css",
    }),
  ],
};
```

<br>

- `filename: 'style.css'` : css 파일들을 번들한 결과가 **style.css 이름으로 생성된다.**
- `use: [MiniCssExtractPlugin.loader,'css-loader']` : 오른쪽에서 왼쪽으로 loader가 실행된다. **css-loader로 css 파일을 읽고 MiniCssExtractPlugin.loader 을 통해 css 파일을 생성한다.**

<br>

※ 만약 React에서 **다른 방식의 Style을 적용한다면, 각각의 loader을 사용해야한다.**

- ex) sass 라면 sass-loader을 추가한다.

```jsx
{
	test: /\.scss$/,
	use: [MiniCssExtractPlugin.loader, "css-loader", "sass-loader"]
}
```

- 마찬가지로 sass-loader 부터 실행되고 css-loader 순으로 넘어간다.

<br>

## 개발 서버

**코드 수정시마다 build를 한다면 불편하다.**

코드가 수정됨을 **관찰하다가, 변경된다면 build가 되게 적용해주어야한다.**

<br>

설치

```bash
npm i -D webpack-dev-server
```

<br>

webpack.config.js

```jsx
const path = require("path");
const HtmlWebPackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  entry: "./src/index.js",
  output: {
    filename: "main.js",
    path: path.join(__dirname, "/dist"),
  },
  devServer: {
    // <------------------------ 추가된 코드
    contentBase: path.join("/dist"), // 만약 예상대로 실행되지 않으면 이부분 삭제해주기
    publicPath: "/dist/", // 만약 예상대로 실행되지 않으면 이부분 삭제해주기
    index: "index.html",
    open: true,
    port: 3000,
    hot: true,
  },
  resolve: {
    extensions: [".jsx", ".js"],
  },
  mode: "development",
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: "/node_modules",
        use: ["babel-loader"],
      },
      {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader",
            options: { minimize: true },
          },
        ],
      },
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, "css-loader"],
      },
    ],
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: "./public/index.html",
      filename: "index.html",
    }),
    new MiniCssExtractPlugin({
      filename: "style.css",
    }),
  ],
};
```

<br>

- `contentBase: path.join("/dist")` : 서버가 로드할 **빌드 폴더의 static 파일 경로, dist 폴더를 로드한다.**
- `publicPath: "/dist/"` : 개발 버전이라면 경로가 항상 일정해서 바로 찾아서 적용할 수 있다. 하지만 배포 버전에서는 다른 지정된 위치로 이동할 수있다. 따라서 수동으로 정해준다.
- `open : true` : devserver를 실행하면 자동으로 브라우저를 열어준다.
- `hot : true` : 코드가 변경되면, 감지하고 자동으로 build한다.
- `port: 3000` : 3000번 포트를 사용한다.

<br>

※ devServer 의 **여러 옵션은** [**Webpack 공식홈페이지의 DevServer 챕터**](https://webpack.js.org/configuration/dev-server/)을 추천한다.

<br>

**script 정해주자.**

<br>

package.json

```json
"scripts": {
	"dev": "webpack serve --mode development",
	"build": "webpack --mode production",
}
```

<br>

npm dev를 한다면 이제 코드가 수정됬을때, build가 알아서 진행된다.

<br>

## Development Mode, Production Mode

**웹팩의 모드는 Development, Production 이렇게 두 가지가 있다.**

<br>

- **Development는 빠르게 빌드하기 위해서 최적화를 진행하지 않는 상태이다.**
- **Production는 빌드시 최적화를 진행한 상태이다.**

<br>

**Script를 변경해서 적용해보자.**

<br>

package.json

```json
"scripts": {
	"dev": "webpack serve --mode development",
	"build": "webpack --mode production",
}
```

<br>

- `--mode development` : **mode을 development로 변경.**
- `--mode production` : **mode을 production로 변경.**
- `--open` : npm start후 **브라우저가 자동으로 열린다.**
- `--hot` : **코드가 변경되면, 감지하고 자동으로 build한다.**

<br>

## 정리

<br>

1.  src, public 폴더, indext.html 파일 생성
2.  npm init
3.  `npm i react react-dom`
4.  `npm i -D webpack webpack-cli webpack-dev-server`
5.  webpack.config.js 생성

    ```jsx
    const path = require("path");
    const HtmlWebPackPlugin = require("html-webpack-plugin");

    module.exports = {
      entry: "./src/index.jsx",
      devServer: {
        hot: true,
        open: true,
        port: 3000,
      },
      mode: "development",
      module: {
        rules: [
          {
            test: /\.html$/,
            use: [
              {
                loader: "html-loader",
                options: { minimize: true },
              },
            ],
          },
          {
            test: /\.jsx?/,
            exclude: "/node_modules",
            use: ["babel-loader"],
          },
        ],
      },
      plugins: [
        new HtmlWebPackPlugin({
          template: "./public/index.html",
          filename: "index.html",
        }),
      ],
      resolve: {
        extensions: [".jsx", ".js"],
      },
      output: {
        filename: "main.js",
        path: path.join(__dirname, "/dist"),
      },
    };
    ```

6.  `npm i -D html-webpack-plugin html-loader`
7.  `npm i -D babel-loader @babel/core @babel/preset-react @babel/preset-env`
8.  `npm install -D @babel/plugin-transform-runtime npm install @babel/runtime-corejs3`
9.  .babelrc 파일 생성

    ```json
    {
      "presets": [
        ["@babel/preset-env", { "targets": { "browsers": ["last 2 versions", ">= 5% in KR"] } }],
        "@babel/react"
      ],
      "plugins": [
        "babel-plugin-styled-components",
        [
          "@babel/plugin-transform-runtime",
          {
            "absoluteRuntime": false,
            "corejs": 3,
            "helpers": true,
            "regenerator": true,
            "version": "7.0.0-beta.0"
          }
        ]
      ]
    }
    ```

10. package.json scripts 수정

    ```jsx
    "scripts": {
    	"dev": "webpack serve --mode development",
    	"build": "webpack --mode production",
    }
    ```

11. jsconfig.json 파일 생성
    `json { "compilerOptions": { "target": "es6" } } `

<br>

## 추가

- 다른 라이브러리 사용시 추가적으로 웹팩 설정을 해야한다.
- CRA도 버전을 거치면서 발전해와서, 좋은 보일러 플레이트로 현업에서도 사용하는 경우가 있다고 한다.
- **위의 방법이 공식이 아니라 만약 Webpack을 사용하면서 에러가 많이 발생할텐데, 그럴때마다 그에 따른 에러를 해결하는 방식으로 loader, plugins을 설치하면된다.**

<br>

참고

- [https://webpack.kr/loaders/babel-loader/](https://webpack.kr/loaders/babel-loader/)
- [https://babeljs.io/docs/en/babel-core](https://babeljs.io/docs/en/babel-core)
- [https://velog.io/@jeff0720/React-개발-환경을-구축하면서-배우는-Webpack-기초](https://velog.io/@jeff0720/React-%EA%B0%9C%EB%B0%9C-%ED%99%98%EA%B2%BD%EC%9D%84-%EA%B5%AC%EC%B6%95%ED%95%98%EB%A9%B4%EC%84%9C-%EB%B0%B0%EC%9A%B0%EB%8A%94-Webpack-%EA%B8%B0%EC%B4%88)
- [https://berkbach.com/웹팩-webpack-과-바벨-babel-을-이용한-react-개발-환경-구성하기-fb87d0027766](https://berkbach.com/%EC%9B%B9%ED%8C%A9-webpack-%EA%B3%BC-%EB%B0%94%EB%B2%A8-babel-%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-react-%EA%B0%9C%EB%B0%9C-%ED%99%98%EA%B2%BD-%EA%B5%AC%EC%84%B1%ED%95%98%EA%B8%B0-fb87d0027766)
- [https://dev-yakuza.posstree.com/ko/react/start/](https://dev-yakuza.posstree.com/ko/react/start/)
- [https://velog.io/@pop8682/번역-React-webpack-설정-처음부터-해보기#6-babelrc](https://velog.io/@pop8682/%EB%B2%88%EC%97%AD-React-webpack-%EC%84%A4%EC%A0%95-%EC%B2%98%EC%9D%8C%EB%B6%80%ED%84%B0-%ED%95%B4%EB%B3%B4%EA%B8%B0#6-babelrc)
- [https://krpeppermint100.medium.com/js-react와-webpack-6ba46f84b327](https://krpeppermint100.medium.com/js-react%EC%99%80-webpack-6ba46f84b327)
- [https://hoony-gunputer.tistory.com/entry/webpack-devServer](https://hoony-gunputer.tistory.com/entry/webpack-devServer)

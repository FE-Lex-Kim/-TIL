# Webpack

Javascript 개발을 수월하게 하기 위해 **모듈을 나누어서 작업한다.**

여기서 모듈이라는 것은 **Javascript, CSS, Images.. 등등 이러한 모든 것들을 말한다.**

만약 배포를 한뒤, **이러한 수많은 파일을 로드한다면, 네트워크가 많이 발생할 것이다.**

이러한 **여러 모듈들을 하나로 묶어주는 것을 번들러라고** 하고 그 중에서 가장 많이 쓰이는 Webpack이 있다.

<br>

이렇게 많은 파일을 아무리 압축하고 최적화를 한다고 해도 **하나의 파일로 번들한다면 초기 로딩 속도가 엄청 느릴것이다.**

이런 문제를 해결하기 위해 **코드 스플릿, 캐시, 청크를 사용해 해결한다.**

<br>

**Webpack에 대해 간단하게 알아보고** 더 자세한 내용은 **[Webpack 공식홈페이지](https://webpack.kr/)** 를 추천한다. 한글로 잘 번역도 되어 있으니 공부하기 수월하다.

<br>

## Webpack 핵심 개념

<br>

### Entry

**웹 리소스를 로드하기위한 최초 진입점이다.**

**Javscript 파일경로를** 이곳에 적어주면된다.

<br>

웹팩은 entry를 통해 **모듈을 로드하고 이곳에 있는 파일을 번들해준다.**

entry의 기본값은 `./src/index.js` 이다.

물론 위의 **기본 경로를 설정에서 변경 가능하다.**

<br>

**webpack.config.js (\***webpack.config.js으로 설정해야 Webpack이 인식한다.)\*

```jsx
module.exports = {
  entry: './src/index.js
};
```

위의 설정은 index.js을 대상으로 번들을 수행한다.

<br>

Webpack 설정에서 다른 경로를 설정하거나 **여러개의 진입점(Entry) 설정도 가능하다.**

```jsx
module.exports = {
  entry: {
    home: "./home.js",
    about: "./about.js",
    contact: "./contact.js",
  },
};
```

키가 home, about, contact로 되어 있어서 **값으로 들어간 파일경로를 번들한뒤**, **번들된 파일이 3개가 나오게된다.**

**키의 이름대로 결과물이 나온다.(output에서 설정시)**

- home.js, about.js, contact.js

<br>

이렇게 **여러개의 결과물을 만들때는 MPA(Multiple Page Application)일때 사용된다.**

- MPA는 **여러 개(Single)의 Page로 구성된 Application**이다.
- 새로운 페이지를 요청할 때마다 **정적 리소스가 다운로드된다.**
- **SSR(Server Side Rendering) 방식으로 렌더링한다고** 말한다.

<br>

배열을 사용해서 **하나의 Entry에 여러개의 파일을** 만들 수 있다.

```jsx
module.exports = {
  entry: {
    app: ["./home.js", "./about.js", "./contact.js"],
  },
};
```

home.js, about.js, contact.js을 번들한뒤 app.js로 나오게 된다.

<br>

npm 모듈을 배열안에 넣게 된다면, **npm 모듈이 적용된후 결과물이 나오게된다.**

```jsx
module.exports = {
  entry: {
    app: ["@babel/polyfill", "./home.js", "./about.js", "./contact.js"],
  },
};
```

app.js는 @babel/polyfill이 적용된다.

<br>

Entry에 대한 자세한 설명은 **[공식홈페이지의 Entry 챕터](https://webpack.kr/concepts/entry-points/)** 를 추천한다.

<br>

### Output

entry로 찾은 모듈들을 번들하고 만든 **결과물을 반환할 위치이다.**

<br>

**기본 값으로 `./dist/main.js` 이다.**

만약 번들과정에서 생성된 **기타 파일들은 `./dist` 폴더에 반환된다.**

<br>

**webpack.config.js**

```jsx
const path = require("path");

module.exports = {
  entry: "./path/to/my/entry/file.js",
  output: {
    path: "./dist",
    filename: "[name]-my-first-webpack.bundle.js",
  },
};
```

**path : 번들된 결과물을 반환활 위치이다.**

**filename : 번들된 결과물의 파일 이름이다. filename 으로 들어갈 여러가지 옵션이 있다.**

- [name] : Entry에 적었던 key 이름이 이곳에 들어가게 된다.
  - 위의 예제에서는 **app**-my-first-webpack.bundle이 되게 된다.
- [hash] : 매번 컴파일이후에 **랜덤으로 문자열을 붙여준다.**
- [chunkhash] : [hash]는 컴파일 이후 마다 변경된다면, [chunkhash]는 **파일이 변경됬을때만 랜덤 문자열이 붙여진다.**
  - 만약 해당 파일이 업데이트되지 않았다면, **브라우저 캐시에 저장된 파일을 그대로 쓰기 때문에, 로딩 성능이 향상된다.**

<br>

Output에 대한 자세한 설명은 **[공식홈페이지의 Output 챕터](https://webpack.kr/concepts/output/)** 를 추천한다.

<br>

### Loader

웹팩은 기본적으로 **JSON, Javascript만 이해가능하다.**

Javascript가 아닌 **다른 리소스(HTML, CSS, IMG.. 등등)도 이해하고 번들링이 가능하게 한다.**

<br>

TypeScript와 같은 언어를 Javascirpt로 변환시켜주거나, JS 모듈에서 CSS 파일을 Import 하는 작업이 가능하게 한다.

<br>

먼저 사용할 loader을 설치한후 사용한다.

예를 들어

```bash
npm install --save-dev css-loader ts-loader
```

<br>

사용방법을 알아보자.

**webpack.config.js**

```jsx
module.exports = {
  module: {
    rules: [
      { test: /\.css$/, use: "css-loader" },
      { test: /\.ts$/, use: "ts-loader" },
    ],
  },
};
```

<br>

`module.rules` 배열 안에 객체로 **각각 파일과 로더를 설정을 해준다.**

- **test** : 변환이 필요한 **파일을 식별하는 속성이다.**
- **use** : 변환을 수행하는 **로더를 가리키는 속성이다**

<br>

위의 예제에서

- `.css` 파일을 css-loader을 통해 변환시켜준다.
- `.ts` 파일을 ts-loader을 통해 변환시켜준다.

<br>

**여러 개의 로더를 지정하는 방법도 있다.**

```jsx
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          // [style-loader](/loaders/style-loader)
          { loader: "style-loader" }, // <------------- 로더 설정
          // [css-loader](/loaders/css-loader)
          {
            loader: "css-loader", // <------------- 로더 설정
            options: {
              modules: true,
            },
          },
          // [sass-loader](/loaders/sass-loader)
          { loader: "sass-loader" }, // <------------- 로더 설정
        ],
      },
    ],
  },
};
```

<br>

use 속성에서 **여러개의 loader을 적용하고 순서를 지정할 수 있다.**

**가장 마지막에 작성한 loader가 제일 먼저 평가/실행 된다.**

- 코드상에서 **오른쪽에서 왼쪽으로** 또는 **아래에서 위로**

<br>

위의 예제에서는

1. sass-loader
2. css-loader
3. style-loader

**가 순서 대로 실행된다.**

<br>

loader에 대한 더 자세한 내용은 [**공식홈페이지의 Loader 챕터**](https://webpack.kr/concepts/loaders/)를 추천한다.

<br>

### Plugin

Webpack의 기본동작 이외 **추가적인 기능을 제공하는 속성이다.**

즉, Plugin은 Webpack의 **결과물을 바꾸는 역활을 한다.**

<br>

**로더는 특정 유형의 모듈만 변환했다.**

플러그인은 **번들을 최적화**, Asset 관리, 환경 변수 추가.. 등등 **다양한 기능을 수행하게 한다.**

<br>

**webpack.config.js**

```jsx
const HtmlWebpackPlugin = require("html-webpack-plugin"); // npm을 통해 설치
const webpack = require("webpack"); // 내장 plugin에 접근하는 데 사용

module.exports = {
  module: {
    rules: [{ test: /\.txt$/, use: "raw-loader" }],
  },
  plugins: [new HtmlWebpackPlugin({ template: "./src/index.html" })],
};
```

위의 예제는 html-webpack-plugin을 모든 모듈에 자동으로 추가하여 HTML 파일을 생성한다.

<br>

여러가지 plugin 목록을 [**공식홈페이지**](https://webpack.kr/concepts/plugins/)를 통해 보는것을 추천한다.

<br>

### Mode

<br>

**development, production , none**으로 설정하면 **webpack에 내장된 환경별 최적화**를 활성화 한다.

**기본값은 production 이다.**

배포용이면 알아서 최적화가 적용된다.

<br>

```jsx
module.exports = {
  mode: "production",
};
```

<br>

[**Mode에 대한 자세한 설명**](https://webpack.kr/configuration/mode/)

<br>

### 정리

**자세한 내용들까지 모두 공부 하지 않고**

**나중에 필요한 내용만 쏙쏙 뽑아서 공부하기 위해 Webpack에 대해 큰 개념을 부터 공부해봤다.**

<br>

**공식홈페이지도 보고 여러 블로그, 사이트를 보고난후 굉장히 많은 기능들이 있고 복잡하다는것을 알았다.**

**React에서 CRA을 실행하는 것은 굉장히 편하지만, 이제는 성능도 고민해야할 시기가 왔다.**

**좀 더 Webpack에 대해 자세히 공부해야하는 시간을 가져야할것같다.**

<br>

참고

- [https://webpack.kr/](https://webpack.kr/)
- [https://www.zerocho.com/category/Webpack/post/58aa916d745ca90018e5301d](https://www.zerocho.com/category/Webpack/post/58aa916d745ca90018e5301d)
- [https://joshua1988.github.io/webpack-guide/](https://joshua1988.github.io/webpack-guide/)
- [https://berkbach.com/웹팩-webpack-과-바벨-babel-을-이용한-react-개발-환경-구성하기-fb87d0027766](https://berkbach.com/%EC%9B%B9%ED%8C%A9-webpack-%EA%B3%BC-%EB%B0%94%EB%B2%A8-babel-%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-react-%EA%B0%9C%EB%B0%9C-%ED%99%98%EA%B2%BD-%EA%B5%AC%EC%84%B1%ED%95%98%EA%B8%B0-fb87d0027766)
- [https://hanamon.kr/spa-mpa-ssr-csr-장단점-뜻정리/](https://hanamon.kr/spa-mpa-ssr-csr-%EC%9E%A5%EB%8B%A8%EC%A0%90-%EB%9C%BB%EC%A0%95%EB%A6%AC/)

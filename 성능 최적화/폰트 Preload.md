# 폰트 Preload

**폰트 파일을 CSS 파일보다 먼저 로드해서** 기본 폰트가 먼저 나오고 적용되게 하지 않을 수 있다.(사용자 경험 향상)

**즉, 폰트 파일을 먼저 로드해서 FOUT를 방지하는 방법이다.**

<br>

방법은 그리 어렵지 않다.

HTML 파일에서 CSS 파일을 로드하는 Element 보다 먼저 폰트를 로드하는 Element를 작성하면된다.

```jsx
<link rel="preload" href="./nanumGothic.woff2" as="font" type="font/woff2" crossorigin="anonymous">
```

<br>

CSS, 파일보다 먼저 로드하기 때문에 렌더링이 더 빨리끝난다.(렌더링 속도가 증가하지만, 로딩 성능은 그대로 이다.)

<br>

preload 옵션을 지원하지 않는 브라우저들이 있다.

![폰트 preload](./../Images/폰트%20preload/폰트%20preload-1.png)

<br>

또한 단점으로 **preload로 다운로드 하는 리소스가 많아질수록, 로딩 성능이 늦어져 렌더링 시간이 늦어진다는 점이 있다.**

<br>

따라서 만약 텍스트의 중요도가 높다면 폰트를 preload 하고, **상황에 맞춰서 적용하는것이 좋을듯 싶다.**

<br>

### **preload-webpack-plugin**

webpack을 사용하면 폰트 파일 이름이 다르게 나온다.

**빌드 과정에서 폰트 파일 이름이 달라 preload 하지 않게 된다.**

**[preload-webpack-plugin](https://www.npmjs.com/package/preload-webpack-plugin) 을 사용해 올바르게 폰트 파일을 선택해서 불러올 수 있다.**

<br>

설치

```bash
npm i preload-webpack-plugin
```

<br>

Webpack config 안에 불러오자.

```jsx
const PreloadWebpackPlugin = require("preload-webpack-plugin");
```

<br>

**webpack.config.js**

```jsx
plugins: [
  new HtmlWebpackPlugin(),
  new PreloadWebpackPlugin({
    rel: "preload",
    as: "font",
    include: "allAssets",
    fileWhitelist: [/(.woff2?)/i],
  }),
];
```

<br>

이제 빌드할때 폰트 파일 이름이 올바르게 들어가서 로드하게된다.

<br>

**만약 CRA를 사용한다면, eject 하기 복잡하므로 [react-app-rewired](https://www.npmjs.com/package/react-app-rewired) 을 통해 적용해보자.**

<br>

참고

- 인프런 프론트엔드 최적화 강의
- [https://d2.naver.com/helloworld/4969726](https://d2.naver.com/helloworld/4969726)
- [https://maternalgrandfather.tistory.com/entry/preload-defer-이용해서-티스토리-속도-향상시키기](https://maternalgrandfather.tistory.com/entry/preload-defer-%EC%9D%B4%EC%9A%A9%ED%95%B4%EC%84%9C-%ED%8B%B0%EC%8A%A4%ED%86%A0%EB%A6%AC-%EC%86%8D%EB%8F%84-%ED%96%A5%EC%83%81%EC%8B%9C%ED%82%A4%EA%B8%B0)
- [https://www.npmjs.com/package/preload-webpack-plugin](https://www.npmjs.com/package/preload-webpack-plugin)

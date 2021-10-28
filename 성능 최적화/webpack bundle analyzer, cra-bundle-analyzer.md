# webpack bundle analyzer, cra-bundle-analyzer

## webpack bundle analyzer

**webpack bundle analyzer**는 웹팩을 통해 번들링해서 나오는 파일의 사이즈를 시각적으로 보여주는 도구이다.

<br>

![webpack bundle analyzer](../Images/bundle%20analyer/bundle%20analyer-1.png)

<br>

### 장점

1. 번들이 어떻게 구성되어 있는지 파악할 수 있음
2. 번들에서 큰 사이즈를 차지하는 모듈이 어떤것인지 알 수 있음
3. 실수로 들어간 모듈이 어떤것인지 찾을 수 있음
4. 최적화 해줌

<br>

설치방법

```bash
npm install --save-dev webpack-bundle-analyzer
```

<br>

적용방법

```jsx
const BundleAnalyzerPlugin = require("webpack-bundle-analyzer")
  .BundleAnalyzerPlugin;

module.exports = {
  plugins: [new BundleAnalyzerPlugin()],
};
```

<br>

위의 적용방법은 webpack config를 수정해야한다.

만약 CRA를 사용하는 프로젝트라면, Eject를 통해 설정 해주어야한다.

<br>

이러한 번거로움을 해결해서 나온것이 **cra-bundle-analyzer**이다.

<br>

## **cra-bundle-analyzer**

<br>

설치방법

```bash
npm install --save-dev cra-bundle-analyzer
```

<br>

사용방법

```bash
npx cra-bundle-analyzer
```

<br>

실행을 하면 아래와 같은 HTML 파일이 보여진다.

![webpack bundle analyzer](../Images/bundle%20analyer/bundle%20analyer-2.png)

<br>

chunk.js 파일의 이름은 빌드 될때마다 달라지기 때문에, 이름이 다르다고 헷갈려하지 않아도된다.

node_modules의 에서 refractor 모듈이 가장 큰 부분을 차지 하는것을 시각적으로 볼 수 있다.

따라서 최적화를 할때 refractor 모듈을 하면 된다는 것을 파악할 수 있다.

<br>

## 정리

**webpack bundle analyzer 또는 cra-bundle-analyzer**을 통해서 어떤 모듈이 큰 사이즈를 차지하는지 파악할 수 있다.

큰 사이즈를 차지하는 모듈을 성능 최적화하는 부분을 고민하면 되기에 한눈에 파악하기 쉽다.

<br>

참고

- 인프런 프론트엔드 최적화 강의
- [https://www.npmjs.com/package/cra-bundle-analyzer](https://www.npmjs.com/package/cra-bundle-analyzer)
- [https://www.npmjs.com/package/webpack-bundle-analyzer](https://www.npmjs.com/package/webpack-bundle-analyzer)

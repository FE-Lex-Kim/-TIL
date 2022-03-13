- [webpack 이미지, 폰트 loader](#webpack-이미지-폰트-loader)
    - [file-loader, url-loader](#file-loader-url-loader)
    - [webpack 이미지, 폰트 적용](#webpack-이미지-폰트-적용)

# webpack 이미지, 폰트 loader

webpack에서는 JS,JSON 파일을 제외하고 모두 로더를 통해 파일을 번들링한다.

**이미지, 폰트 또한 파일이기 때문에 loader를 설치해서 번들링해야한다.**

<br>

### file-loader, url-loader

이미지, 폰트 파일 loader는 file-loader, url-loader를 사용해서 번들링한다.

<br>

설치

```bash
npm i -D file-loader, url-loader
```

---

- file-loader: Webpack에서 **실제 사용하는 파일을 복사하는데 사용된다.**
- url-loader: Webpack에서 **파일 사이즈가 작은 파일을 번들 파일로 만드는데 사용합니다.**
  → 사이즈가 작은 파일은 [data URL Scheme](https://pilot376.tistory.com/3) 형식으로 만들어준다.
  - data URL Scheme란? : **이미지 파일을 따로 저장하지 않고, base64 형태로 인코딩해서 문자열로 만든다.** 해당 문자열 자체가 이미지이기 때문에 따로 http요청을 하지 않아도 된다는 장점이 있다.

<br>

webpack loader 설정(폰트)

webpack.config.js

```jsx
{
  test: /\.(woff|woff2|eot|ttf|otf)$/i,
  use: [
    {
      loader: 'file-loader',
      options: {
        name: 'assets/fonts/[name].[ext]',
      },
    },
  ],
},
```

폰트 파일이 번들링되어 생성되는 경로는 `dist/assets/fonts/` 가 된다.

<br>

### webpack 이미지, 폰트 적용

이미지, 폰트를 loader 통해 파일을 번들링 했지만, 실제로 적용이 안될 수 도 있다.

- 이미지, 폰트를 적용하기 위해서는 **실제 개발 경로와 build한후 경로가 다르기 때문이다.**
- 따라서 **build 한후에 생겨난 이미지,폰트의 경로를 접근해야한다.**

<br>

위의 웹팩 설정에서는 `dist/assets/fonts/` 경로에 파일이 번들링되어 생성된다.

따라서 폰트를 찾는 경로 또한 위와 같은 경로를 찾아야한다.

<br>

**주의할점은 file-loader는 request, import를 사용할때 해당 url(경로)에 있는 폰트 or 이미지를 찾아서 번들링한다.**

- request, import를 하지 않은 폰트,이미지는 번들링 하지 않는다.
  ```css
  @font-face {
    font-family: "test";
    src: url(${request("./assets/fonts/BhuTukaExpandedOne-Regular.ttf")});
    font-weight: 100;
  }
  ```

<br>

1. 위와 같이 `request('./assets/fonts/BhuTukaExpandedOne-Regular.ttf')` 경로로 폰트 파일을 찾는다.
   - 위의 코드에서는 request를 사용했지만, 최상단 코드에서 import해서 불러와도 된다.
     ```css
     import test from "./assets/fonts/BhuTukaExpandedOne-Regular.ttf" @font-face {
       font-family: "test";
       src: url(test);
       font-weight: 100;
     }
     ```
2. 웹팩 빌드시 **폰트 파일을 번들링해`dist/assets/fonts/` 경로에 파일이 번들링된다.**
3. **폰트를 불러온 코드가 있는 파일을 기준으로** `'./assets/fonts/BhuTukaExpandedOne-Regular.ttf'` 경로에 폰트 파일을 찾는다.
   - 만약 file-loader에 옵션값으로 publicPath를 주면 위의 경로에서 가장 앞에 붙여서 들어간다.
   - 예를 들어) `publicPath : /dist/` 라면 `‘/dist/assets/fonts/BhuTukaExpandedOne-Regular.ttf'` 이 번들된후 파일을 찾는 경로이다.
   - 만약 css 파일에서 폰트를 찾는다면, 빌드된후 css 파일을 기준으로 찾는다.
   - 만약 styledComponent 같이 js 파일 안에 들어있다면, 해당 js 파일을 기준으로 찾는다.
4. **webpack에서 hot-loading-module 기능을 사용하고 있다면, request의 반환값은 객체형식이 된다. 반환한 객체의 default 프로퍼티에 불러온 경로가 들어가 있다.(핫로딩모듈의 특징)**

   - 따라서 dev server에서 hot-loading-module 기능을 사용한다면, request.defalut로 바꾸어서 넣어야한다.

   ```css
   @font-face {
     font-family: "test";
     src: url(${request("./assets/fonts/BhuTukaExpandedOne-Regular.ttf").default});
     font-weight: 100;
   }
   ```

<br>

참고

- [https://dev-yakuza.posstree.com/ko/react/image-and-font/](https://dev-yakuza.posstree.com/ko/react/image-and-font/)
- [https://fgh0296.tistory.com/12](https://fgh0296.tistory.com/12)
- [https://jeonghwan-kim.github.io/js/2017/05/22/webpack-file-loader.html](https://jeonghwan-kim.github.io/js/2017/05/22/webpack-file-loader.html)\
- [https://pilot376.tistory.com/3](https://pilot376.tistory.com/3)

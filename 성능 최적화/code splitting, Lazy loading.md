# code splitting, Lazy loading

<br>

**webpack**과 같은 도구를 사용해서 여러 파일을 하나로 병합한 **번들된 파일을 만들어서, 한번에 전체 앱을 로드** 할 수 있게해준다.

<br>

예를들면

```jsx
// math.js
export function add(a, b) {
  return a + b;
}
```

```jsx
// app.js
import { add } from "./math.js";

console.log(add(16, 26)); // 42
```

<br>

app.js 파일은 add을 불러와서 사용한다.

이렇게 app.js, math.js 파일이 두 개로 분리되어 있다.

하지만 번들 된 파일은 다음과 같이 구성되어진다.

```jsx
// bundle
function add(a, b) {
  return a + b;
}

console.log(add(16, 26)); // 42
```

<br>

## code splitting

<br>

번들링을 통해 파일을 한번에 로드 하게되어 효과적으로 사용감을 높인다.

<br>

하지만 앱의 크기가 커지게 된다면, 번들된 파일의 사이즈도 굉장히 커진다.

최초로 들어간 페이지의 코드 뿐만이 아니라, 다른 페이지의 코드까지도 한번에 로드하기 때문에, 로드 시간이 오래 걸리게 된다.

<br>

code splitting은 번들링된 파일이 거대해져 로드되는 시간이 오래걸리는 되기 때문에,

**방지하기 위한 방법으로 번들을 나누는 것이다.**

<br>

첫 페이지에 진입시에만 필요한 코드를 받고, 특정 다른 페이지에 진입시 그때 코드를 받게 하는 방법을 쓰면 성능이 향상될 것이다.

<br>

이러한 방식을 **lazy-load**이라고 한다.

<br>

장점

- lazy loading을 통해 성능 향상
- 앱의 코드를 줄이지 않고 해당 페이지에 필요하지 않은 코드를 불러오지 않음
  - 그로인한 초기 앱 성능 향상

<br>

### **Dynamic Imports**

<br>

특정 조건에 따라서 import 할 수 있다.

```jsx
let boolean = true;
if (boolean) {
  import("./alex.js");
} else {
  import("./james.js");
}
```

<br>

```jsx
import("./math").then((math) => {
  console.log(math.add(16, 26));
});
```

<br>

dynamic import의 반환값은 promise객체가 나온다.

<br>

Babel과 사용하는경우 **babel/plugin-syntax-dynamic-import**을 사용해야한다.
[더 자세한 설명](https://babeljs.io/docs/en/babel-plugin-syntax-dynamic-import)

<br>

사용예시

- 특정 화면위치 할때 import
- 사용자가 클릭할때 import
- 거대한 JS를 여러 JS로 나눔
- 다른 페이지로 라우팅될때 import

<br>

## React.lazy

동적 import를 사용하는 것은 은근 귀찮다.

고맙게도 React에서는 React.lazy를 지원해준다.

**React.lazy 함수를 사용하면 동적 import를 사용해서 컴포넌트를 렌더링 할 수 있다.**

<br>

React.lazy 함수안에는 콜백함수를 받는다.

해당 콜백함수안에는 import를 호출하는 로직을 가지게 된다.

<br>

이곳에서 컴포넌트, 다른 import을 받아 특정 조건에서도 사용 할 수 있다.

```jsx
import React, { lazy, Suspense } from "react";
import { Route, Switch } from "react-router-dom";

// import LazyCountOne from "./ReactLazy/LazyCountOne.jsx";
// import LazyCountTwo from "./ReactLazy/LazyCountTwo.jsx";
const LazyCountOne = lazy(() => import("./ReactLazy/LazyCountOne.jsx"));
const LazyCountTwo = lazy(() => import("./ReactLazy/LazyCountTwo.jsx"));

function App() {
  return (
    <>
      <Suspense fallback={<div>Lodaing...</div>}>
        <Switch>
          <Route path="/" component={LazyCountOne} exact />
          <Route path="/two" component={LazyCountTwo} exact />
        </Switch>
      </Suspense>
    </>
  );
}

export default App;
```

위와 같이 코드 최상단에서 import를 하는 구문을 lazy함수를 호출해사용한다.

이제부터는 LazyCountOne, LazyCountTwo 컴포넌트는 해당 라우터로 이동할때, import를 실행한다.

<br>

이렇게 최초에 모든 페이지를 로드받지 않고, 각각 페이지에 들어갈때 로드를 한다.

<br>

### code splitting 적용할때

<br>

**A페이지 진입시**

![code splitting, Lazy loading](./../Images/code%20splitting,%20Lazy%20loading/code%20splitting,%20Lazy%20loading-3.png)

<br>

**A 페이지에서 B 페이지로 진입시**

![code splitting, Lazy loading](./../Images/code%20splitting,%20Lazy%20loading/code%20splitting,%20Lazy%20loading-4.png)

<br>

A페이지와 B페이지를 따로 code splitting을 진행한후

network 패널에서 js를 요청한것을 본것이다.

<br>

A페이지에서 B페이지로 진입시 **1.chunk.js**와 **4.chunk.js**가 새롭게 다시 생겼다.

A페이지와 B페이지가 따로 코드가 번들링 되어있어,

B페이지로 접근했을때, B페이지인 **1.chunk.js**와 **4.chunk.js을 요청했다.**

<br>

### code splitting 적용 하지 않았을때

<br>

A페이지

![code splitting, Lazy loading](./../Images/code%20splitting,%20Lazy%20loading/code%20splitting,%20Lazy%20loading-5.png)

<br>

A페이지에서 B페이지로 접근시

![code splitting, Lazy loading](./../Images/code%20splitting,%20Lazy%20loading/code%20splitting,%20Lazy%20loading-6.png)

<br>

A페이지와 B페이지로 접근시 network 패널에 변화는없다.

모든 페이지를 한번에 번들링한후 최초렌더링시 한번에 받아오기 때문이다.

<br>

### Suspense

React.lazy를 사용하면 Suspense 컴포넌트를 사용하라고 에러메세지가 나온다.

<br>

다른 페이지로 넘어갈때 해당 페이지를 로드한다.

이때 해당 페이지를 로드하는 시간동안 화면에 렌더링 되는 내용은 없다.

Suspense 컴포넌트를 통해 **다른페이지를 로드하는 시간동안, 대체해주는 역활을한다.**

<br>

```jsx
<Suspense fallback={<div>Lodaing...</div>}>
```

<br>

```jsx
<Suspense fallback={<div>Lodaing...</div>}>
  <Switch>
    <Route path="/" component={LazyCountOne} exact />
    <Route path="/two" component={LazyCountTwo} exact />
  </Switch>
</Suspense>
```

<br>

lazy loading을 사용하지 않으면 모든 컨텐츠를 최초에 모두 로드한다.

![code splitting, Lazy loading](./../Images/code%20splitting,%20Lazy%20loading/code%20splitting,%20Lazy%20loading-1.png)

<br>

처음과 다르게 두가지로 나누어져있다.

이렇게 최초에 렌더할때 모든 페이지를 로드하는것이 아니라 번들파일을 나누어서 로드한다.

![code splitting, Lazy loading](./../Images/code%20splitting,%20Lazy%20loading/code%20splitting,%20Lazy%20loading-2.png)

<br>

## 정리

- 앱의 사이즈가 커지면 번들링을 하면서 최초 로드시간이 길어진다.
- code splitting을 통해 해당 페이지를 들어갈때 로드한다.

<br>

참고

- 인프런 프론트엔드 최적화 강의
- [https://ko.reactjs.org/docs/code-splitting.html](https://ko.reactjs.org/docs/code-splitting.html)
- [https://medium.com/hackernoon/lazy-loading-and-preloading-components-in-react-16-6-804de091c82d](https://medium.com/hackernoon/lazy-loading-and-preloading-components-in-react-16-6-804de091c82d)
- [https://pks2974.medium.com/dynamic-import-로웹페이지-성능-올리기-caf62cc8c375](https://pks2974.medium.com/dynamic-import-%EB%A1%9C%EC%9B%B9%ED%8E%98%EC%9D%B4%EC%A7%80-%EC%84%B1%EB%8A%A5-%EC%98%AC%EB%A6%AC%EA%B8%B0-caf62cc8c375)
- [https://babeljs.io/docs/en/babel-plugin-syntax-dynamic-import](https://babeljs.io/docs/en/babel-plugin-syntax-dynamic-import)

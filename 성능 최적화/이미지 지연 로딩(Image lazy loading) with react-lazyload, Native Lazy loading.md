# 이미지 지연 로딩(Image lazy loading) with react-lazyload, Native Lazy loading

현재 뷰포트에 보여지지 않는 **이미지를 나중에 로드해서, 웹 페이지 최초 로딩 속도를 향상 시키는 방법이다.**

<br>

그 중에서 쉽게 lazy loading을 하는 라이브러리를 알아보자.

<br>

## react-lazyload

[react-lazyload](https://www.npmjs.com/package/react-lazyload)는 컴포넌트, 이미지, 어떤것이든 lazyload 가능하게 해주는 라이브러리다.

뷰포트에 보여질때, 이미지가 로드되어 사용된다.

<br>

설치

```bash
$ npm install --save react-lazyload
```

<br>

사용

```jsx
import LazyLoad from "react-lazyload";

const App = () => {
  <LazyLoad>
    <img src="tiger.jpg" />
  </LazyLoad>;
};
```

<br>

LazyLoad라는 컴포넌트의 자식 컴포넌트에 img Element를 넣어주면된다.

이후 뷰포트에 보이면 해당 img가 로드되어 보여진다.

<br>

추가적으로 LazyLoad에는 여러가지 추가할 수 있는 **[속성들이](https://www.npmjs.com/package/react-lazyload#Props)** 있다.

<br>

예를들어)

```jsx
<LazyLoad offset={100}>
  <img src="tiger.jpg" />
</LazyLoad>
```

<br>

`offset={100}` 을 넣는다면, **뷰포트에서 100px 밑에 위치 했을때, 로드하게 된다.**

**뷰포트에 보여지기 이전에 로드하기 때문에, 좀더 자연스럽게 이미지가 보이게 되고 사용자 경험이 굉장히 올라간다.**

<br>

**Intersection Observer API과 다른점은 react-lazy는 스크롤 이벤트를 사용해서 로드하게 된다.**

<br>

## **Native Lazy Loading**

In Chrome 76 부터 Native Lazy Loading을 지원한다.(Chrome, Edge, Opera and Firefox)

![이미지 지연 로딩(Image lazy loading) with react-lazyload, Native Lazy loading](<./../Images/이미지%20지연%20로딩(Image%20lazy%20loading)%20with%20react-lazyload,%20Native%20Lazy%20loading/이미지%20지연%20로딩(Image%20lazy%20loading)%20with%20react-lazyload,%20Native%20Lazy%20loading-1.png>)

<br>

단순하게 img Element 속성에 loading 만 추가하면된다.

```jsx
<img src="image.png" loading="lazy" alt="…" width="200" height="200">
```

<br>

loading 속성에 사용되는 값은

- **auto : 기본값으로 loading 속성을 추가하지 않은것과 같다.**
- **lazy : 뷰포트에 닫을때 이미지를 로드한다.**
- **eager : 페이지에 위치하는것과 상관없이 이미지를 즉시 로드한다.**

<br>

**width, height 값을 지정해주는 것이 좋다.**

브라우저가 페이지에서 이미지를 로드하기전, 충분한 공간을 가지게해서 **Layout Shift가 발생하지 않게한다.**

<br>

## lazy load 주의점

<br>

### 1. 뷰포트 500px 전에 미리 로드하기

<br>

스크롤 내리면서 lazy load를 하게되면, **갑자기 로드하기 때문에 이미지 부분이 검게 보여진다.**

**따라서 뷰포트 500px 전에 미리 로드하게 설정하는게 사용자 경험을 향상 시켜준다.**

![이미지 지연 로딩(Image lazy loading) with react-lazyload, Native Lazy loading](<./../Images/이미지%20지연%20로딩(Image%20lazy%20loading)%20with%20react-lazyload,%20Native%20Lazy%20loading/이미지%20지연%20로딩(Image%20lazy%20loading)%20with%20react-lazyload,%20Native%20Lazy%20loading-2.gif>)

<br>

### 2. **가장 먼저 보여지는 뷰포트에는 lazy load을 하지 않는다.**

**가장 먼저 보여지는 뷰포트에는 lazy load을 하지 않는다.**

가능하면 뷰포트에 보여지지 않는 이미지에만 하는 것이 좋다.

lazy load를 하는 경우, **이미지가 해당 뷰포트에 위치했는지 확인하고 이미지를 로드하기 때문에 오히려 성능에 좋지 않다.**

<br>

### 3. 모바일, 데스크톱 화면 크기가 다르기 때문에 주의한다.

모바일, 데스크톱 화면 크기가 다르기 때문에 보여지는 뷰포트가 다르다.

**따라서 모든 이미지를 lazy load 걸지 않기 위해서, 어떤 이미지는 미리 로드 할지 디바이스 타입을 고려해야한다.**

<br>

### 4. 가장 먼저 보여지는 뷰포트의 살짝 밑에는 lazy load 하지 않기

뷰포트 500px 전에는 미리 이미지를 로드해야한다.

**최초 뷰포트에서 500px 까지의 이미지들은 lazy load 하지 않고, 즉시 로드하는 것이 사용자 경험을 향상 시켜준다.**

<br>

### 5. 이미지 개수가 적을때, 굳이 lazy load 하지 않기

**페이지 스크롤링을 몇번하지 않는데도 불구하고 lazy load를 하는 것은 좋지 않을 수 도있다.**

따라서 이미지 개수가 얼마 없다면 **즉시 로드하도록 하자.**

<br>

참고

- 프론트엔드 최적화 강의 인프런
- [https://www.npmjs.com/package/react-lazyload](https://www.npmjs.com/package/react-lazyload)
- [https://web.dev/browser-level-image-lazy-loading/](https://web.dev/browser-level-image-lazy-loading/)
- [https://imagekit.io/blog/lazy-loading-images-complete-guide/](https://imagekit.io/blog/lazy-loading-images-complete-guide/)
- [https://helloinyong.tistory.com/297](https://helloinyong.tistory.com/297)

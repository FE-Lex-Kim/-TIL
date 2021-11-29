# 스켈레톤 UI (Skeleton UI)

리소스의 사이즈가 커서 페이지를 **렌더링하는데 시간이 오래 걸린다면, 사용자는 페이지를 쉽게 떠난다.**

컨텐츠를 기다리다가 지치고 지루함을 느끼게된다.

**단순한 흰색 페이만 보여주기보다, 무언가를 보여주는게 훨씬 사용자 친화적인 UI이다.**

<br>

**스켈레톤 UI는 데이터를 가져오는 동안 대신 보여주는 UI 이다.**

![스켈레톤 UI](<../Images/스켈레톤%20UI%20(Skeleton%20UI)/스켈레톤%20UI%20(Skeleton%20UI)-1.gif>)

출처 : **[React Native Skeleton Content](https://www.npmjs.com/package/react-native-skeleton-content)**

<br>

전통적으로 로딩중에 무언가를 보여주는것은 **Spinner, Bubbles.. 등등 많이 사용해왔다.**

![스켈레톤 UI](<../Images/스켈레톤%20UI%20(Skeleton%20UI)/스켈레톤%20UI%20(Skeleton%20UI)-2.png>)

![스켈레톤 UI](<../Images/스켈레톤%20UI%20(Skeleton%20UI)/스켈레톤%20UI%20(Skeleton%20UI)-3.png>)

<br>

**스켈레톤 UI는 Spinner, Bubbles.. 보다 더 시각적으로 빠르다고 느껴진다고 한다.**

본격적으로 스켈레톤 UI을 알아보자.

<br>

## Skeleton UI in the Wild

다양한 큰 서비스에서들도 많이 사용되고 있다.

**리소스의 사이즈가 클 수록(로드하는 시간이 길수록) 스켈레톤 UI는 사용자 경험에 효과적이다.**

<br>

**google drive**

![스켈레톤 UI](<../Images/스켈레톤%20UI%20(Skeleton%20UI)/스켈레톤%20UI%20(Skeleton%20UI)-4.png>)

<br>

**youtube**

![스켈레톤 UI](<../Images/스켈레톤%20UI%20(Skeleton%20UI)/스켈레톤%20UI%20(Skeleton%20UI)-5.png>)

<br>

**facebook**

![스켈레톤 UI](<../Images/스켈레톤%20UI%20(Skeleton%20UI)/스켈레톤%20UI%20(Skeleton%20UI)-6.png>)

<br>

## 스켈레톤 UI 만들기

(리소스를 모두 로드한뒤 브라우저에 보여질 UI을 줄여서 브라우저 UI라고 말하겠다.)

아래의 설명의 [원문](https://dev.to/devggaurav/build-a-simple-card-skeleton-loader-component-using-html-and-css-3a20)

<br>

### 1. 브라우저 UI와 스켈레톤 UI을 똑같은 구조로 만들어준다.

<br>

Card.js

```jsx
<div class="card">
  <div>
    <img class="card-img" src="https://.." />
  </div>
  <div class="card-text">
    <h2 class="card-title">Gaurav Gawade</h2>
    <p class="card-description">
      <span>Lorem ipsum dolor sit amet consectetur, adipisicing eli.</span>
    </p>
    <ul class="card-skills">
      <li class="skill">UI Designer.</li>
      <li class="skill">UX Designer.</li>
      <li class="skill">Web Developer.</li>
    </ul>
    <a href="#" class="card-link">
      Read More
    </a>
  </div>
</div>
```

<br>

Skeleton.js

```jsx
<div class="skeleton">
  <div class="skeleton-img"></div>
  <div class="skeleton-text">
    <h2 class="skeleton-title"></h2>
    <p class="skeleton-description">
      <span></span>
      <span></span>
    </p>
    <ul class="skeleton-skills">
      <li class="skill"></li>
      <li class="skill"></li>
      <li class="skill"></li>
    </ul>
    <a href="#" class="skeleton-link"></a>
  </div>
</div>
```

<br>

### 2. 브라우저 UI와 스켈레톤 UI의 기본 스타일을 똑같이 만들어준다.

브라우저에 똑같곳에 위치하게 하기위해, **레이아웃의 위치, 크기.. 같은 큰 레이아웃만 똑같이 만들어준다.**

<br>

```css
.card,
.skeleton {
  display: flex;
  justify-content: space-around;
  padding: 20px;
  max-width: 400px;
  height: 155px;
  margin: 20px;
  border-radius: 10px;
  background-color: #e2e8f0;
  box-shadow: 0 9px 33px rgba(0, 0, 0, 0.07);
}

.card-text,
.skeleton-text {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  width: 250px;
  margin-left: 10px;
}

.card-img,
.skeleton-img {
  width: 75px;
  height: 75px;
  border-radius: 50%;
}

.card-description,
.skeleton-description {
  margin: 10px 0;
}

.card-link,
.skeleton-link {
  margin-top: 20px;
}

.card-skills,
.skeleton-skills {
  display: flex;
  justify-content: space-between;
}
```

<br>

### 3. 브라우저 UI에 구체적인 style을 넣어준다.

브라우저 UI에 Font size, color.. 등등 **스켈레톤 UI에 없는 컨텐츠를 위한 Style을 넣어준다.**

<br>

```css
skeleton-img {
  animation: pulse 2s infinite ease-in-out;
}

.skeleton-description span:nth-child(1) {
  display: block;
  width: 210px;
  height: 10px;
  background-color: #94a3b8;
}

.skeleton-description span:nth-child(2) {
  display: block;
  width: 150px;
  height: 10px;
  background-color: #94a3b8;
  margin-top: 5px;
}

.skeleton-skills .skill {
  width: 65px;
  height: 12px;
  background-color: #94a3b8;
}

.skeleton-link {
  width: 75px;
  height: 12px;
  background-color: #94a3b8;
}
```

<br>

### 4. Skeleton UI에 구체적인 Style을 넣어준다.

<br>

Skeleton UI에 height, width, background color을 구체적으로 넣어준다.

브라우저 UI와 다르게 **스켈레톤의 형태를 잡아주는 역활을 하는 Sytle을 넣어준 것이다.**

<br>

```css
.skeleton-img {
  animation: pulse 2s infinite ease-in-out;
}

.skeleton-description span:nth-child(1) {
  display: block;
  width: 210px;
  height: 10px;
  background-color: #94a3b8;
}

.skeleton-description span:nth-child(2) {
  display: block;
  width: 150px;
  height: 10px;
  background-color: #94a3b8;
  margin-top: 5px;
}

.skeleton-skills .skill {
  width: 65px;
  height: 12px;
  background-color: #94a3b8;
}

.skeleton-link {
  width: 75px;
  height: 12px;
  background-color: #94a3b8;
}
```

<br>

현재 브라우저 UI와 스켈레톤 UI 형태

<br>

![스켈레톤 UI](<../Images/스켈레톤%20UI%20(Skeleton%20UI)/스켈레톤%20UI%20(Skeleton%20UI)-7.png>)

<br>

### 5. Skeleton UI에 Animation을 넣어준다.

<br>

Animation을 넣어주어 **리소스가 진행되는것을 동적으로 보여준다.**

Animation 으로는 **background의 색을 점점 진하게 해주어서 로딩중임을 보여준다.**

<br>

```css
@keyframes pulse {
  0% {
    background-color: #94a3b8;
  }

  50% {
    background-color: #cbd5e1;
  }

  100% {
    background-color: #94a3b8;
  }
}
```

<br>

```css
.skeleton-title {
  width: 150px;
  height: 12px;
  animation: pulse 2s infinite ease-in-out;
}

.skeleton-img {
  animation: pulse 2s infinite ease-in-out;
}

.skeleton-description span:nth-child(1) {
  display: block;
  width: 210px;
  height: 10px;
  animation: pulse 2s infinite ease-in-out;
}

.skeleton-description span:nth-child(2) {
  display: block;
  width: 150px;
  height: 10px;
  animation: pulse 2s infinite ease-in-out;
  margin-top: 5px;
}

.skeleton-skills .skill {
  width: 65px;
  height: 12px;
  animation: pulse 2s infinite ease-in-out;
}

.skeleton-link {
  width: 75px;
  height: 12px;
  animation: pulse 2s infinite ease-in-out;
}
```

<br>

## react-content-loader

<br>

위의 방법은 직접 만들다보니 Skeleton을 만드는데 작업시간이 길다.

다행이도 손쉽게 사용할 수 있는 Skeleton library 3가지 있다.

- **react-content-loader**
- react-placeholder
- React Loading Skeleton

<br>

이 중에서 가장 많이 사용되고 관리가 잘되고 있는 **react-content-loader을 사용해 보자.**

<br>

설치

```bash
npm i react-content-loader
```

<br>

사용법

```jsx
import ContentLoader, { Rect, Circle } from "react-content-loader/native";

const MyLoader = () => (
  <ContentLoader viewBox="0 0 380 70">
    <Circle cx="30" cy="30" r="30" />
    <Rect x="80" y="17" rx="4" ry="4" width="300" height="13" />
    <Rect x="80" y="40" rx="3" ry="3" width="250" height="10" />
  </ContentLoader>
);
```

<br>

`Rect`, `Circle` 을 통해 사각형, 원 모양의 Skeleton을 만들어준다.

x, y, rx, ry, width, height... 등등 어트리뷰트로 위치와 높이를 Custom 할 수 있다.

<br>

![스켈레톤 UI](<../Images/스켈레톤%20UI%20(Skeleton%20UI)/스켈레톤%20UI%20(Skeleton%20UI)-8.png>)

<br>

react-content-loader 옵션들을 시각적으로 **쉽게 적용할 수 있게하는 [웹 서비스](https://skeletonreact.com/)를 사용할 수있다.**

더 자세한 옵션은 [Github](https://github.com/danilowoz/react-content-loader)을 보면 알 수 있다.

<br>

### 주의할 점

**실제 UI와 너무 다르면 좋지 않다.**

스켈레톤 UI를 **로딩중에 보여지는 UI라고 판단하지 않을 수 도 있다.**

**다른 정보를 보여주는 UI 라고 헷갈릴 수 있기 때문이다.**

<br>

참고

- [https://ui.toast.com/weekly-pick/ko_20201110](https://ui.toast.com/weekly-pick/ko_20201110)
- [https://uxdesign.cc/what-you-should-know-about-skeleton-screens-a820c45a571a](https://uxdesign.cc/what-you-should-know-about-skeleton-screens-a820c45a571a)
- [https://dev.to/devggaurav/build-a-simple-card-skeleton-loader-component-using-html-and-css-3a20](https://dev.to/devggaurav/build-a-simple-card-skeleton-loader-component-using-html-and-css-3a20)
- [https://github.com/danilowoz/react-content-loader](https://github.com/danilowoz/react-content-loader)
- [https://dev.to/bnevilleoneill/improve-ux-in-react-apps-by-showing-skeleton-ui-5a3i](https://dev.to/bnevilleoneill/improve-ux-in-react-apps-by-showing-skeleton-ui-5a3i)
- [https://dev.to/sanderdebr/speed-up-your-ux-with-skeleton-loading-18ja](https://dev.to/sanderdebr/speed-up-your-ux-with-skeleton-loading-18ja)

# 이미지 동적 로딩 with intersection observer

<br>

최초 렌더링시 사용자의 화면에 먼저 보여지는 부분의 리소스들이 가장 먼저 보여져야한다.

하지만 이미지 또는 동영상과 같은 리소스의 사이즈가 크다면, **다른 부분의 이미지, 동영상이 먼저 로드되고 나중에 로드되어 보여진다.**

![이미지 동적 로딩 with intersection observer](./../Images/이미지%20with%20intersection%20observer/이미지%20with%20intersection%20observer-1.png)

<br>

위의 사진처럼 먼저 보여져야할 동영상이 **pending 상태가되어 나중에 보여진다.**

<br>

이런 문제를 해결하는 방법은

**최초 렌더링시 사용자 화면에 보여지는 부분부터 로드하고, 스크롤해서 보여지는 부분의 이미지들은 스크롤해서 이미지가 보여질때 로드하면된다.**

<br>

## 스크롤 이벤트 핸들러 등록

스크롤 이벤트할때 함수를 호출하면 어떨까?

<br>

하지만 스크롤 할때마다 이벤트가 계속 발생하여 **의미없이 함수가 계속 호출될것이다.**

Javascript 메인 스레드가 지속적으로 태스크를 처리하고 **성능에 오히려 악영향을 줄 수 있다.**

<br>

그래서 이미지 element가 viewport에 보여질때 로드하는 방법을 사용하면 된다.

이런 방법을 **intersection observer** 이라고 한다.

<br>

## observer 객체 생성

특정 element를 옵저브한뒤, **viewport에서 보여질때만 함수를 호출**하는 방법을 말한다.

스크롤 할때마다 이벤트를 발생시키는 것보다 훨씬 성능이 좋아질 것이다.

<br>

```jsx
let options = {
  root: document.querySelector("#scrollArea"),
  rootMargin: "0px",
  threshold: 1.0,
};

let observer = new IntersectionObserver(callback, options);
```

<br>

intersection observer의 특징은 지속적으로 element를 감시해서 viewport에 **들어오거나 나갈때 callback 함수를 호출한다.**

<br>

## Callback 함수

callback 함수는 인자로

- **entries 객체**
- 자신을 호출한 **observer**를 받는다.

<br>

콜백함수가 호출되는 경우는 2가지이다.

1. 해당 엘리먼트가 **viewport에 보여지거나 나갈때 callback 함수 호출**
2. observer가 최초에 타겟을 관측할때, 즉, **observer 객체를 최초 생성할때 callback 함수를 호출한다**.(**threshold 비율만큼 보여질때 callback 함수가 호출된다.**)

<br>

```jsx
let callback = (entries, observer) => {
  entries.forEach((entry) => {
    // Each entry describes an intersection change for one observed
    // target element:
    //   entry.boundingClientRect
    //   entry.intersectionRatio
    //   entry.intersectionRect
    //   entry.isIntersecting
    //   entry.rootBounds
    //   entry.target
    //   entry.time
  });
};
```

<br>

### entries

**Target Element으로 설정된 Element 들이 entries 객체에 들어가 있다.**

entires는 여러개의 Target Element가 들어있을 수도있다.

<br>

예를들어

```jsx
<div>Div 1</div>
<div>Div 2</div>
<div>Div 3</div>
```

```jsx
var targets = document.getElementsByTagName("div");
var observer = new IntersectionObserver(callback, options);

// observe each div element
for (var target of targets) {
  observer.observe(target);
}
```

- entries에는 총 3개의 Target Element가 들어가게된다.

<br>

```jsx
let callback = (entries, observer) => {
  entries.forEach((entry) => {
    // Each entry describes an intersection change for one observed
    // target element:
    //   entry.boundingClientRect
    //   entry.intersectionRatio
    //   entry.intersectionRect
    //   entry.isIntersecting
    //   entry.rootBounds
    //   entry.target
    //   entry.time
  });
};
```

<br>

![이미지 동적 로딩 with intersection observer](./../Images/이미지%20with%20intersection%20observer/이미지%20with%20intersection%20observer-2.png)

<br>

entry는 여러 프로퍼티를 가지고 있다.

- boundingClientRect : [Element.getBoundingClientRect()](https://developer.mozilla.org/en-US/docs/Web/API/Element/getBoundingClientRect) 으로 계산된 target Element 의 사각형 범위를 알려준다.
- intersectionRatio : Target Element의 **노출된 비율을 알려준다.**
- intersectionRect : Target Element의 **사각형 범위를 반환한다.**
- **isIntersecting : Target Element가 노출되었는지 안되었는지 Boolean 값으로 알려준다.**
- rootBounds : **root로 지정된 Element의 범위**를 알려준다.
- target : **Target Element을 알려준다.**
- time : 노출됬을때는 viewport에서 **보여졌을때 또는 안보였을때의 얼마나 걸렸는지 시간을 알려준다.**

<br>

이제 intersection observer을 알게 되었으니, 드디어 viewport에 보여졌을때 이미지를 로드하는 방법을 배워보자

<br>

## IntersectionObserver options

```jsx
let options = {
  root: document.querySelector("#scrollArea"),
  rootMargin: "0px",
  threshold: 1.0,
};

let observer = new IntersectionObserver(callback, options);
```

options 객체는 **observer 콜백이 호출되는 상황을 조작할 수 있다.**

<br>

root : root 로 정의된 Element 기준으로 Target Element 의 노출, 비노출 여부를 결정한다.

- **root로 지정된 Element의 children에 Target Element가 있지 않는다면(root element의 자식요소가 아니라면) 화면에 노출된다고 해도, 노출로 보지 않아 callback 함수가 호출되지 않는다.**
- root 값이 null 이거나 지정되지 않을때 **기본값은 브라우저 viewport가 된다.**

<br>

rootMargin : root Element의 margin 영역을 정의한다.

- 10px 10px 10px 10px 과 같이한다면 **그 만큼 더 영역이 늘어난다.**
- 기본값은 0px 0px 0px 0px 이다.

<br>

threshold : **Target Element가 노출되는 비율을 정해 비율에 따라 callback 함수를 호출 시켜준다.**

- 만약 0.5 이라면 50% 만큼 보여졌을때 호출된다.
- 만약 단일 Number을 넣는것이 아니라 Array를 넣는다면, 해당 비율마다 callback 함수가 호출된다 ex) `[0, 0.25, 0.5, 0.75, 1]`

<br>

## Target Element

observer를 생성한뒤, target Element를 설정해야한다.

**Target Element는 viewport에 target Element가 보여질때, callback 함수를 호출된다.**

<br>

```jsx
let observer = new IntersectionObserver(callback, options);
let target = document.querySelector("#listItem");

observer.observe(target);
```

<br>

**observe 메서드의 target Element를 인수로 넣어주면된다.**

<br>

## Viewport에 보여졌을때, 이미지 로드

<br>

img 엘리먼트를 IntersectionObserver 객체의 Target Element로 지정해준다.

지정해주기 위해 useRef로 img 엘리먼트를 선택해준다.

<br>

```jsx
import React, { useEffect, useRef } from "react";

function Card(props) {
  let imgRef = useRef(null);

  useEffect(() => {
    function callback(entries, observer) {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.src = entry.target.dataset.src;
        }
      });
    }

    const option = {
      root: null,
      rootMargin: "0px",
      threshold: 1.0,
    };

    const observer = new IntersectionObserver(callback, option);
    observer.observe(imgRef.current);
  });

  return (
    <div className="Card text-center">
      <img data-src={props.imgae} ref={imgRef} />
      <div className="p-5 font-semibold text-gray-700 text-xl md:text-lg lg:text-xl keep-all">
        {props.children}
      </div>
    </div>
  );
}

export default Card;
```

<br>

이후 Didmount때 최초에 지정해주면 되니 useEffect 안에서 로직을 넣어준다.

<br>

img 엘리먼트의 **src에 주소를 넣게되면 바로 서버에 요청하게 되므로 넣어주지 않는다.**

대신에 dataset을 이용한다. **data-src에 넣어주어 viewport에 보여질때 target Element(img태그)의 src프로퍼티에 넣어주어 이미지 요청을한다.**

```jsx
entry.target.src = entry.target.dataset.src;
```

<br>

IntersectionObserver의 callback을 불러오는 경우는 앞서 말한대로 2가지이다.

1. 최초에 IntersectionObserver 객체 생성시
2. Target Element가 viewport에 보여졌을때 and 사라질때

<br>

**따라서 viewport에 보여질때만 callback 함수가 호출되게 조건을 걸어주어야한다.**

```jsx
function callback(entries, observer) {
  entries.forEach((entry) => {
    if (entry.isIntersecting) {
      entry.target.src = entry.target.dataset.src;
    }
  });
}
```

<br>

또! 문제가 생겼다.

스크롤링 하면서 Target Element가 보여질때마다 callback함수가 호출된다는 점이다.

<br>

callback 함수가 한번 호출됬을때, 이제 Target Element를 그만 감시하고 해제 시켜주자.

```jsx
function callback(entries, observer) {
  entries.forEach((entry) => {
    if (entry.isIntersecting) {
      entry.target.src = entry.target.dataset.src;
      observer.unobserve(entry.target);
    }
  });
}
```

<br>

이제 viewport에 들어올때, 요청하게되고 이미지가 보이게된다.

<br>

최초 렌더링시에 보여지는 viewport에서 리소스의 크기가 크다면, 이런 방법을 사용하는게 좋아보인다.

<br>

참고

- 인프런 프론트엔드 최적화 강의
- [https://developer.mozilla.org/ko/docs/Web/API/Intersection_Observer_API](https://developer.mozilla.org/ko/docs/Web/API/Intersection_Observer_API)
- [https://www.codeguage.com/courses/advanced-js/intersection-observer-entries](https://www.codeguage.com/courses/advanced-js/intersection-observer-entries)
- [https://pks2974.medium.com/intersection-observer-간단-정리하기-fc24789799a3](https://pks2974.medium.com/intersection-observer-%EA%B0%84%EB%8B%A8-%EC%A0%95%EB%A6%AC%ED%95%98%EA%B8%B0-fc24789799a3)
- [https://velog.io/@yejinh/Intersection-Observer로-Lazy-Image-구현하기](https://velog.io/@yejinh/Intersection-Observer%EB%A1%9C-Lazy-Image-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0)
- [http://blog.hyeyoonjung.com/2019/01/09/intersectionobserver-tutorial/](http://blog.hyeyoonjung.com/2019/01/09/intersectionobserver-tutorial/)

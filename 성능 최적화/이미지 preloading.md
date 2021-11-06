# 이미지 preloading

이미지 리소스는 사이즈가 커서 로드하는 시간이 오래걸리는 경우가 많다.

컴포넌트를 preloading 하는 방법은 동적 import를 통해 불러왔다.

<br>

이미지는 브라우저에 보여져야 미리 불러올 수 있다.

하지만 JS의 `new Image()` **이미지 객체를 생성하면 미리 불러 올 수 있는 방법이 있다.**

**이후 불러온뒤 캐시에 저장하는 방법을 사용한다.**

<br>

## 대표적 이미지 하나만 preloading

```jsx
const img = new Image();
img.src = "https://img/...";
```

<br>

위와 같이 이미지 객체를 생성한후,

**src 프로퍼티에 이미지 주소를 넣는 순간 이미지를 불러올 수 있다.**

하지만 불러온 이미지를 사용하는 페이지에 들어갔을때, 이전에 불러온 이미지를 **그대로 사용하지않고 다시 불러온다.**

<br>

이미지를 불러온뒤, **캐쉬에 저장해서 이미지를 사용하는 페이지에서 해당 캐쉬에 있는 것을 확인한다.** 이후 백엔드 서버에서 로드 하지않고 캐쉬에서 불러와 빠른 시간내에 이미지가 브라우저에 렌더링 된다.

<br>

※ 여러 이미지를 preloading 하게된다면, 다른 로직을 수행할때 성능에 악영향을 줄 수 있기때문에, **이미지 사이즈가 제일 큰 것만 preloading 하는 것도 좋은 방법이다.**

```jsx
function App() {
  useEffect(() => {
    const img = new Image();
    img.src =
      "https://stillmed.olympic.org/media/Photos/2016/08/20/part-1/20-08-2016-Football-Men-01.jpg?interpolation=lanczos-none&resize=*:800";
  });
}
```

<br>

![이미지 preloading](./images/../../Images/이미지%20preloading/이미지%20preloading-1.png)

※ 네트워크 패널을 사용하면 img를 먼저 불어온 것을 확인 할 수 있다. 또한 size에서 disk cache라고 적혀있는데 cache에서 불러온것을 확인 할 수 있다.

<br>

## 이미지 여러개 preloading

<br>

### 비동기적(병렬적) preloading

이번 코드는 하나의 대표적인 이미지만 preloading 하는 것이아니라, **여러 이미지를 preloading 방법이다.**

```jsx
function preloadingImg(imgArr) {
  imgArr.forEach((v) => {
    let img = new Image();
    img.src = v;
  });
}

const images = const images = [
  "https://img1.png",
  "https://img2.png",
  "https://img3.png",
  "https://img4.png",
  "https://img5.png",
];

function App() {
  useEffect(() => {
    preloadingImg(images);
  });
}
```

<br>

위와 같이 여러 이미지를 preloading 해서 캐쉬에 저장해 둔다.

![이미지 preloading](./images/../../Images/이미지%20preloading/이미지%20preloading-2.png)

- size 부분을 보면 disk cache에서 불러온것을 볼 수 있다.(아래의 이미지들은 미리 불러온것들)

<br>

**조심!**

이미지를 여러개 불러오다보니, **만약 사이즈가 어마어마하게 큰 이미지라면 성능에 오히려 악영향이 갈 수 있다.**

<br>

![이미지 preloading](./images/../../Images/이미지%20preloading/이미지%20preloading-3.png)

<br>

이미지는 **비동기적으로 불러오게되어서(병렬적)** **모든 이미지들이 불러와지는데 시간이 오래걸리게 된다.**

우선순위와 상관없이 처리하게되, **우선순위가 높은 이미지가 뒤늦게 로딩될 수 도 있다.**

이러한 경우 **직렬적 즉, 동기적으로 불어와야한다.**

<br>

### 동기적으로(직렬적) preloading

<br>

동기적으로 이미지를 다운로드하게 된다면, **이미지 우선순위에 따라 preload를 통해 브라우저에 먼저 렌더링 할 수 있게한다.**

<br>

**즉, 브라우저에 우선순적으로 보여주어야 할것을 미리 로딩한다.**

```jsx
function preload(imageArray, index) {
  index = index || 0;
  if (imageArray && imageArray.length > index) {
    var img = new Image();
    img.onload = function () {
      preload(imageArray, index + 1);
    };
    img.src = images[index];
  }
}

const images = const images = [
  "https://img1.png",
  "https://img2.png",
  "https://img3.png",
  "https://img4.png",
  "https://img5.png",
];

function App() {
  useEffect(() => {
		preload(images)
  });

	return ...
}
```

<br>

로직 동작과정

1. `preload(imgArr)` 호출된다.
2. 초기 index 파라미터 값이 없으니, 0값이 들어간다.
3. `if (imageArray && imageArray.length > index)`을 통과한다.
4. `onload`에 재귀적으로 호출하기위해, 현재 `index` 번호에 +1 해준다.
5. `img.src`의 값에 이미지 주소를 넣어 이미지를 호출한다.
6. 이미지가 완전히 불러온뒤 `img.onload(imageArray, index + 1)`이 실행된다.
7. ... 이 과정이 마지막 번호까지 실행된다.
8. 마지막 번호때, `if (imageArray && imageArray.length > index)` 조건문을 통과하지 못해 종료한다.

<br>

※ [img.onload 함수](https://kyounghwan01.github.io/blog/etc/html/img-onload/#onload) : 이미지가 완전히 로드된후, 작업을 수행하고 싶을때 사용. **즉, src의 값을 완전히 불러온 후, onload 함수가 실행된다.**

<br>

![이미지 preloading](./images/../../Images/이미지%20preloading/이미지%20preloading-4.png)

이렇게 병렬적으로 하나하나 처리하게된다.

<br>

### 정리

**사용자 경혐, 브라우저에 보여지는 이미지 위치, 용도 등등에 따라 동기적 또는 비동기적으로 preloading을 선택해야한다.**

<br>

참고

- 인프런 프론트엔드 최적화 강의
- [https://www.photo-mark.com/notes/image-preloading/](https://www.photo-mark.com/notes/image-preloading/)
- [https://mygumi.tistory.com/277](https://mygumi.tistory.com/277)
- [https://kyounghwan01.github.io/blog/etc/html/img-onload/#onload](https://kyounghwan01.github.io/blog/etc/html/img-onload/#onload)

# auto multiple slider(carousel) 이미지 여러개 자동 슬라이더

보통 이미지 여러개를 자동으로 슬라이딩 시키는 방식은 로고들을 나열시키고 쭉 자연스럽게 보이기위해 사용한다.

<br>

아래같은 이미지 처럼 오른쪽에서 좌측으로 천천히 이동 시킨다.

![auto multiple slider(carousel)](<../Images/auto%20multiple%20slider(carousel)/auto%20multiple%20slider(carousel)-1.png>)

<br>

요소 구조

Slider > slide-track > slide

```html
<div class="slider">
  <div class="slide-track">
    <div class="slide">
      <img
        src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/557257/1.png"
        height="100"
        width="250"
        alt=""
      />
    </div>
    <div class="slide">
      <img
        src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/557257/2.png"
        height="100"
        width="250"
        alt=""
      />
    </div>
    <div class="slide">
      <img
        src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/557257/3.png"
        height="100"
        width="250"
        alt=""
      />
    </div>
    <div class="slide">
      <img
        src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/557257/4.png"
        height="100"
        width="250"
        alt=""
      />
    </div>

    <div class="slide">
      <img
        src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/557257/1.png"
        height="100"
        width="250"
        alt=""
      />
    </div>
    <div class="slide">
      <img
        src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/557257/2.png"
        height="100"
        width="250"
        alt=""
      />
    </div>
    <div class="slide">
      <img
        src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/557257/3.png"
        height="100"
        width="250"
        alt=""
      />
    </div>
    <div class="slide">
      <img
        src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/557257/4.png"
        height="100"
        width="250"
        alt=""
      />
    </div>
  </div>
</div>
```

`Slider` > `slide-track` > `slide`

<br>

보여주고 싶은 이미지들을 이미지 리스트이라고 부를때,

- 첫번째 이미지 리스트
- 두번째 이미지 리스트를 만든다.

<br>

첫번째 이미지 리스트이 뷰포인트에서 끝나면, 두번째 이미지 리스트을 보여준다.

<br>

두번째 이미지 리스트가 딱 끝나는 지점에서, 딱 첫번째 이미지 리스트로 이동시킨다.

silder

```css
silder {
  overflow: hidden;
  width: 1000px;
  height: 100px;
  position: relative;

  &::before,
  &::after {
    background: linear-gradient(to right, rgba(255, 255, 255, 1) 0%, rgba(255, 255, 255, 0) 100%);
    content: "";
    height: 100px;
    position: absolute;
    width: 200px;
    z-index: 2;
  }

  &::after {
    right: 0;
    top: 0;
    transform: rotateZ(180deg);
  }

  &::before {
    left: 0;
    top: 0;
  }
}
```

- `width` : 브라우저에서 보여질 width 값을 정한다.
- `overflow : hidden` :
  slider width 값 밖에 있는 요소들은 숨겨준다.
- `position : relative` : silde-track이 absolute이기 때문에 기준점을 잡아준다.

<br>

```css
slider {
  ... ... &::before,
  &::after {
    background: linear-gradient(to right, rgba(255, 255, 255, 1) 0%, rgba(255, 255, 255, 0) 100%);
    content: "";
    height: 100px;
    position: absolute;
    width: 200px;
    z-index: 2;
  }
}
```

- `&::before`, `&::after` : slider의 자식요소중 가장 앞과 뒤에 가상 요소를 만들어준다.
  - 만들어주는 이유는 위 예제 이미지처럼 하얀색 그레디언트를 주기위해서이다.
- `height: 100px`, `width: 200px` : silder 높이, 넓이를 똑같이 준다.
- `z-index : 2` : 이미지 요소들보다 위에 존재해서 그래디언트가 들어가게한다.

<br>

```css
slider {
  ... ... &::after {
    right: 0;
    top: 0;
    transform: rotateZ(180deg);
  }

  &::before {
    left: 0;
    top: 0;
  }
}
```

- `&::before` 가상요소를 slider의 가장 앞에 위치시킨다.
- `&::after` 가상요소를 slider의 가장 앞에 뒤에 위치시킨다. 이후 그래디언트를 반대로 주기위해 `transform` 으로 180도 돌린다.

<br>

silde-track

```css
// Animation
@keyframes scroll {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(calc(-250px * 7));
  }
}

slide-track {
  animation: scroll 40s linear infinite;
  width: calc(250px * 14);
}
```

- 애니메이션을 걸어준다.
  - 좌측방향으로 천천히 이동시키위해 마이너스 부호로 넣어준다.
  - **이미지 width \* 이미지 개수** 만큼 좌측으로 이동시킨다
- width 값을 두 이미지 섹션의 값으로 계산해 넣어준다.
  - 이미지 하나당 250px이고 하나의 이미지 리스트들의 개수가 7개라면, 총 두개의 리스트이므로 250 \* 14

<br>

silde

```css
slide {
  height: 100px;
  width: 250px;
}
```

- slider 높이와 똑같이 정해준다.

<br>

참고

- [https://splidejs.com/extensions/auto-scroll/](https://splidejs.com/extensions/auto-scroll/)
- [https://multislider.trevorblackman.io/](https://multislider.trevorblackman.io/)
- [https://codepen.io/studiojvla/pen/qVbQqW](https://codepen.io/studiojvla/pen/qVbQqW)
- [https://css-tricks.com/infinite-all-css-scrolling-slideshow/](https://css-tricks.com/infinite-all-css-scrolling-slideshow/)

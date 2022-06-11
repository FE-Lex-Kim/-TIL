- [무한 슬라이드 중간에 멈추고 진행하는법(Only CSS | 다음,이전 버튼X)](#무한-슬라이드-중간에-멈추고-진행하는법only-css--다음이전-버튼x)
    - [Slider](#slider)
    - [ReviewList](#reviewlist)
    - [Review](#review)

<br>

# 무한 슬라이드 중간에 멈추고 진행하는법(Only CSS | 다음,이전 버튼X)

![무한 슬라이드 중간에 멈추고 진행하는법(Only CSS | 다음,이전 버튼X)](<../Images/무한%20슬라이드%20중간에%20멈추고%20진행하는법(Only%20CSS%20|%20다음,이전%20버튼X)/무한%20슬라이드%20중간에%20멈추고%20진행하는법(Only%20CSS%20|%20다음,이전%20버튼X)-1.gif>)

버튼 클릭형이 아닌 정보를 중간중간 제공하는 용도로 슬라이드를 만드는 경우도 많다.

이때 슬라이드가 계속해서 움직이는것이 아니라,

정보를 보여주어야 하는 지점에 정확하게 몇초 정도 대기한후 이동하는 슬라이더를 구현해보자.

<br>

HTML 구조

Slider > ReviewList > Review

```jsx
<Slider>
  <ReviewList>
    <FristReview />
    <SecondReview />
    <ThirdhReview />
    <FirstReview />
  </ReviewList>
</Slider>
```

Review가 3개만 보여지게 하려고한다.

**하지만 마지막 세 번째에서 첫 번째로 넘어갈때, 바로 즉각적으로 넘어가면 순간이동 하는것처럼 보이게 된다.**

**따라서 첫 번째 Review를 마지막 요소로 하나 더 넣어서 세 번째에서 첫 번째로 자연스럽게 이동하게한뒤,**

- **마지막 요소인 첫 번째 Reivew에서 바로 첫 번째 요소인 FirstReview로 이동시킨다.(이때는 애니메이션 효과가 없고 바로 순간이동 시킨다.)**

<br>

### Slider

```css
const Slider = styled.div`
  position: relative;

  width: 1200px;
  height: 280px;
  margin-top: 120px;

  overflow: hidden;

  background-color: #15161d;
  border-radius: 16px;
`;
```

Silder는 오로지 브라우저에 보여져야할 부분이다.

`overflow : hidden` → ReviewList에서 넘치는 Review들을 감추게 한다.

`width, height` → 정확한 높이와 넓이 값을 준다.

<br>

### ReviewList

```css

const ReviewList = styled.ul`
  all: unset;

  display: flex;
  position: absolute;
  top: 0;

  width: calc(1200px * 4);

  @keyframes scroll {
    0% {
      /* 0초 */
      transform: translateX(0px);
    }
    23.085% {
      /* 5초 */
      transform: translateX(0px);
    }
    32.607% {
      /* 7초 */
      transform: translateX(-1200px);
    }
    55.692% {
      /* 12초 */
      transform: translateX(-1200px);
    }
    65.214% {
      /* 14초 */
      transform: translateX(-2400px);
    }
    88.299% {
      /* 19초 */
      transform: translateX(-2400px);
    }
    100% {
      /* 21초 */
      transform: translateX(-3600px);
    }
  }

  animation: scroll 21s linear infinite;
`;
```

ReviewLists는 모든 Review들을 담는다.

`width: calc(1200px * 4);` → 하나의 Review 넓이 \* 총 개수를 width 값으로 준다.

<br>

애니메이션을 정할때 keyframe으로 각각 멈추고 진행해야할 시간을 정해 줄 수 있다.

```css
@keyframes scroll {
  0% {
    /* 0초 */
    transform: translateX(0px);
  }
  23.085% {
    /* 5초 */
    transform: translateX(0px);
  }
  32.607% {
    /* 7초 */
    transform: translateX(-1200px);
  }
  55.692% {
    /* 12초 */
    transform: translateX(-1200px);
  }
  65.214% {
    /* 14초 */
    transform: translateX(-2400px);
  }
  88.299% {
    /* 19초 */
    transform: translateX(-2400px);
  }
  100% {
    /* 21초 */
    transform: translateX(-3600px);
  }
}

animation: scroll 21s linear infinite;
```

21초를 100%로 1% 는 4.761% 이다.

`0% transform: translateX(0px);` → 0초때 ReviewList의 0px 지점을 보여주게한다.

`23.085% transform: translateX(0px);` → 23.085%는 5초이다. 5초 시점때 ReviewList의 0px로 보여지도록 한다. 따라서 0초 ~ 5초 사이에는 0px를 보여지게하므로 멈추어 있게 한다.

<br>

`32.607% transform: translateX(-1200px);` → 32.607%는 7초이다. 7초 시점때, -1200px이 보이게 한다는 의미이다. 즉, 7초에 -1200px 부분이 보여져야하므로, 5초 ~7초 사이에 -1200px로 이동해야한다.

<br>

`55.692% transform: translateX(-1200px);` → 55.692%는 12초이다. 12초 시점떄, -1200px이 보이게한다는 의미이다. 즉, 12초때 -1200px 부분이 보여져야하므로, 7초 ~ 12초 사이에는 멈추어서 움직이지 않는다.

<br>

`65.214% transform: translateX(-2400px);` → 65.214%는 14초이다. 14초 시점때, -2400px이 보이게 한다는 의미이다. 즉, 14초에 -2400px 부분이 보여져야하므로, 12초 ~14초 사이에 -12400px로 이동해야한다.

<br>

**이런식으로 각각 각 퍼센테이지가 의미하는 것은 보여지고 싶은 CSS를 정해주고**

**그 퍼센테이지 사이에 시간에서 애니메이션으로 움직인다.**

<br>

### Review

```css
const Review = styled.div`
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;

  width: 62.5vw;
  height: 14.5833vw;
`;
```

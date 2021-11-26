# Layout Shift

브라우저가 렌더링하는 동안 **예기치 않게 레이아웃이 화면상에서 이동하는 것을 말한다.**

잘못된 클릭을 유도하여 실제 피해를 일으킬 수도 있고 아주 실망스러운 사용자 경험으로 이어진다.

<br>

예를들어)

- 링크나 버튼을 클릭하려고 할때, 갑자기 움직여서 다른 것을 클릭한다.
- 페이지에서 갑자기 바뀌는 부분이 생겨, 텍스트가 움직여서 읽던 부분을 놓친다.

<br>

## CLS(**Cumulative Layout Shift**)

![Layout Shift](../images/Layout%20Shift/Layout%20Shift-1.png)

Cumulative Layout Shift는 페이지가 전체 완전히 로드되어 보여지기까지 기간동안 **Layout shift가 발생하는 것을 점수로 표시한것이다.**

0점은 아예 Layout shift가 발생하지 않았을때, 1점은 모든 레이아웃이 변경되었을때를 말한다.

**높은 사용자 경험을 제공하려면 CLS가 0.1점 이하 여야한다.**

<br>

![Layout Shift](../images/Layout%20Shift/Layout%20Shift-2.png)

이미지 출처 : [https://web.dev/i18n/ko/cls/](https://web.dev/i18n/ko/cls/)

<br>

![Layout Shift](../images/Layout%20Shift/Layout%20Shift-3.png)

<br>

Performance 패널에서 Exerience을 보게되면 Layout Shift라고 보인다.

**어느 시점때 Layout Shift가 발생한지, 얼마나 발생한지 시각적으로 확인 할 수 있다.**

<br>

이렇게 발생한 Layout Shift는 성능에 큰 악영향을 미친다.

**레이아웃이 변경된 만큼 다시 계산하여 repaint, reflow가 발생하기 때문이다.**

<br>

## Layout Shift 발생원인

1. 사이즈가 정해지지 않아서

   → 해당 사이즈 영역을 정확하게 표시했으면 발생하지 않는다. **그 부분에 대충 위치해 있을것으로 정해 놓기 때문에 발생한다.**

2. 페이지에 포함된 광고의 사이즈가 정해지지 않아서

   → **이미지, 동적로딩 때문에 밀리게된다.**

3. 동적 삽입 콘텐츠가 있어서

   → **동적으로 컨텐츠가 삽입되어서 밀리게된다.**

4. Web font의 뒤늦은 로드, 스타일 차이

   → Web font을 로드하는 과정에서 뒤늦게 다운로드 됬을때, **Web font가 다운로드 받기전 보여지는 기본 폰트와 크게 스타일이 달라 밀리게 된다.**

<br>

## Layout Shift 해결

**Layout Shift가 발생하는 Layout의 정확한 사이즈를 지정 해주면된다.**

마찬가지로 Web font도 기본 폰트와 크기를 비교한뒤 정확한 크기를 지정해주면된다.(**[Font style matcher](https://sangziii.github.io/fontStyleMatcher/) 사용하면 편하다.)**

<br>

이미지 Layout Shift를 발생하는 경우가 대부분이다.

이미지 Layout Shift를 해결하는 방법에 대해 알아보자

<br>

### **`aspect-ratio` 속성 사용**

**x/y 비율을 지정하면 해당 비율로 요소를 나타낸다**.

**이미지가 로드되기 전에 `width` 및 `height` 속성을 기준으로 aspect-ratio 를 계산한다.**

이 정보는 레이아웃 계산을 시작할 때 제공된다.

```html
<div class="box1">
  <img src="" />
</div>
```

```css
img {
  width: 100%;
  aspect-ratio: 16/9;
}
```

<br>

이미지에 `aspect-ratio` 을 주면된다.

이미지가 없어서 content의 영역이 없다. content의 aspect-ratio을 통해 값을 미리 지정해 두어 Layout Shift가 발생하지 않게 한다.

<br>

현재 지원하지 않는 브라우저가 있다.(2021/11/17)

![Layout Shift](../images/Layout%20Shift/Layout%20Shift-4.png)

<br>

### padding 비율

이미지가 로드되기 전에 content 영역이 없기 때문에 layout shfit가 발생한다.

**이미지가 로드되기 전에도 영역 지정해 주면된다.**

방법은 여러가지 있지만 **padding을 비율로 주는 방법으로 해결해보자.**

<br>

```jsx
<ImageWrap>
  <Image src={urls.small} alt={alt} onClick={openModal} />
</ImageWrap>
```

```jsx
const ImageWrap = styled.div`
  width: 100%;
  padding-bottom: 56.25%;
  position: relative;
`;

const Image = styled.img`
  cursor: pointer;
  width: 100%;
  position: absolute;
  top: 0;
  left: 0;
`;
```

<br>

ImageWrap의 CSS 부터 확인해보자

- `width: 100%` → ImageWrap에 width값 100%를 주어서, **고정된 width 값을 가지게한다.**
- `padding-bottom: 56.25%` → padding-bottom 값을 준 이유는 **contenet-box의 고정된 영역를 padding으로 채우기 위해서이다.**
  - 56.25%는 16:9 비율로 이미지를 맞추려고 하기 위해서 이다.
  - **padding-bottom의 퍼센테이지는 width를 기준으로 한다. (상위 요소기준이 아니다.)**
  - padding 값을 주었기 때문에, **이미지가 로드가 되지 않았어도 영역을 가지게 되어서 Layout Shift 가 발생하지 않는다.**

<br>

Image의 CSS를 확인해보자

- `width : 100%` → 이미지 크기를 ImageWrap에 맞추어준다.
- `position: absolute`, `top: 0`, `left: 0` → 최상단에 딱 붙여준다.

<br>

정리

- ImageWrap에서 비율을 정하고 Img는 Wrap에다 고정해 둔다.
- **Img가 느리게 로드되어도 레이아웃은 이미 자리잡아 있기때문에 Layout Shift는 발생하지 않는다.**

<br>

참고

- 인프런 프론트엔드 최적화 강의
- [https://web.dev/i18n/ko/cls/](https://web.dev/i18n/ko/cls/)
- [https://wit.nts-corp.com/2020/12/28/6240](https://wit.nts-corp.com/2020/12/28/6240)
- [https://www.onely.com/blog/cumulative-layout-shift/](https://www.onely.com/blog/cumulative-layout-shift/)
- [https://hyeonseok.com/blog/709](https://hyeonseok.com/blog/709)

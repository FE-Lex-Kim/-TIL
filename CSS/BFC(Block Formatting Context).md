# BFC(Block Formatting Context)

<br>

BFC는 float요소의 레이아웃 위치를 어디에 위치시킬 것인지 구분해준다.

<br>

## BFC가 생기는 조건

- html root 태그
- none을 제외한 float
- position : fixed, absolute
- display : inline-block, table, table-cell, table-caption
- overflow : visible을 제외한 모든 값
- display: floow-root
- display: flex, inline-flex, grid, inline-grid

등등..

<br>

BFC는 요소의 레이아웃에 영향을 준다.

BFC는 레이아웃을 변경시키는 것보다 레이아웃 위치와 플로팅 요소를 위해 생성한다.

<br>

BFC를 생성하는 요소는 일반적으로

- 내부의 float과
- 외부의 float
- 마진 충돌

을 해결하기위해 사용한다.

<br>

## BFC의 특성

<br>

### float는 BFC를 생성한다.

따라서 플로팅된 요소의 자식요소 또한 플로팅 요소라면

플로팅된 부모요소는 BFC이므로 자식요소인 플로팅 요소의 높이를 파악하고 부모의 플로팅 요소는 높이를 가지게 된다.

<br>

### BFC의 자식의 자식은 어떨까?

BFC요소는 자식의 자식 요소의 높이값을 인지하고 해당 자식의 자식의 플로팅 요소의 높이값을 가지게된다.

<br>

하지만 BFC의 자식요소는 BFC가 아니므로 자식요소의 자식요소의 높이값을 인식하지 못한다.

<br>

### BFC는 margin collaps를 막는다.

margin collaps는 요소의 margin 값이 겹칠경우, 값이 더큰 margin 만 적용되는 현상이다.

<br>

BFC가 아닌 자식요소와 부모요소가 있을때 내부의 자식요소가 margin 값을 가지면 부모요소는 margin collaps가 발생한다.

<br>

하지만 BFC인 부모요소와 margin값을 가진 자식요소는 margin collaps를 방지하므로 BFC인 부모요소는 내부의 높이가 더 커지게된다.

<br>

참고

- [https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Block_formatting_context](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Block_formatting_context)
- [https://blueshw.github.io/2020/05/17/know-css-block-formatting-context/](https://blueshw.github.io/2020/05/17/know-css-block-formatting-context/)
- [https://velog.io/@pandati0710/css-block-formatting-context](https://velog.io/@pandati0710/css-block-formatting-context)

# position 요소를 배치하는 방법(relative, fixed, absolute, static)

<br>

## position

position 속성은 문서 상에 요소를 배치하는 방법을 지정한다.

top, right, bottom, left 속성이 요소를 배치할 최종 위치를 결정한다.

그 속성의 값으로는 5가지가 있다.

<br>

### static

기본 값이다.

요소를 일반적인 문서 흐름에 배치한다. top, right, bottom, left, z-indxe 속성이 아무런 영향도 주지 않는다.

<br>

### relative

요소를 일반적인 문서 흐름에 따라 배치하고, 자기 자신을 기준으로 top, right, bottom, left의 값에 오프셋을 적용한다.

오프셋은 다른 요소에 영향을 주지 않는다.

따라서 페이지 레이아웃에서 요소가 차지 하는 공간은 static 일때와 같다.

<br>

\*\*오프셋 : 최종 위치를 만들기 위해 기준이 되는 주소에 더해진 값을 의미한다.

<br>

### absolute

요소를 일반적인 문서 흐름에서 제거하고, 페이지 레이아웃에 해당 요소가 공간을 가지지 않는다.

<br>

대신 가장 가까운 위치 지정 조상 요소(static이 아닌 position 속성을 가지고 있는 fixed, absolute, relative, sticky)에 대해 상대적으로 배치한다.

단, 조상 중 위치 지정 요소가 없으면 초기 컨테이닝 블록을 기준으로삼는다.

<br>

top, right, bottom, left 값이 최종 위치를 지정한다.

<br>

초기 컨테이닝 블록 : 루트요소(<html>)의 컨테이닝 블록이다. 초기 컨테이닝 블록은 뷰포트 또는 페이지 영역의 크기와 같다.

<br>

### fixed

요소를 일반적인 문서흐름에서 제거하고, 페이지 레이아웃에서 공간이 없다.

대신 뷰포트의 초기 컨테이닝 블록을 기준으로 삼아 배치한다.

<br>

단 요소의 조상 중 하나가 `transform`, `perspective`, `filter` 속성중 하나라도 `none` 이 아니라면 뷰포트 대신 그 조상을 컨테이닝 블록으로 삼는다.

<br>

top, right, left, bottom이 최종위치를 결정한다.

<br>

### sticky

요소를 일반적인 문서흐름에 따라 배치하고, 가장 가까운 스크롤 동작(overflow, hidden, scroll, aotu, overlay)이 존재하는 가장 가까운 조상에 달라 붙는다.

<br>

해당 가까운 스크롤 동장 조상을 기준으로 top, right, bottom, left의 값에 따라 오프셋을 적용한다.

<br>

참고

- [https://developer.mozilla.org/ko/docs/Web/CSS/position](https://developer.mozilla.org/ko/docs/Web/CSS/position)
- [https://developer.mozilla.org/ko/docs/Web/CSS/Containing_block](https://developer.mozilla.org/ko/docs/Web/CSS/Containing_block)

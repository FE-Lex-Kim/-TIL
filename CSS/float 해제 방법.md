# clear - 플로팅 해제

<br>

플로팅한 요소의 부모요소는 플로팅한 자식요소를 인식하지 못해 해당 자식요소를 감싸주지 못하여 문서의 흐름상에서 벗어나 레이아웃을 무너뜨린다.

<br>

이러한 문제를 해결하기 위해 float을 해제하는 방법을 사용한다.

<br>

## 첫번째 방법 - 부모요소도 floating 적용

부모요소는 플로팅된 자식요스를 포함하는 경우 부모요소는 높이를 인지 하지 못한다.

<br>

이 문제를 해결하는 방법은 부모요소에게도 float 속성을 반영하면 된다.

이렇게 하면 부모 요소는 자식요소의 높이를 인지하게 되지만 부모 요소도 floating이 되어 인라인 블럭의 특징을 가지게 된다.

<br>

## 두번째 방법 - 부모 요소에 `display: inline-block` 적용

부모 요소에 `display: inline-block` 속성을 사용한다.

이렇게 적용된 부모요소는 자식 요소의 높이를 인지하지만 부모 요소에 정의된 인라인 블럭 속성 특성으로 부모요소의 영역만큼 너비를 가진다.

<br>

## 세번째 방법 - 부모요소에 `overflow: hidden` 적용

부모 요소에 `overflow: hidden` 을 적용하여 자식요소가 부모요소 박스보다 클경우 자식 요소 박스의 콘텐츠를 보이지 않게해주는 속성이다.

<br>

그런데 부모 요소가 플로팅된 자식 요소로 인해 높이를 인지하지 못하는 상태에서 `overflow-hidden` 을 적용하면 부모요소는 넘치는 요소를 숨김 처리 하려고 자식 요소의 높이를 인지하기 위해 자동으로 높이 값을 계산한다.

<br>

- 주의 : 자식 요소중에 부모 요소를 벗어나면 보이지 않아 플로팅된 요소뿐만 아니라 다른 요소도 영향을 준다.

<br>

## 네번째 방법 - 플로팅된 자식요소의 마지막 형제요소 자리에 clear된 요소 생성

플로팅된 자식요소의 가장 마지막 형제요소 자리에 빈 요소를 만들어 clear 속성을 적용한다.

```jsx
<div class="box-group">
	<div class="box is-blue"></div>
	<div class="box is-yellow"></div>
  <div class="box is-green"></div>
	<div class="clear"></div>
</div>

출처: https://webclub.tistory.com/606 [Web Club]
```

<br>

- 주의: 불필요한 의미없는 빈요소를 사용하여 권장되지 않는다.

<br>

## 다섯번째 방법 - 부모요소에 가상요소(:after)를 생성해서 float을 해제해 준다.

부모요소에 `:after` 가상요소를 생성해서 내부에 빈 컨텐츠를 넣은후 clear를 해주어 float를 해주는 방법이다.

```css
.parent:after {
	content: "";
	display:block;
	clear:both;
}

출처: https://webclub.tistory.com/606 [Web Club]
```

<br>

- 가장 많이 쓰이고 있다.
- 구형 브라우저에서는 지원하지 않는다.

<br>

\*\* `:after` : 자신의 마지막 자식요소에 가상요소를 생성한다.

<br>

참고

- [https://webclub.tistory.com/606](https://webclub.tistory.com/606)

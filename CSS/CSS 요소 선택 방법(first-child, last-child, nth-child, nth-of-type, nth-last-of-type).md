- [CSS 요소 선택 방법(first-child, last-child, nth-child, nth-of-type, nth-last-of-type)](#css-요소-선택-방법first-child-last-child-nth-child-nth-of-type-nth-last-of-type)
  - [first-child, :last-child](#first-child-last-child)
  - [:nth-child(n)](#nth-childn)
  - [:nth-of-type(n)](#nth-of-typen)
  - [:nth-last-of-type(n)](#nth-last-of-typen)

# CSS 요소 선택 방법(first-child, last-child, nth-child, nth-of-type, nth-last-of-type)

<br>

## first-child, :last-child

부모의 자식 엘리먼트중 첫번째, 마지막 요소를 선택한다.

```jsx
<div>
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
</div>

div {
	li:first-child{
		color:red;
	}

	li:last-child {
	  color: lime;
	}
}
```

li의 첫번째 요소, 마지막 요소를 선택해서 CSS를 적용했다

<br>

## :nth-child(n)

**nth-child는 형제 사이에서 n번째 형제 요소를 선택한다.**

nth-child는 부모의 모든 자식 엘리먼트중에서 n번째 인것을 선택한다.

```jsx
<div>
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
</div>

div {
	li:nth-child(2) {
	  color: lime;
	}
}
```

li의 2번째 요소를 선택한다.

<br>

## :nth-of-type(n)

**nth-of-type은 같은 유형의 n번째 형제 요소를 선택한다.**

**nth-child는 부모의 모든 자식 엘리먼트중에서 n번째 인것을 선택한다고 했다.**

따라서 내가 적용하려는 엘리먼트의 순번을 정확하게 알아야 한다는 단점이 있다.

예제 코드를 보자.

```jsx
<div>
	<p>나는 p</p>
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
</div>

// 선택되는 것은 li 1이 선택된다.
div {
	li:nth-child(2) {
	  color: lime;
	}
}
```

위 코드에서 CSS가 적용되는 `li`는 텍스트가 `1`인 `li` 요소가된다.

그 이유는 div의 2번째 요소로 카운팅 했기때문이다.

<br>

따라서 nth-of-type을 사용하면, 같은 유형의 n 번째 형제 요소를 선택하니 보다 쉽게 선택가능하다.

```jsx
<div>
	<p>나는 p</p>
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
</div>

// 선택되는 것은 li 1이 선택된다.
div {
	li:nth-of-type(1) {
	  color: lime;
	}
}
```

이제 정상적으로 텍스트가 `1`인 `li`요소가 CSS 적용됬다.

<br>

## :nth-last-of-type(n)

nth-of-type은 같은 유형의 n번째 형제 요소를 선택한다고 했다.

<br>

**nth-last-of-type은 마지막 순서의 n번째 형제 요소를 선택한다.**

<br>

예를 들어)

- a:nth-last-of-type(1) → 맨 마지막 부터 계산해서 a 태그의 첫 번째 요소를 선택한다.

<br>

리액트를 사용하다보면, 거의 필수적으로 Fragment 태그를 사용한다.

**이때 Fragment의 자식 요소들은 상위 태그에 병합되어진다.**

**따라서 상위 태그의 CSS에서 마지막 요소로 선택하려고 해도 안된다.**

<br>

아마 Fragment는 자바스크립트에 의해 실행되어 병합되다보니,

CSSOM이 생겨나는 시점은 자바스크립트가 실행되는 시점보다 빨라서 마지막 요소로 선택되지 않는것 같다.

<br>

이해가 안된다면 예제코드를 보고 이해해보자.

```jsx
<Container>
	<>
    <a href="#">로그인</a>
    <a href="#">회원가입</a>
		<a href="#">마이페이지</a>
		<a href="#">게시판</a>
		<a href="#">리뷰 보기</a>
  </>
</Container>

// Container css에서 last-child로
// a태그를 선택하려고 해도 적용되지 않는다.
// (아래의 CSS 코드는 올바른 문법이 아니지만 추상적인 설명하기위해서 썼다.)
Container {
	& > a:last-child{
		color: red;
		font-size : 100px;
	}
}
```

`<>` Fragment가 Container로 병합 전에 CSS를 실행해서 정상적으로 적용되지 않는다.

따라서 이런 상황에서는 `nth-last-of-type`을 사용하면 좋다.

<br>

```jsx
<Container>
	<>
    <a href="#">로그인</a>
    <a href="#">회원가입</a>
		<a href="#">마이페이지</a>
		<a href="#">게시판</a>
		<a href="#">리뷰 보기</a>
  </>
</Container>

// 정상적으로 적용이 된다.
// (아래의 CSS 코드는 올바른 문법이 아니지만 추상적인 설명하기위해서 썼다.)
Container {
	& a:nth-last-of-type(1){
		color: red;
		font-size : 100px;
	}
}
```

이때는 &의 자식요소들을 모두 포함한 CSS 문법을 사용한다.

- `& a:nth-last-of-type`
- `& > a:nth-last-of-type` 이 아니다!

<br>

참고

- [https://developer.mozilla.org/en-US/docs/Web/CSS/:nth-last-of-type](https://developer.mozilla.org/en-US/docs/Web/CSS/:nth-last-of-type)
- [https://developer.mozilla.org/ko/docs/Web/CSS/:nth-child](https://developer.mozilla.org/ko/docs/Web/CSS/:nth-child)
- [https://aboooks.tistory.com/319](https://aboooks.tistory.com/319)

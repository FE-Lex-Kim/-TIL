## `data-`속성

<br>

data- 전역특성은 사용자 지정 데이터 특성이라는 특성 클래스를 생성함으로써 임이의 데이터를 자바스크립트로 HTML과 DOM사이에서 교환할 수 있는 방법이다.

<br>

### 자바스크립트에서 사용

자바스크립트에서 dataset 객체를 통해 data 속성을 가져올 수 있다.

이때 `data-` 뒷 부분을 사용한다.

<br>

```jsx
<article
  id="electriccars"
  data-columns="3"
  data-index-number="12314"
  data-parent="cars"
>
  ...
</article>
```

<br>

```jsx
var article = document.getElementById("electriccars");

article.dataset.columns; // "3"
article.dataset.indexNumber; // "12314"
article.dataset.parent; // "cars"
```

<br>

해당 속성은 문자열이며 읽거나 쓸 수 있다.

`article.dataset.columns = 5`

해당 속성의 값을 변경도 가능한다.

<br>

### CSS에서 사용

```jsx
article::before {
  content: attr(data-parent);
}
```

<br>

attr() 함수의 생성된 content를 사용하면 된다.

attr의 파라미터값으로 `data-` 이름이 오면된다.

<br>

CSS의 속성 선택자로도 데이터에 따라 스타일을 바꾸는데 사용할 수 있다.

```jsx
article[data-columns='3'] {
  width: 400px;
}
article[data-columns='4'] {
  width: 600px;
}
```

<br>

### 장점

hidden 태그를 사용해서 이전과 같이 숨겨두고 데이터를 저장할 필요가 없다.

따라서 HTML 스크립트가 훨씬 간결해진다.

<br>

예제 코드)

```jsx
<div class="item number" seq="1">1</div> // 타입은 number, 순서는 1

<div class="item string" seq="2">A</div> // 타입은 string, 순서는 2

출처: https://mygumi.tistory.com/341 [마이구미의 HelloWorld]
```

위의 예제처럼 해당 텍스트에 대한 정보가 클래스에 저장되어 있어 정확하게 어떤 정보인지 가독성이 좋지않고 명확하지 않다.

<br>

```jsx
<div class="item" data-type="number" data-seq="1">1</div>

<div class="item" data-type="string" data-seq="2">A</div>

출처: https://mygumi.tistory.com/341 [마이구미의 HelloWorld]
```

위와 같이 바꾸면 data- 속성을 사용해 type이 어떤것인지 명확히 하고 순서가 몇번째인지 명확하고 가독성 좋게 만들 수 있다.

<br>

### 문제점

보여야하고 접근 가능해야하는 내용은 데이터 속성에 저장하면 안된다.

접근할수 있는 기능이 없기 때문이다.

<br>

또한 크롤러가 데이터 속성의 값을 찾지 못한다.

또한 dataset을 지원하지 않는 버전들이 있다.

<br>

참고

- [https://developer.mozilla.org/ko/docs/Web/HTML/Global_attributes/data-\*](https://developer.mozilla.org/ko/docs/Web/HTML/Global_attributes/data-*)
- [https://mygumi.tistory.com/341](https://mygumi.tistory.com/341)

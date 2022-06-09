- [호버, 클릭시 별점 기능 UI](#호버-클릭시-별점-기능-ui)
  - [1. 호버시 별점 색상 변경](#1-호버시-별점-색상-변경)
  - [2. 이전 순번의 별점 색상 변경](#2-이전-순번의-별점-색상-변경)
    - [호버시 일반 형제 결합자를 사용해서 CSS를 변경](#호버시-일반-형제-결합자를-사용해서-css를-변경)
  - [3. 0.5점 별 호버시 색상 변경](#3-05점-별-호버시-색상-변경)
  - [4. 별점 클릭시 이전 순번의 별점 색상 유지](#4-별점-클릭시-이전-순번의-별점-색상-유지)

# 호버, 클릭시 별점 기능 UI

<br>

## 1. 호버시 별점 색상 변경

**호버시 별의 내부 색상을 변경하는 방법은 2가지 정도 생각 할 수 있다.**

1. 호버시 **하얀색 별점 이미지에서 빨간색 이미지로 변경**
2. 호버시 SVG를 사용해서 **CSS 컬러 변경**

<br>

첫 번째 방법은 **이미지를 두번 불러오기 때문에** 네트워크 성능상 두 번째 보다 좋지 않다.

두 번째 방법인 **svg을 사용해서 color css 변경이 훨씬 성능에 좋다.**

<br>

비지니스 로직

- 호버 이벤트 발생
- 해당 요소의 CSS 컬러 변경

<br>

## 2. 이전 순번의 별점 색상 변경

**만약 3번 별점에 호버시 1번, 2번 별점이 마찬가지로 빨간색으로 변경 되어야한다.**

**이것도 구현 하는 방법은 2가지 정도 생각 할 수 있다.**

1. 별 호버시 해당 요소 이미지의 **data-number로 state에 저장한다.**
   1. 3번 별을 호버시, state를 3으로 변경
   2. 각각 1, 2, 3번 별의 색상을 변경
2. 호버시 **CSS 선택자 중 일반 형제 결합자를 사용**해서 CSS를 변경시킴

<br>

첫 번째 방법은 **Javascript을 사용해서 상태 값을 변경하므로** 컴포넌트가 업데이트가 되어져 성능이 좋지않다.

**두 번째 방법은 CSS 만을 변경하므로 첫 번쨰 방법보다 성능이 좋다.**

<br>

그렇다면, 두 번째 방법으로 알아보자.

<br>

### 호버시 일반 형제 결합자를 사용해서 CSS를 변경

1,2,3,4,5번 중에 3번을 호버 한다고 하면, 1번과 2번의 색상이 같이 채워져야한다.

일반 형제 결합자는 A ~ B{} 라고 하면, A 뒤에 위치하는 요소들만 적용된다.

하지만 HTML 상의 요소의 위치는 1번과 2번이 3번의 앞에 존재한다.

따라서 `3번 ~ 1번 {}` 이 가능할 수 없다.

```html
<img src="star" alt="1번 별점" />
<img src="star" alt="2번 별점" />
<img src="star" alt="3번 별점" />
```

<br>

**대신에 순서를 5,4,3,2,1번 역순으로 HTML 요소를 위치 시키면 가능하다.**

1. 5,4,3,2,1번 별들을 각각 HTML 위치킴

   ```html
   <div>
     <img src="star" alt="5번 별점" />
     <img src="star" alt="4번 별점" />
     <img src="star" alt="3번 별점" />
     <img src="star" alt="2번 별점" />
     <img src="star" alt="1번 별점" />
   </div>
   ```

2. 요소들의 레이아웃을 `flex-direction: row-reverse` 시켜서 실제 보여질때는 1,2,3,4,5번 순으로 만들어준다.
   - ☆(1번)☆(2번)☆(3번)☆(4번)☆(5번)
   - 순으로 위치 시킨다.
3. 호버시 일반 형제 결합자을 사용해 뒤의 img 형제 요소들의 CSS를 변경시킨다.

   ```css
   lmg:horver {
     & ~ img {
       // svg 색상은 fill로 관리한다.
       fill: red;
     }
   }
   ```

※ 일반 형제 결합자(General sibling combinator) : `A ~ B{}` A태그 뒤의 형제 요소중 모든 B 태그를 선택함.

<br>

## 3. 0.5점 별 호버시 색상 변경

별점은 1점만이 아니라 0.5점도 존재하므로 **별점을 절반만 호버하면 절반만 색상 변경 시켜야한다.**

<br>

- **별의 이미지를 절반을 기본단위로 한다.**
  - 왼쪽 별 절반 + 오른쪽 별 절반 = 별 하나

```html
<div>
  <img src="star" alt="5번 별점" />
  <img src="star" alt="4.5번 별점" />

  <img src="star" alt="4번 별점" />
  <img src="star" alt="3.5번 별점" />

  <img src="star" alt="3번 별점" />
  <img src="star" alt="2.5번 별점" />

  <img src="star" alt="2번 별점" />
  <img src="star" alt="1.5번 별점" />

  <img src="star" alt="1번 별점" />
  <img src="star" alt="0.5번 별점" />
</div>
```

<br>

## 4. 별점 클릭시 이전 순번의 별점 색상 유지

호버 뿐만 아니라 **별을 클릭한 후에도 이전 순번의 별 색상들이 유지되어야한다.**

<br>

`img` 태그를 사용해서 별을 만들었지만, 대신해서 **`label` 태그로 별 이미지를 만들고**

**클릭한 후 상태값을 가지려면 `radio type`의 `input` 태그를 사용하면된다.**

`input`은 클릭한 후의 상태값을 가지기 위한 용도이므로 `display:none` 으로 없애준다.

```html
<div>
  <label for="rate5">5번 별</label>
  <input name="rateGroup" type="radio" id="rate5" src="{Star}" />

  <label for="rate4.5">4.5번 별</label>
  <input name="rateGroup" type="radio" id="rate4.5" src="{Star}" />

  <label for="rate4">4번 별</label>
  <input name="rateGroup" type="radio" id="rate4" src="{Star}" />

  <label for="rate3.5">3.5번 별</label>
  <input name="rateGroup" type="radio" id="rate3.5" src="{Star}" />

  <label for="rate3">3번 별</label>
  <input name="rateGroup" type="radio" id="rate3" src="{Star}" />

  <label for="rate2.5">2.5번 별</label>
  <input name="rateGroup" type="radio" id="rate2.5" src="{Star}" />

  <label for="rate2">2번 별</label>
  <input name="rateGroup" type="radio" id="rate2" src="{Star}" />

  <label for="rate1.5">1.5번 별</label>
  <input name="rateGroup" type="radio" id="rate1.5" src="{Star}" />

  <label for="rate1">1번 별</label>
  <input name="rateGroup" type="radio" id="rate1" src="{Star}" />

  <label for="rate0.5">0.5번 별</label>
  <input name="rateGroup" type="radio" id="rate0.5" src="{Star}" />
</div>
```

<br>

이후 별을 클릭시 input 태그가 `checked` 된다.

`checked input` 뒤에 있는 label 들을 일반 형제 선택자로 색상을 채워준다.

```css
input[type="radio"] {
  display: none;
}

&:horver ~ label {
  // svg 색상은 fill로 관리한다.
  fill: red;
}

input:checked ~ label {
  // svg 색상은 fill로 관리한다.
  fill: red;
}
```

<br>

![호버,클릭시 별점 기능UI](../Images/호버,클릭시%20별점%20기능UI/호버,클릭시%20별점%20기능UI-1.gif)

<br>

참고

- [https://shubamba.tistory.com/65](https://shubamba.tistory.com/65)
- [https://blogpack.tistory.com/993](https://blogpack.tistory.com/993)
- [https://puterism.com/svg-viewport-and-viewbox/](https://puterism.com/svg-viewport-and-viewbox/)

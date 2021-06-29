# 효율적인 CSS

<br>

## CSS 스타일 적용 규칙

스타일 엔진은 4개의 카테고리로 스타일 규칙을 분류한다.

ID규칙, Class 규칙, Tag 규칙, Universal 규칙

<br>

키 선택자 : 마지막 선택자(선택되는 엘리먼트)를 의미한다.

<br>

### ID 규칙

ID 선택자를 키 셀렉터로 가지는 규칙들로 이루어진다.

```css
#info {
  ...;
}
button#info {
  ...;
}
food > fruits > #info {
  ...;
}
```

<br>

### Class 규칙

키 선택자에 class 가 명시되어 있을경우

```css
button.toolbarButton {…} /* Class 규칙 */
.fancyText {…}	/* Class 규칙 */
menuitem > .menu-left[checked="true"] {…} /* Class 규칙 */

출처: https://webclub.tistory.com/361#recentEntries [Web Club]
```

<br>

### Tag 규칙

키 선택자에 class와 ID가 명시되지 않고 tag가 있으면 tag 규칙에 속한다.

<br>

### Universal 규칙

위의 세개의 규칙에 속하지 않으면 모든 규칙은 Universal 규칙에 속한다.

<br>

### 스타일 엔진이 어떤 규칙을 결정하는지

스타일 엔진은 키 셀렉터부터 시작해서 왼쪽으로 이동하면서 엘리먼트가 규칙에 적합한지 확인한다.

만약 엘리먼트가 이 규칙에 적합하거나 적합하지 않다는게 확인되면 멈춘다.

<br>

성능을 향상시키는 방법이 이것에 있다.

주어진 엘리먼트가 적합한지 확인하는데 고려해야할 규칙 수가 적을수록 성능이 좋아진다.

<br>

### 성능 향상시키는 방법

1. Universal 규칙을 피하기
2. ID 규칙에 쓸모없는 태그나 클래스 붙이기 말기

   - 만약 어떤 규칙이 키 선택자로 ID 선택자를 가지면 여기에 부차적인 태그를 붙이지 말기

   ```css
   button#backButton {…} /* 나쁨 */
   .menu-left#newMenuIcon {…} /* 나쁨 */
   #backButton {…} /* 좋음*/
   #newMenuIcon {…} /* 좋음 */
   ```

   - ID는 고유하므로 이곳에 추가적인 태그를 붙이면 매칭 작업이 더디다.

3. class 규칙에 쓸모없는 태그 붙이지 않기
   - 카멜, 파스칼, 스네이크 케이스.. 등등을 사용해서 사용하면 규칙을 줄일 수 있다.
4. 가장 명확한 규칙 사용
   - Tag 규칙을 사용하지 않고 최대한 Class 규칙에 속하는 규칙을 사용한다.
5. 자손 선택자를 피하기

   - 자손 선택자는 CSS에서 가장 속도가 느린 선택자이다.
   - 심지어 Tag규칙과 Universal 규칙에 속하면 더 느리다.
   - 자손 선택자는 속도가 너무 느려 파이어폭스에서도 제거되었다.
   - 대체로 `자식 선택자`를 사용하면된다.

     ```css
     treehead treerow treecell {…} /* 나쁨 */
     treehead > treerow > treecell {…} /* 개선되었지만 여전히 나쁨 (다음 지침을 보라) */
     ```

6. Tag 규칙에서 자식 선택자 사용X
   - Tag 규칙에 속하는 규칙에 자식 선택자를 사용하지 않기
7. 자식 선택자를 사용할때 항상 주의하기
8. 상속을 사용하기

   ```css
   #bookmarkMenuItem > .menu-left {
     list-style-image: url(blah);
   } /* 나쁨 */
   #bookmarkMenuItem {
     list-style-image: url(blah);
   } /* 좋음 */
   ```

<br>

## 애니메이션 GPU 사용하기

브라우저에서 CSS 애니메이션 작업을 최적화할 수 있다.

이미지 또는 어떠한 컨텐트를 이동시킬때 postion으로 이동시키는 것이 아닌 transform을 통해 GPU를 사용하면 빠른 최적화를 통해 성능 향상 시킬 수 있다.

<br>

## Will-change CSS 속성 사용하기

Will-change CSS 속성은 요소에 예상되는 변화의 종류에 관해 브라우저에 미리 제공한다.

그래서 실제 요소가 변화되기전에 미리 브라우저는 적절하게 최적화 작업을 한다.

이러한 종류의 최적화는 잠재적으로 성능 비용이 큰 작업을 실제로 요청하기 전에 미리 실행해서 페이지의 반응성을 높일 수 있다.

<br>

참고

- [https://developer.mozilla.org/en-US/docs/Learn/Performance/CSS](https://developer.mozilla.org/en-US/docs/Learn/Performance/CSS)
- [https://webclub.tistory.com/361](https://webclub.tistory.com/361)

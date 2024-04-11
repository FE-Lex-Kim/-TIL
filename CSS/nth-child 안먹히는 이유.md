# nth-child 안먹히는 이유

```html
<li>
  <span>1</span>
  <span>2</span>
  <h3>Heading</h3>
  <span>3</span>
  <span>4</span>
  <span>5</span>
</li>
```

여기서 `<span>3</span>` 에 핑크색 배경을 주고싶어 한다고 해보자.

이때 생각하기로 아래와 같이 하면 된다고 생각한다.

```css
li > span:nth-child(3) {
  background-color: pink;
}
```

아쉽지만 생각대로 동작하지 않는다.

왜그럴까.

nth-child를 사용하면 span요소중 3번째를 찾는게 아니다.

1. li의 요소중에 3번째 **순서를** 먼저 찾는다.
2. 이때 3번째 요소가 h3이다.
3. `span:nth-child(3)` 으로 span요소를 선택한다고 했는데 h3이다. 따라서 올바르지 않은 코드이므로 작동하지 않는다.

<br>

우리가 원하는 대로 동작하기 위해서는 nth-child가 아니라 `nth-of-type()`을 사용해야한다.

```html
<li>
  <span>1</span>
  <span>2</span>
  <h3>Heading</h3>
  <span>3</span>
  <span>4</span>
  <span>5</span>
</li>
```

위의 코드에서 span요소중 3번째에(`<span>3</span>`) pink를 줘보자.

<br>

배운대로 nth-of-type을 사용하면 예상대로 잘 작동한다.

```css
li > span:nth-of-type(3) {
  background-color: pink;
}
```

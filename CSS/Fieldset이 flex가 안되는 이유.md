Fieldset이 flex가 안되는 이유
===

**Microsoft Edge**와 **Google Chrome**에는 `fieldset`내부에서 `flex box` 와 `grid` 레이아웃을 사용할 수 없는 버그가 존재한다.

![fieldset](../Images/fieldset.jpg)

```html
<form action="">
  <fieldset>
    <div class="box1"></div>
    <div class="box2"></div>
  </fieldset>
</form>
```

```css
fieldset{
  border: 5px solid black;
  display: flex;
  justify-content: flex-end;
  width: 400px;
}
.box1{
  display: inline-block;
  width: 100px;
  height: 100px;
  background-color: red;
}
.box2{
  display: inline-block;
  width: 100px;
  height: 100px;
  background-color: blue;
}

```


----
## 해결방법



![fieldset](../Images/fieldset1.jpg)

> `<div role='group'>`처럼 하면 fieldset과 비슷한 역활로서 의미을 가진다.<br>그러므로 div태그에 flex 걸어 해결해 줄수있다.
```html
<form action="">
  <div role='group'>
    <div class="box1"></div>
    <div class="box2"></div>
  </div>
</form>
```

```css
div[role='group']{
  border: 5px solid black;
  display: flex;
  justify-content: flex-end;
  width: 400px;
}
.box1{
  display: inline-block;
  width: 100px;
  height: 100px;
  background-color: red;
}
.box2{
  display: inline-block;
  width: 100px;
  height: 100px;
  background-color: blue;
}
```
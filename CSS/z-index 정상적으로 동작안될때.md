# z-index 정상적으로 동작안될때

# z-index 정상적으로 동작안될때

z-index가 정상적으로 작동이 안될때가 있다.

만약 부모 자식 관계요소에 있을때는, z-index가 정상적으로 안될때가 많다.

<br>

**만약 부모요소에 position이 있을 경우 내부의 요소들 끼리만 z-index가 영향이 끼친다.**

<br>

예를들어

a 박스의 postion 값이 존재한다면,

```jsx
<div class='a'>
	<div class='b'>
	<div class='c'>
</div>

<div class='d'></div>
```

아무리 `b`에서 z-index를 9999 한다고 해도 `d`보다 위에 존재할 수 없다.

b의 z-index는 a 박스 내부에서만 동작한다.

c와의 z-index의 값을 계산할 뿐이지 d 까지 영향을 끼칠 수 없다.

<br>

참고

- [https://www.zerocho.com/category/CSS/post/5a18b330e9c0ec001b08238e](https://www.zerocho.com/category/CSS/post/5a18b330e9c0ec001b08238e)

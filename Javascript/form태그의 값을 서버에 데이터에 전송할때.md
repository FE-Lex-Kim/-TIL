# form태그의 값을 서버에 데이터에 전송할때

<br>

form태그로 메소드 전송방식을 post방식으로 하게되면

여러데이터를 서버쪽에 한번에 전송할 수 있다는 장점이 있다.

<br>

하지만 submit의 브라우저 기본값을

화면 렌더링을 하고 페이지가 넘어가게 된다.

<br>

또한 데이터를 넘기는 것을 기본값으로 하게 된다.

때문에 preventDefault() 메소드를 사용하여 기본값을 멈추게 하면된다.

<br>

하지만 렌더링이 되는것은 막지만

한번에 서버에 전송하는것 또한 막게된다.

<br>

때문에 서버에 보낼 form태그의 값만들 가져와 보내주면 된다.

```css
const formData = new FormData(form 태그요소);
```

<br>

브라우저에서 확인하려고 하면 formData 값을 확인 할수없다.

단순한 객체가 아닌 XMLHttpRequest 전송을 위해 설계된 특수한 객체 형태이기 때문이다.

확인하는 방법은

```css
for (let key of formData.keys()) {
  console.log(key);
}
for (let key of formData.values()) {
  console.log(key);
}
```

key와 values를 확인할수 있다.

<br>

form태그에서 name 속성을 가진 노드들의 데이터만 가져온다.

<br>

새로만든 객체안에 할당을 해서 

서버쪽에 데이터를 보내면된다.

```css
	e.preventDefault();

  const formData = new FormData($fromNode);
  const ob = {};

  for (const pair of formData) {
    console.log(pair);
    ob[pair[0]] = pair[1];
  }

  fetch('/users', {
    method:'POST',
    headers: { 'content-Type': 'application/json' },
    body: JSON.stringify(ob)
  })
```

form태그에서 method를 사용해 get과 post로 서버와 데이터를 통신하는것은 보안상 좋지않다.

때문에 form태그는 감싸주는 부모태그로만 역활을 하게 해준다.

<br>

데이터와 통신은 자바스크립트에서 해주어야한다.
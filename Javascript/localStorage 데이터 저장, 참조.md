## localStorage 데이터 저장, 참조

<br>

**localStorage 안에 데이터를 저장하기**

<br>

setIte 메서드를 사용하여 첫번째 인수로 저장할 데이터를 참조할 수 있게하는 식별자 이름을 설정한다.

<br>

두번째 인수로 저장하려는 데이터를 JSON.stringify로 json형식의 문자열로 변경시켜준뒤 넣어준다.

<br>

기존에 로컬스토리지에 키로 존재하는 식별자를 첫번째 매개변수로 넣게되면

덮어 씌우게 된다.

<br>

**localStorage 안에 데이터를 참조하기**

<br>

저장할때 설정해두었던 식별자를

getItem메서드의 첫번째 인수로 넣어준뒤

<br>

그반환값이 json형식으로 반환되므로

parse하여 변환시켜준다.

<br>

```css
// 로컬스토리지에 데이터를 쌓이게 해준다.
localStorage.setItem('login', 
  JSON.stringify({
    id: 'Alex123', 
    name: 'Alex', 
    genre: 'SF', 
    bookmarks: ["726739", "718444", "524047", "531219", "652004", "413518"]
}));

let user = JSON.parse(localStorage.getItem('login'));
```

<br>

만약 getItem으로 참조하려는 키가 없는 경우

`null` 값이 반환된다.
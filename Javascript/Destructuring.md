Destructuring
===
배열 또는 객체를 Destructuring(비구조화, 파괴)하여 개별적인 변수에 할당하는 것이다. 배열 또는 객체 리터럴에서 필요한 값만을 추출하여 변수에 할당하거나 반환할 때 유용하다

## 1.배열 디스트럭처링

```js
let name = ['alex' ,'james' ,'jin'];

let [name1, name2, name3] = name;

console.log([name1, name2, name3])

// [ 'alex', 'james', 'jin' ] 출력
```

왼쪽 배열과 오른쪽 배열의 인덱스 기준으로 할당된다.

``` js
let [a,b,c] = [1,2,3];
console.log(a,b,c)
// 1 2 3
```

왼쪽인덱스의 남은 부분은 들어가지 않는다.
```js
let [a,b] = [1,2,3];
console.log(a,b);
// 1 2
```
기본값을 줄수도 있다.   
```js
let [a,b,c = 3] = [1,2];
console.log(a,b,c);
// 1 2 3
```

## 2. 객체 디스트럭처링
객체의 각 프로퍼티를 추출 하여 넣는다.
```js
let myname = {firstName : 'Alex', lastName: 'Kim'};

let {firstName, lastName} = myname;

console.log(firstName, lastName);
```

객체 디스트럭처링을 위해서 왼쪽에 객체 형태의 변수리스트가 필요하다
```js
const { firstName, lastName } = { firstName: 'Alex', lastName: 'Kim' };

console.log({ firstName, lastName });

 // { firstName: 'Alex', lastName: 'Kim' }
```

함수의 파라미터에도 사용할 수 있다.

```js
const ironMan = {
    name : '토니 스타크',
    actor : '로버트 다우니 주니어',
    alias : '아이언맨'
};

const captimAmerica = {
    name : '스티븐 로저스',
    actor : '크리스 에반스',
    alias : '캡틴 아메리카'
};

print = ({name, actor, alias}) => {
    const text = `${alias}(${name}) 역활을 맡은 배우는 ${actor} 입니다.`;
    console.log(text);
};

print(ironMan);
// 아이언맨(토니 스타크) 역활을 맡은 배우는 로버트 다우니 주니어 입니다.
```
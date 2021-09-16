# React Hook useState

함수형 컴포넌트는 stateless 하다는 점이 있다.

하지만 useState를 사용해서 상태값을 사용할 수 있다.

<br>

## useState 선언

react에서 `useState` 를 import 해온뒤 호출한다.

<br>

`useState`를 호출하면 2개의 배열 반환값을 가진다.

배열의 첫번째 인덱스의 값은 state가 된다.

두번째 인덱스 값은 state를 변경할 수 있는 함수이다.

<br>

이렇게 가진 2개의 배열을 구조 분해 할당을 통해 각각 원하는 네이밍을 해준다.

보통 두번째의 state를 변경시켜주는 함수는 "set + state이름" 을 넣어준다.

```jsx
import React, { useState } from "react";

function Exmaple() {
  let [count, setCount] = useState(0);
  return <div></div>;
}

export default Exmaple;
```

<br>

위의 예제를 보면 useState에 인수로 0을 넣어주었다.

이 값은 count(state)에 들어갈 초기값을 설정해준것이다.

class와 다르게 항상 state에 객체값을 넣어줄 필요가 없다.

<br>

## useState 갱신

state값을 갱신하기 위해서는 두번째 인덱스의 함수를 호출해주면된다.

위의 예제에서 `setCount`를 호출해주면된다.

```jsx
import React, { useState } from "react";

function Exmaple() {
  let [count, setCount] = useState(0);

  return (
    <>
      <div>{count}</div>
      <button
        onClick={() => {
          setCount(count + 1);
        }}
      >
        Click me
      </button>
    </>
  );
}

export default Exmaple;
```

<br>

이렇게 `setCount` 를 호출해주면 컴포넌트는 state가 변경되었기 때문에 다시 컴포넌트 비교 알고리즘을 통해 재조정 과정을 거친다.

<br>

### 여러개의 useState 사용

한 컴포넌트 안에서 여러개의 `useState`를 호출해서 state를 여러개 가질 수 있다.

각각 state의 네이밍은 서로 다르게 설정 가능하다.

```jsx
import React, { useState } from "react";

function Exmaple() {
  let [name, setName] = useState("Alex");
  let [age, setAge] = useState(27);
  let [location, setLocation] = useState("Bucheon");
  let [skill, setSkill] = useState("React");

  return (
    <>
      <div>{age}</div>
      <button
        onClick={() => {
          setAge(age + 1);
        }}
      >
        Click me
      </button>
    </>
  );
}

export default Exmaple;
```

<br>

이렇게 여러개의 state를 설정가능하다.

또한 연관성 있는 state들은 배열, 객체로 데이터를 묶어서 관리할 수 있다.

그렇게 된다면 훨씬 가독성 및 유지보수도 좋을 것이다.

```jsx
import React, { useState } from "react";

function Exmaple() {
  let [myInfo, setMyInfo] = useState({
    name: "Alex",
    age: 27,
    location: "Bucheon",
    skill: "React",
  });

  return (
    <>
      <div>{myInfo.age}</div>
      <div>{myInfo.name}</div>
      <div>{myInfo.location}</div>
      <div>{myInfo.skill}</div>
      <button
        onClick={() => {
          // useState는 state값을 대체 시켜준다. 즉, 덮어버린다.
          setMyInfo({ ...myInfo, name: "James", age: myInfo.age + 1 });
        }}
      >
        Click me
      </button>
    </>
  );
}

export default Exmaple;
```

<br>

하지만 위의 예제에서 보이는 것처럼 클릭 이벤트로 **age만 변경 시키려고** 로직을 구성하고 싶을때는 spread 문법(...)을 사용해서 현재 객체와 수동적으로 병합을 시켜주어야한다.

<br>

즉, useState는 class의 this.setState와 다르게 state의 값을 갱신할때 **"병합"** 시키는 것이아니라 **"대체"** 한다는 점이다.

<br>

**따라서 state끼리 연관이 있어도 함께 변경되는 state끼리 묶어서 분할하는 것을 추천한다.**

```jsx
import React, { useState } from "react";

function Exmaple() {
  let [myInfo, setMyInfo] = useState({
    name: "Alex",
    age: 27,
  });

  let [myInfo2, setMyInfo2] = useState({
    location: "Bucheon",
    skill: "React",
  });

  return (
    <>
      <div>{myInfo.age}</div>
      <div>{myInfo.name}</div>
      <div>{myInfo2.location}</div>
      <div>{myInfo2.skill}</div>
      <button
        onClick={() => {
          // useState는 state값을 대체 시켜준다. 즉, 덮어버린다.
          setMyInfo({ name: "James", age: myInfo.age + 1 });
        }}
      >
        Click me
      </button>
    </>
  );
}

export default Exmaple;
```

<br>

또한 이렇게 만들게 되었을때 장점이 하나 더 있다.

컴포넌트 끼리 중복되는 코드가 있을때 커스텀 Hook을 통해서 만들기 유리하다는 점이 있다.

```jsx
import React, { useEffect, useState } from "react";

function useChangeMyinfo(name, age) {
  let [myInfo, setMyInfo] = useState({
    name,
    age,
  });

  useEffect(() => {
    document.addEventListener("click", () => {
      setMyInfo({ name: "James", age: age + 1 });
    });
  }, [age]);

  return myInfo;
}

function Exmaple() {
  let { name, age } = useChangeMyinfo("Alex", 27);

  let [myInfo2, setMyInfo2] = useState({
    location: "Bucheon",
    skill: "React",
  });

  return (
    <>
      <div>{age}</div>
      <div>{name}</div>
      <div>{myInfo2.location}</div>
      <div>{myInfo2.skill}</div>
    </>
  );
}

export default Exmaple;
```

<br>

참고

- [https://ko.reactjs.org/docs/hooks-state.html](https://ko.reactjs.org/docs/hooks-state.html)

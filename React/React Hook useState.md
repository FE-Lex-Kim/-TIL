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

## 비동기적 일괄처리되는 setState

setState는 **비동기적으로 일괄처리 되어진다.**

모든 컴포넌트가 **자신의 이벤트 핸들러에서 setState가 호출될때까지** React는 리렌더링 하지 않고 내부적으로 기다리고 있다.

<br>

**state이 갱신되는 시점은 리렌더링** 되어질때이다.

따라서 **모든 컴포넌트의 setState가 동시에 일괄적으로 처리되어질때까지 state는 갱신 되어지지 않는다.**

<br>

### 왜 비동기적으로 일괄처리 되어질까?

부모와 자식에서 setState가 호출된다면 자식은 두번 렌더링 되지 않는다.

setState는 이벤트 핸들러에 의해서 호출되지만 비동기적으로 동작한다.

이후 state 갱신은 모든 setState가 호출된뒤(가장 마지막 이벤트 핸들러가 끝날 시점) state들이 일괄적으로 업데이트 된다.

<br>

**setState가 호출될때마다 리렌더링 된다면, 성능이 안 좋아 진다.**

따라서 여러번 리렌더링 되는 것을 방지해서 **한번의 리렌더링으로 성능을 크게 향상시킨다.**

<br>

## 이전 state 값 받아와서 갱신

이전 state를 사용해서 새로운 state를 계산하는 경우 setState에 콜백함수를 전달해서 갱신할 수 있다.

콜백함수는 인자는 이전 state 값을 가지고 있다.

<br>

일반적으로 state를 갱신할때

```jsx
const [count, setCount] = useState(0);
setCount(count + 1);
```

<br>

콜백함수로 state를 갱신할때

```jsx
const [count, setCount] = useState(0);
setCount((prevState) => prevState + 1);
```

<br>

setState는 비동기적으로 일괄처리 되어진다.

하지만 **setState에 콜백함수를 전달하면 이전에 변경한 state 값에 접근 할 수 있어서, 이전 값(가장 최근에 setState로 갱신한 값)을 기준으로 갱신가능하다.**

setState는 일괄적으로 처리되기 때문에 여러 업데이트 사항이 충돌없이 차례대로 반영된다.

<br>

예제코드

```jsx
import React, { useState } from "react";

function FcSetStaetCount(props) {
  const [count, setCount] = useState(0);
  return (
    <>
      <button onClick={onClick}>click me</button>
      <p>{count}</p>
    </>
  );

  function onClick() {
    setCount(count + 1);
    setCount(count + 1);
    setCount(count + 1);
  }
}

export default FcSetStaetCount;
```

<br>

당연히 비동기적으로 동작하므로 리렌더링이 되지않아, state가 갱신 안되어서,

세 개의 setCount count 값은 항상 1이 된다.

```jsx
import React, { useState } from "react";

function FcSetStaetCount(props) {
  const [count, setCount] = useState(0);
  return (
    <>
      <button onClick={onClick}>click me</button>
      <p>{count}</p>
    </>
  );

  function onClick() {
    setCount((prevState) => prevState + 1);
    setCount((prevState) => prevState + 1);
    setCount((prevState) => prevState + 1);
  }
}

export default FcSetStaetCount;
```

콜백함수의 인자는 이전의 setState로 변경한 state값에 접근이 가능하다.(리렌더링이 되진 않았지만, 이전의 setState의 변경값을 접근가능함)

<br>

참고

- [https://ko.reactjs.org/docs/hooks-state.html](https://ko.reactjs.org/docs/hooks-state.html)
- [https://ko.reactjs.org/docs/hooks-reference.html#functional-updates](https://ko.reactjs.org/docs/hooks-reference.html#functional-updates)
- [https://ko.reactjs.org/docs/faq-state.html](https://ko.reactjs.org/docs/faq-state.html)

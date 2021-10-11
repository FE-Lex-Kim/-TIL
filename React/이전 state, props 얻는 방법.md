# 이전 state, props 얻는 방법

<br>

현재 React에서 이전 state, props의 값을 참조하는 Hook은 없다.

따라서 직접 이전 state, props 값을 저장하는 로직을 만들어야 한다.

<br>

```jsx
import React, { useEffect, useRef, useState } from "react";

function PreviousCount(props) {
  const [count, setCount] = useState(0);
  let prevCountRef = useRef();

  useEffect(() => {
    prevCountRef.current = count;
  }, [count]);

  const prevCount = prevCountRef.current;
  return (
    <>
      <button onClick={increaseCount}>increase count</button>
      <div>count : {count}</div>
      <div>prevCount : {prevCount}</div>
    </>
  );

  function increaseCount() {
    setCount(count + 1);
  }
}

export default PreviousCount;
```

<br>

코드 로직 동작 과정

<br>

### 최초 Mount 시

1. 최초에 `useState`, `useRef`를 호출해서 `count` 값은 `0`, `prevCountRef` 값은 `undefined` 값이 들어간다.
2. 이후 `useEffect`가 호출 되고 인수인 첫번째 콜백함수는 비동기 함수가 되므로 모든 로직이 종료된 이후에 호출된다.
3. 다음 `prevCount` 값은 `prevCountRef.current` 값이 할당되는데 이 값은 `undefined` 값이 된다.
4. 이후에 `useEffect`의 콜백함수가 호출어서, `prevCountRef.current` 값은 `count` 값인 `0`이 들어간다.

<br>

### setCount로 컴포넌트 update 시

1. `useState`가 호출되고 `setCount` 값이 였던 `1`이 `count`값에 들어가서 반환되어 진다.
2. `PrevCountRef` 값은 이전에 `useRef` 값의 `currnet`이 `0`인 객체가 들어간다.
3. `useEffect`가 호출되고 인수의 콜백함수는 이후에 호출되어 진다.
4. `prevCount`값에 `prevCountRef.current` 값이 할당 되어지는데, `prevCountRef.current` 값은 최초 Mount시에 `useEffect`의 콜백함수가 호출한것에 의해서 `0`값이 들어간다.
5. 따라서 이후 `prevCount` 값은 `0`이되고 현재 `count`값은 업데이트된 `1`이 된다.

<br>

update의 로직이 계속 반복되어서 state 또는 props가 저장되어서 이전의 값을 사용할 수 있다.

<br>

## usePrvious

<br>

해당 로직을 관리하고 쉽고 다른 컴포넌트에 사용할 수 있게 custom hook으로 만들 수 있다.

<br>

usePrevious.js

```jsx
import { useEffect, useRef } from "react";

function usePrevious(state) {
  let prevCountRef = useRef(0);

  useEffect(() => {
    console.log(1);
    prevCountRef.current = state;
  }, [state]);

  const prevCount = prevCountRef.current;

  return prevCount;
}

export default usePrevious;
```

<br>

PreviousCount.jsx

```jsx
import React, { useEffect, useRef, useState } from "react";
import usePrevious from "../custom Hook/usePrevious";

function PreviousCount(props) {
  const [count, setCount] = useState(0);
  const prevCount = usePrevious(count);
  console.log(2);
  return (
    <>
      <button onClick={increaseCount}>increase count</button>
      <div>count : {count}</div>
      <div>prevCount : {prevCount}</div>
    </>
  );

  function increaseCount() {
    setCount(count + 1);
  }
}

export default PreviousCount;
```

<br>

참고

- [https://ko.reactjs.org/docs/hooks-faq.html#how-to-get-the-previous-props-or-state](https://ko.reactjs.org/docs/hooks-faq.html#how-to-get-the-previous-props-or-state)

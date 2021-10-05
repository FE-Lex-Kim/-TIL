# custom Hook으로 container 컴포넌트 대체

<br>

## container component

- **전달한** state, dispatch 재사용 불가능
- presentaion component 자체를 **재사용 가능**

<br>

container / presentation component를 사용하는 이유는 컴포넌트를 재사용하는 목적이 아니라, 단지 로직을 분리하기 위함의 목적이다.('관심을 분산시키기 위함')

<br>

## custom hook

- **반환하는** state와 dispatch 재사용 가능
- presentaion component 자체에 **custom hook이 있어 재사용 어려움**

<br>

custom hook을 사용하는 이유는 컴포넌트안의 로직을 재사용하기 위한 목적이다.(물론 그안의 상태 로직을 분리할 수 있지만, 순수한 UI 컴포넌트를 재사용하기 어려움)

<br>

## 예제

### container

<br>

redux module

```jsx
import { createAction, handleActions } from "redux-actions";

// 액션 타입
const INCREASE_COUNT = "count/increase_count";
const DECREASE_COUNT = "count/decrease_count";

// 액션 생성함수

export const increase_count = createAction(INCREASE_COUNT);
export const decrease_count = createAction(DECREASE_COUNT);

// 초기값

const initialize = {
  count: 0,
};

// 리듀서

const count = handleActions(
  {
    [INCREASE_COUNT]: (state, action) => ({
      ...state,
      count: state.count + 1,
    }),
    [DECREASE_COUNT]: (state, action) => ({
      ...state,
      count: state.count === 0 ? 0 : state.count - 1,
    }),
  },
  initialize
);

export default count;
```

<br>

Container

```jsx
import React from "react";
import { useDispatch, useSelector } from "react-redux";
import Count from "../../Count";
import { decrease_count, increase_count } from "../../modules/count";

function CountContainer(props) {
  const num = useSelector((state) => state.count.count);
  const dispatch = useDispatch();

  return <Count num={num} increase={increase} decrease={decrease} />;

  function increase() {
    dispatch(increase_count());
  }

  function decrease() {
    dispatch(decrease_count());
  }
}

export default CountContainer;
```

<br>

component

```jsx
import React from "react";

function Count({ num, increase, decrease }) {
  return (
    <>
      <button onClick={increase}>increase</button>
      <button onClick={decrease}>decrease</button>
      <div>now count : {num}</div>
    </>
  );
}

export default Count;
```

<br>

위의 로직처럼 container 내부에서 dispatch와 state의 로직들을 store에서 가져와서 그대로 component에 props로 전달해준다.

<br>

container는 state에 대한 로직들을 관리하고

component는 오로지 UI를 관리한다.

<br>

이렇게 presentaion / container 컴포넌트 디렉토리 패턴은 상태 로직과 UI 로직을 분리시켜 놓아서 좀더 유지보수와 가독성을 높여준다.

<br>

위의 방식을 custom Hook으로 변경 시켜 보자

<br>

### custom Hook

<br>

component

```jsx
import React from "react";
import useCount from "./components/custom hook/useCount";

function Count() {
  const { num, increase, decrease } = useCount();

  return (
    <>
      <button onClick={increase}>increase</button>
      <button onClick={decrease}>decrease</button>
      <div>now count : {num}</div>
    </>
  );
}

export default Count;
```

<br>

custom Hook

```jsx
import { useDispatch, useSelector } from "react-redux";
import { decrease_count, increase_count } from "../../modules/count";

function useCount() {
  const num = useSelector((state) => state.count.count);
  const dispatch = useDispatch();

  function increase() {
    dispatch(increase_count());
  }
  function decrease() {
    dispatch(decrease_count());
  }

  return { num, increase, decrease };
}

export default useCount;
```

<br>

container 컴포넌트와 다르게 custom hook으로 변경시켜 놔서 해당 로직 자체를 다른 컴포넌트에도 재사용 가능하다.

<br>

하지만 count 컴포넌트 내부에 custom hook 자체를 넣어 놓았기 때문에 UI 자체로의 재사용은 불가능하다.

<br>

## 정리

- container component와 custom hook의 장점을 동시에 사용하면됨
- presentaion component를 **재사용 할 경우**는 container component 사용
- presentaion component를 **재사용 하지 않으면** custom hook 사용

두 방식 모두 장단점이 있으므로 한가지 패턴으로만 사용하지 않고 적절하게 두가지 방법을 조화롭게 사용하면 각 두가지 방법의 장점을 가져 갈 수 있을것이다.

<br>

참고

- [https://yujonglee.com/socwithhooks.html](https://yujonglee.com/socwithhooks.html)
- [https://medium.com/humanscape-tech/슬슬-hooks로-이사-가셔야죠-34be22e2962f](https://medium.com/humanscape-tech/%EC%8A%AC%EC%8A%AC-hooks%EB%A1%9C-%EC%9D%B4%EC%82%AC-%EA%B0%80%EC%85%94%EC%95%BC%EC%A3%A0-34be22e2962f)
- [https://felixgerschau.com/react-hooks-separation-of-concerns/](https://felixgerschau.com/react-hooks-separation-of-concerns/)
- [https://www.youtube.com/watch?v=l6GTpKLWllQ](https://www.youtube.com/watch?v=l6GTpKLWllQ)

# Hooks

<br>

## 1. useState

<br>

```jsx
import React, { useState } from "react";

const Counter = () => {
  const [count, setCount] = useState(0);
  return (
    <div>
      <h1>현재 카운터 값은 {count} 입니다.</h1>
      <button
        onClick={() => {
          setCount(count + 1);
        }}
      >
        +1
      </button>
      <button
        onClick={() => {
          setCount(count - 1);
        }}
      >
        -1
      </button>
    </div>
  );
};

export default Counter;
```

<br>

import로 useState를 불러오고

디스트럭처링 할당으로 state를 선언한다.

```jsx
import React, { useState } from "react";
const [count, setCount] = useState(0);
```

<br>

useState()는 배열을 반환한다.

첫번째 원소는 상태의 값을 나타낸다.

useState함수에서 인수로 값을넣어 기본값을 설정해준다.

<br>

두번째 원소는 원소의 상태값을 설정해주는 함수이다.

이함수의 파라미터에 값을 넣어주면 상태값이 변하고 **컴포넌트가 리렌더링이 된다.**

<br>

useState 함수는 하나의 상태값만 관리하게 된다.

따라서 여러개의 상태가 있고 관리를 하려면 useState를 여러번 사용해야한다.

```jsx
import React, { useState } from "react";

const Counter = () => {
  const [name, setname] = useState("");
  const [nickname, setnickname] = useState("");
  return (
    <div>
      <input
        onChange={(e) => {
          setname(e.target.value);
        }}
      />
      <input
        onChange={(e) => {
          setnickname(e.target.value);
        }}
      />
      <b>이름: {name}</b>
      <b>닉네임: {nickname}</b>
    </div>
  );
};

export default Counter;
```

<br>

## 2. useReducer

<br>

리듀서는 현재 상태, 업데이트를 위해 필요한 정보를 담은 액션값을 전달받아 **새로운 상태를 반환한다.**

이때 새로운 상태(state)를 반환하지만, 불변성을 지켜주어야 한다.

```jsx
function reducer(state, action) {
  return { ... }; 
}
```

<br>

액션값은 보통 객체로 이루어져있다.

type은 꼭 리덕스에서는 있어야한다.

<br>

useReducer에서 객체형태로가 아니라 다른 타입이여도 상관없다.

```jsx
import React, { useReducer } from "react";

const reducer = (state, action) => {
  switch (action.type) {
    case "Increment":
      return { value: state.value + 1 };
    case "Decrement":
      return { value: state.value - 1 };
    default:
      return state;
  }
};

const Counter = () => {
  const [state, dispatch] = useReducer(reducer, { value: 0 });

  return (
    <div>
      <h1>현재 카운터 값은 {state.value}입니다.</h1>
      <button onClick={() => dispatch({ type: "Increment" })}>+1</button>
      <button onClick={() => dispatch({ type: "Decrement" })}>-1</button>
    </div>
  );
};

export default Counter;
```

<br>

useReducer 의

첫번째 인수로 리듀서 함수를 넣고

두번째 인수로 해당 리듀서의 상태 초기값을 넣어준다.

<br>

useReducer에서 

첫번째 원소는 현재 state의 상태를 나타내고

두번째 원소는 redcuer 함수를 호출하고 그안의 인수에 액션의 타입을 결정해주는 함수이다. ex) dispatch(action)

<br>

**장점**

컴포넌트 업데이트 로직을 그 컴포넌트 안에서 작업하지 않고

바깥 외부에 빼낼수있다.

<br>

### 여러개의 태그에 하나의 이벤트를 발생할때 state관리

<br>

기존에는 똑같은 이벤트를 발생하는 태그가 여러개일때 이벤트 함수를 재활용하기 위해 

useState를 여러번 사용했다.

<br>

useReducer를 사용하면 기존 클래스형 컴포넌트에서 태그의 props 값에따라 이벤트를 할당하듯이 비슷한 방법으로 작업할수있다.

예제) 이전의 input 태그에서 onChange의 함수가 재활용할 수 없었다.

```jsx
import React, { useState } from "react";

const Counter = () => {
  const [name, setname] = useState("");
  const [nickname, setnickname] = useState("");
  return (
    <div>
      <input
        onChange={(e) => {
          setname(e.target.value);
        }}
      />
      <input
        onChange={(e) => {
          setnickname(e.target.value);
        }}
      />
      <b>이름: {name}</b>
      <b>닉네임: {nickname}</b>
    </div>
  );
};

export default Counter;
```

<br>

useReducer를 사용한후 재활용한 코드

```jsx
import React, { useReducer } from "react";

const reducer = (state, action) => {
  return {
    ...state,
    [action.name]: action.value,
  };
};

const Counter = () => {
  const [nameState, setNames] = useReducer(reducer, {
    name: "",
    nickname: "",
  });

  const inputOnChange = (e) => {
    setNames(e.target);
  };

  const { name, nickName } = nameState;
  return (
    <div>
      <input name="name" value={name} onChange={inputOnChange} />
      <input name="nickName" value={nickName} onChange={inputOnChange} />
      <b>이름: {name}</b>
      <b>닉네임: {nickName}</b>
    </div>
  );
};

export default Counter;
```

<br>

## 3. useMemo

[useMemo 내용 정리 링크](https://github.com/Alex-Eojin/-TIL/blob/master/React/useMemo.md)

## 4. useCallback

<br>

useMemo와 마찬가지로 렌더링 성능을 최적화해야 하는 상황에서 사용한다.

만약 컴포넌트가 리렌더링 될때마다 컴포넌트안에서 정의한 함수가 새로 만들어지게 된다.

이러한 경우를 최적화 해준다.

<br>

useCallback의 첫번쨰 파라미터에 생성하고싶은 함수를 넣는다.

두번째 파라미터에는 배열을 넣으면된다.

배열에는 안의 값의 변화에 따라 함수가 생성된다.

만약 비어있게 된다면 처음랜더링 될때만 함수를 생성한다.

즉, 처음에 랜더링될때 만들어진 함수를 계속해서 재사용한다.

<br>

함수 내부에서 상태값에 의존해야할 경우 반드시 두번째 파라미터 안에 포함해주어야 한다.

```jsx
import React, { useCallback, useMemo, useState } from "react";

const avg = (list) => {
  if (list.length === 0) return;
  const sum = list.reduce((a, b) => a + b);
  return sum / list.length;
};

const Average = () => {
  const [value, setValue] = useState("");
  const [list, setList] = useState([]);

  const memoAvg = useMemo(() => avg(list), [list]);

  const onChange = useCallback((e) => {
    setValue(e.target.value);
  }, []);

  const onClick = useCallback(() => {
    setList([...list, +value]);
    setValue("");
  }, [list, value]);
  return (
    <div>
      <input value={value} onChange={onChange} />
      <button onClick={onClick}>등록</button>
      <ul>
        {list.map((todo, index) => (
          <li key={index}>{todo}</li>
        ))}
      </ul>
      <div>
        <b>평균값 : {memoAvg}</b>
      </div>
    </div>
  );
};

export default Average;
```

<br>

## 5. useRef

<br>

useRef는 함수형 컴포넌트에서 ref를 쉽게 사용할수있도록 해준다.

useRef를 사용하여 ref를 설정하면 useRef를 통해 만든 객체안의 current값이 설정한 태그 엘리먼트를 가리킨다.

```jsx
import React, { useCallback, useMemo, useRef, useState } from "react";

const avg = (list) => {
  if (list.length === 0) return;
  const sum = list.reduce((a, b) => a + b);
  return sum / list.length;
};

const Average = () => {
  const inputRef = useRef(null);
  const [value, setValue] = useState("");
  const [list, setList] = useState([]);

  const memoAvg = useMemo(() => avg(list), [list]);

  const onChange = useCallback((e) => {
    setValue(e.target.value);
  }, []);

  const onClick = useCallback(() => {
    setList([...list, +value]);
    setValue("");
    inputRef.current.focus();
  }, [list, value]);
  return (
    <div>
      <input ref={inputRef} value={value} onChange={onChange} />
      <button onClick={onClick}>등록</button>
      <ul>
        {list.map((todo, index) => (
          <li key={index}>{todo}</li>
        ))}
      </ul>
      <div>
        <b>평균값 : {memoAvg}</b>
      </div>
    </div>
  );
};

export default Average;
```

## 6. useEffect

[useEffect 내용정리 챕터](https://github.com/Alex-Eojin/-TIL/blob/master/React/useEffect.md)
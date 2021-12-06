- [React 성능 최적화](#react-성능-최적화)
  - [체크리스트](#체크리스트)
  - [useEffect 내부에 함수 선언](#useeffect-내부에-함수-선언)
  - [컴포넌트 외부에서 받아온 함수를 useEffect에서 사용할때](#컴포넌트-외부에서-받아온-함수를-useeffect에서-사용할때)
  - [effect 내부에서 지속적으로 자주 state가 변경하는 경우](#effect-내부에서-지속적으로-자주-state가-변경하는-경우)
  - [useMemo](#usememo)
  - [useState 고비용 초기값 성능 최적화](#usestate-고비용-초기값-성능-최적화)

# React 성능 최적화

<br>

## 체크리스트

- [ ] useEffect **내부에 함수 선언**
- [ ] props로 받아온 **함수를** **useEffect의 의존성 배열에 넣기**.(해당 부모 컴포넌트도 useCallback 사용)
- [ ] effect 내부에서 **지속적으로 state가 변경하는 경우**(prev state 사용)
- [ ] **고비용 계산 최적화**(useMemo)
- [ ] **useState 초기값 고비용** 계산되는지

<br>

## useEffect 내부에 함수 선언

<br>

`useEffect`에서 함수를 호출한다면 해당 함수 내부의 에서 state 또는 props를 참조하는 로직이 들어간다면 문제가 된다.

<br>

`useEffect`의 의존성 배열안에 해당 state 또는 props을 넣어주어서 최적화 작업을 진행하기 까다롭다.

아래의 예제를 보자

```jsx
function Example({ someProp }) {
  function doSomething() {
    console.log(someProp);
  }

  useEffect(() => {
    doSomething();
  }, []);
}
```

<br>

`doSomething`이 `useEffect` 내부에서 호출 되었지만 그 **내부의 로직이 보이지 않아서 `useEffect`의 의존성 배열이 무엇이 들어가는지 확인하기 어렵다.**(vscode에서도 노란색 줄이 뜨는 경고가 안뜸)

<br>

그래서 일반적으로 `useEffect` 내부에서 함수를 선언는 이유이다. 그러면 effect가 되는 state 또는 props를 확인하기 쉽기 때문이다.

만약 함수 내부의 로직중에서 특별한 state 또는 props에 의해 호출되어야 하는 상황이 없으면 `[]` 을 넣어준다.

```jsx
function Example({ someProp }) {
  useEffect(() => {
    function doSomething() {
      console.log(someProp);
    }

    doSomething();
  }, [someProp]);
}
```

<br>

## 컴포넌트 외부에서 받아온 함수를 useEffect에서 사용할때

<br>

**다른 컴포넌트에서 함수를 받아와(부모 컴포넌트) useEffect에서 호출할때 어떻게 의존성 배열에 넣어야할까?**

<br>

다른 컴포넌트에서 함수를 정의할때, **애초부터 useCallback을 사용해서 해당 함수의 최적화 작업을 해준다.**

- 리-렌더링시, **외부에서 불러온 함수의 참조값이 변경된다.**
- 따라서 참조값이 변경되지 않게하기 위해서, **외부함수가 정의된곳에 useCallback을 사용해준다.**

현재 컴포넌트에서 받아온 **함수 자체를 의존성 배열에 넣어준다.**

<br>

아래의 예제를 보면 쉽게 이해가간다.

```jsx
function ProductPage({ productId }) {
  // 모든 렌더링에서 변경되지 않도록 useCallback으로 래핑
  const fetchProduct = useCallback(() => {
    // ... productId로 무언가를 한다. ...
  }, [productId]); // 모든 useCallback 종속성이 지정된다.

  return <ProductDetails fetchProduct={fetchProduct} />;
}

---function ProductDetails({ fetchProduct }) {
  useEffect(() => {
    fetchProduct();
  }, [fetchProduct]); // 모든 useEffect 종속성이 지정된다.
  // ...
};
```

<br>

## effect 내부에서 지속적으로 자주 state가 변경하는 경우

<br>

```jsx
import React, { useEffect, useState } from "react";

function IntervalCount(props) {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const id = setInterval(() => {
      setCount(count + 1); // 이 effect는 'count' state에 따라 다릅니다
    }, 1000);
    console.log("effect");
    return () => {
      console.log("clear");
      return clearInterval(id);
    };
  }, []); // 🔴 버그: `count`가 종속성으로 지정되지 않았습니다

  return <h1>{count}</h1>;
}

export default IntervalCount;
```

<br>

위와 같이 count가 1초 간격으로 증가하는 `setCount`가 있다.

하지만 1초 간격으로 호출된다고 해도, 이미 `setCount`의 스코프는 클로저로 `count` 값이 `0`으로 설정되어 계속 `setCount(0 + 1)`를 호출하므로 카운트가 1이 된다.

<br>

```jsx
import React, { useEffect, useState } from "react";

function IntervalCount(props) {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const id = setInterval(() => {
      setCount(count + 1); // 이 effect는 'count' state에 따라 다릅니다
    }, 1000);
    console.log("effect");
    return () => {
      console.log("clear");
      return clearInterval(id);
    };
  }, [count]); // 🔴 버그: `count`가 종속성으로 지정되지 않았습니다

  return <h1>{count}</h1>;
}

export default IntervalCount;
```

의존성 배열안에 [count]를 넣는다고 하면, count가 변경될때마다 useEffect가 다시 호출되므로 clean up도 다시 호출되어서 clearInterval이 호출된다.

<br>

이것은 의미없는 clearInterval이 1초뒤에 지속적으로 호출되므로 옳바르지 않은 로직이다.

<br>

![React 성능 최적화](../Images/React%20성능%20최적화/React%20성능%20최적화-1.png)

<br>

```jsx
import React, { useEffect, useState } from "react";

function IntervalCount(props) {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const id = setInterval(() => {
      setCount((prevCount) => prevCount + 1);
    }, 1000);
    console.log("effect");
    return () => {
      console.log("clear");
      return clearInterval(id);
    };
  }, []);

  return <h1>{count}</h1>;
}

export default IntervalCount;
```

그래서 위의 setCount를 콜백함수를 넣어서, 현재 state 참조 하지 않고 이전 state 값을 참조해서 증가하게 할 수 있다.

<br>

그리고 이제는 count state 값에 의존하지 않기 때문에, 의존성 배열값에 count를 넣지 않아도 된다는 점이 있다.

<br>

## useMemo

<br>

useMemo를 사용하면 함수형 컴포넌트 내부에서 발생하는 연산을 **최적화** 할 수 있다.

랜더링 과정에서 **특정 값이 바뀌었을때만 연산을 실행하고, 원하는 값이 바뀌지 않았으면**

**이전에 연산한 결과를 다시 사용한다.**

```jsx
const getAverage = (numbers) => {
  console.log("평균값 계산중..");
  if (numbers.length === 0) return 0;
  const sum = numbers.reduce((a, b) => a + b);
  return sum / numbers.length;
};

const Average = () => {
  const [list, setList] = useState([]);
  const [number, setNumber] = useState("");

  const onChange = (e) => {
    setNumber(e.target.value);
  };

  const onInsert = (e) => {
    const nextList = list.concat(parseInt(number));
    setList(nextList);
    setNumber("");
  };

  const avg = useMemo(() => getAverage(list), [list]);

  return (
    <div>
      <input value={number} onChange={onChange} />
      <button onClick={onInsert}>등록</button>
      <ul>
        {list.map((value, index) => (
          <li key={index}>{value}</li>
        ))}
      </ul>
      <div>
        <b>평균 값:</b> {avg}
      </div>
    </div>
  );
};
```

<br>

useMemo Hook로 getAverage(list) 함수가 **list값이 변경될 경우에만 연산을 실행한다.**

만약 list의 값이 변경하지 않았다면 이전에 연산했던 결과를 다시 사용하여 리렌더링이 될때마다 재사용한다.

<br>

## useState 고비용 초기값 성능 최적화

<br>

useState의 초기값 자체가 어떠한 로직을 통해서 고비용 든다면 문제가 된다.

<br>

해당 초기값이 함수의 호출로 생성되고, 이전의 useState의 값이 있으면 최초 렌더링이 아니라고 판단해 이전의 state 값을 사용하는 로직 방식이라, **매번 리렌더링이 될때마다 호출이되어** **계산이 되어서 고비용이 발생한다.**

<br>

```jsx
import React, { useState } from "react";

function InitUseState(props) {
  function initCount(a, b) {
    console.log("callback");
    return a ** b;
  }

  const [count, setCount] = useState(() => initCount(10, 10));
  return (
    <>
      <h1>{count}</h1>
      <button onClick={onClick}>Increase count</button>
    </>
  );

  function onClick(params) {
    setCount(count + 1);
  }
}

export default InitUseState;
```

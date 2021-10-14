# useMemo

<br>

useMemo를 사용하면 함수형 컴포넌트 내부에서 발생하는 연산을 **최적화** 할 수 있다.

랜더링 과정에서 **특정 값이 바뀌었을때만 연산을 실행하고, 원하는 값이 바뀌지 않았으면**

**이전에 연산한 결과를 다시 사용한다.**

<br>

## 사용법

useMemo()함수안에

첫번째 인수로 특정한 값이 변경할때만 실행할 작업을 넣어준다.

두번째 인수로 특정값이 어떤 것 일지 배열로 넣어서 정해주어야 한다.

<br>

useMemo Hook은 **메모이제이션** 된 값을 반환한다.

<br>

### **메모이제이션 이란?**

동일한 계산을 반복적으로 해야 할때, 그값을 메모리에 저장함으로써

**동일한 계산의 반복수행을 제거하여** 빠르게 하는 기술이다.

<br>

**주의!**

**useMemo로 전달된 콜백함수는 랜더링 중에 실행된다.**

만약 **렌더링 중에 하지 않는 것을 이 함수내에서 사용하면 안된다**.

그런경우 useEffect Hook 에서 해야하는 작업이다!

<br>

## 예제

<br>

useMemo를 적용하기전 예제코드)

```jsx
import React, { useState } from "react";

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
        <b>평균 값:</b> {getAverage(list)}
      </div>
    </div>
  );
};

export default Average;
```

<br>

input태그안에 내용이 수정될때마다 Average 컴포넌트가 리렌더링이 된다.

따라서 getAverage(list)도 계속 평균값을 연산한다.

<br>

하지만 렌더링될때마다 평균값을 연산할 필요가 없다.

<br>

useMemo를 적용한 예제코드)

```jsx
import React, { useMemo, useState } from "react";

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

export default Average;
```

<br>

useMemo Hook로 getAverage(list) 함수가 **list값이 변경될 경우에만 연산을 실행한다.**

만약 list의 값이 변경하지 않았다면 이전에 연산했던 결과를 다시 사용하여 리렌더링이 될때마다 재사용한다.

<br>

## 정리

메모이제이션된 값은 getAverage(list) 이 된다.

<br>

**렌더링중에 의존성이 변경되었을때**

2-1 . getAverage(list)인 메모이제이션 값만 다시 계산한다.

<br>

**렌더링중에 의존성이 변경되지 않았을때**

2-2 . getAverage(list)인 메모이제이션 값을 재사용한다.

<br>

## React.memo vs useMemo

### React.memo

parent component

```jsx
import React from "react";

function ReactMemoCount(props) {
  console.log("child Component");
  return (
    <>
      <h1>I'm child Component</h1>
    </>
  );
}

export default React.memo(ReactMemoCount);
```

<br>

child component

```jsx
import React from "react";

function ReactMemoCount(props) {
  console.log("child Component");
  return (
    <>
      <h1>I'm child Component</h1>
    </>
  );
}

export default ReactMemoCount;
```

<br>

부모 컴포넌트가 호출되면 자식 컴포넌트도 따라서 리렌더링 된다.

이때 자식 컴포넌트에게 React.memo를 사용하면 자식 컴포넌트의 props의 변경이 없으면 리렌더링 되지 않

<br>

### useMemo

<br>

parent component

```jsx
import React, { useMemo, useState } from "react";
import ReactMemoCount from "./ReactMemoCount";

function ReactMemoConuntContainer(props) {
  const [count, setCount] = useState(0);
  const [str, setStr] = useState("hi ");

  const MemoCountComponent = useMemo(() => <ReactMemoCount str={str} />, [str]);

  return (
    <>
      {MemoCountComponent}
      <button onClick={increaseCount}>Increase count</button>
      <button onClick={addStrAlex}>add str Alex</button>
      <h2>curCount : {count}</h2>
    </>
  );

  function increaseCount(params) {
    setCount(count + 1);
  }

  function addStrAlex(params) {
    setStr(str + "Alex ");
  }
}

export default ReactMemoConuntContainer;
```

<br>

child component

```jsx
import React from "react";

function ReactMemoCount({ str }) {
  console.log("child Component");
  return (
    <>
      <h1>I'm child Component</h1>
      <h2>curStr : {str}</h2>
    </>
  );
}

export default ReactMemoCount;
```

<br>

parent componet에서 useMemo를 사용해서,

자식 컴포넌트를 str이 변경되지 않으면 리렌더링이 되지않게 했다.

<br>

### 공통점

React.memo 와 useMemo의 공통점은 자식 컴포넌트가 특정한 props가 변경되지 않으면 리렌더링 되지않게 성능을 향상 시킬 수 있다.

<br>

### 차이점

<br>

React.memo

1. React.memo는 HOC 이여서 클래스 컴포넌트, 함수형 컴포넌트 두가지 모두 사용할 수 있다는 점이 있다.
2. React.memo는 성능 최적화를 하려는 컴포넌트에서 로직을 작성한다는 점이다.(이 점을 말하는 이유는 useMemo는 그렇지 않아서)

<br>

useMemo

1. useMemo는 함수형 컴포넌트에서만 사용가능하다.
2. useMemo는 성능 최적화를 부모 컴포넌트에서 로직을 작성해야한다는 점이다.
   - 따라서 정작 성능 최적화를 하려는 컴포넌트에서 해당 로직을 확인 하지 못해 가독성, 유지보수가 좋지 않다는 점이 있다.

<br>

개인적인 의견으로는 아무래도 React.memo가 훨씬 좋다고 생각이든다.

최적화 하는 컴포넌트 내부에서 로직을 작성하다 보니 가독성과 유지보수가 높다는 점을 무시할 수 없는것 같다.

그리고 useMemo는 로직 자체가 길어진다는 점에서도 불편하고 컴포넌트가 컴포넌트 처럼 보이지않는다는 단점이 있어보인다.

<br>

결론은 컴포넌트 성능 최적화를 하려면 React.memo를 사용하고

useMemo는 컴포넌트 성능 최적화 보다 함수 중복 계산을 최적화 하는데 사용하는게 좋아 보인다.

<br>

참고:

[리액트 공식문서](https://ko.reactjs.org/docs/hooks-reference.html#usememo)

책 리액트를 다루는 기술

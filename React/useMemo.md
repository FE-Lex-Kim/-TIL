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

2-1 .  getAverage(list)인 메모이제이션 값만 다시 계산한다.

<br>

**렌더링중에 의존성이 변경되지 않았을때**

2-2 . getAverage(list)인 메모이제이션 값을 재사용한다.

<br>

## useEffect vs useMemo 비교

<br>

**useEffect**

**렌더링이 된후 DOM작업이 완료되고** 난 이후에 의존성이 변경된 경우 작업을 한다.

<br>

**useMemo**

**랜더링중에** 의존성이 변경된경우 작업을 실행한다.

<br>

[eslint-plugin-react-hooks](https://www.npmjs.com/package/eslint-plugin-react-hooks#installation) 패키지로 의존성이 바르게 정의 되지 않으면 그에 대해 경고로 수정하도록 알려준다.

<br>

참고: 

[리액트 공식문서](https://ko.reactjs.org/docs/hooks-reference.html#usememo)

책 리액트를 다루는 기술
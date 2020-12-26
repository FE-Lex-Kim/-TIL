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

## 2. useEffect

<br>

useEffect는 **컴포넌트가 렌더링 될때** 마다 특정 작업을 수행하도록 설정해준다.

클래스형 컴포넌트에서 componenetDidMount와 componenetDidUpdate와 합친형태와 비슷하다.

컴포넌트가 렌더링이 된후 실행된다.

```jsx
import React, { useEffect, useState } from "react";

const Counter = () => {
  const [name, setname] = useState("");
  const [nickname, setnickname] = useState("");
  useEffect(() => {
    console.log("렌더링이 완료되었습니다.");
    console.log({
      name,
      nickname,
    });
  });
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

### 마운트된 경우만 실행할때

<br>

useEffect는 **마운트** 와 **업데이트** 가 될때 실행된다.

마운트가 됬을 때만 실행하려면 useEffect 함수의 두번째 인수에 빈배열을 넣으면된다.

<br>

```jsx
useEffect(() => {
    console.log("마운트가 완료되었습니다.");
  }, []);
```

<br>

처음 컴포넌트가 생긴 경우에만 호출되고

그 이후에 업데이트 되어도 실행되지 않는다.

<br>

### 특정값이 업데이트 될때만 실행할때

<br>

useEffect를 사용할 때, 특정값만 변경될 때 호출하고 싶다면

두번째 인수에 검사하고 싶은 값을 넣어주면 된다.

```jsx
useEffect(() => {
    console.log(name);
  }, [name]);
```

<br>

name state가 변경될 때만 useEffect가 실행된다.

<br>

useState로 관리하는 state를 넣어주어도 되고

또는

props로 전달받은 값을 넣어 주어도 된다.

<br>

### 언마운트 업데이트 직전 작업 처리시 실행

<br>

useEffect는 렌더링이되고 난후 실행된다.

두번째 파라미터는 조건에따라 실행된다.

<br>

만약 컴포넌트가 **언마운트** 나 **업데이트 되기전** 에 호출하고 싶다면 useEffect에서 cleenup(뒷정리)함수를 반환해주어야한다.

```jsx
useEffect(() => {
    console.log("effect");
    console.log(name);
    return () => {
      console.log("cleenUp");
      console.log(name);
    };
  }, [name]);
```

<br>

오직 언마운트될 때만 호출하고 싶다면 useEffect 함수의 두번째 파라미터에 빈배열을 넣으면된다.

<br>

상태가 바뀌면 업데이트가 되고

클래스 컴포넌트에서는 렌더함수가 실행되는데 

함수 컴포넌트에서는 컴포넌트 함수자체가 다시 실행된다.

<br>

그 이후 상태가 변경된 변수가 useState()에 의해 retrun 값으로 변경이된다.

그이후 함수형 컴포넌트의 return의 상태의 변경부분만 비교한뒤 브라우저의 DOM에 적용한다.

<br>

useEffect를 사용하면 함수형 컴포넌트의 실행순서가 헷갈린다.

정확히 어떤식으로 Lifecycle이 동작하는지 알아야한다.

<br>

예제)

```jsx
import React, { useState, useEffect } from 'react';
import styled from 'styled-components';
import NewsItem from './NewsItem';
import axios from 'axios';

const NewsListBlock = styled.div`
  box-sizing: border-box;
  padding-bottom: 3rem;
  width: 768px;
  margin: 0 auto;
  margin-top: 2rem;
  @media screen and (max-width: 768px) {
    width: 100%;
    padding-left: 1rem;
    padding-right: 1rem;
  }
`;

const NewsList = () => {
  const [articles, setArticles] = useState(null);
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    // async를 사용하는 함수 따로 선언
    const fetchData = async () => {
      try {
        console.log(1);
        setLoading(true);
        const response = await axios.get(
          'https://newsapi.org/v2/top-headlines?country=kr&apiKey=0a8c4202385d4ec1bb93b7e277b3c51f',
        );
        setArticles(response.data.articles);
        console.log(2);
      } catch (e) {
        console.log(3);
      }
      setLoading(false);
      console.log(4);
    };
    fetchData();
  }, []);

  // 대기 중일 때
  if (loading) {
    console.log(5);
    return <NewsListBlock>대기 중…</NewsListBlock>;
  }
  // 아직 articles 값이 설정되지 않았을 때
  if (!articles) {
    console.log(6);
    return null;
  }

  console.log(7);
  // articles 값이 유효할 때
  return (
    <NewsListBlock>
      {articles.map((article) => (
        <NewsItem key={article.url} article={article} />
      ))}
    </NewsListBlock>
  );
};

export default NewsList;
```

<br>

**코드실행순서**

1 . 컴포넌트가 마운트 되고난후

articles가 falsy값이므로 `6`이 콘솔에 찍힌다.

<br>

2 . 이후 마운트가 된이후 useEffect가 실행된다.

그이유는 useEffect의 두번째 인자로 `[]` 빈배열을 주었기 때문이다.

<br>

3 . useEffect 함수안에서 try문의 첫번째 `1` 이 찍힌후 setLoading으로 상태값을 변경했으므로 함수 컴포넌트가 reRendering되어 if문안의 loadting이 trusy값이므로 콘솔에 `5`가 찍힌후 대기중이 랜더가 된다.

<br>

4. 다시axios가 실행된후, setArticles가 실행되어 상태가 변하므로 함수컴포넌트가 리랜더링 된후 loading은 아직도 true값이므로 다시 콘솔에 `5` 가 찍한다.

<br>

5. 이후에 다시 useEffect로 돌아와 콘솔에 `2`가 찍힌다.

<br>

6. try문이 종료 된후 setLoading이 false가 되므로 다시 함수 컴포넌트가 리랜더링 되어 loading if문을 지나 articles if문을 지나 콘솔에 `7` 이 찍히고 retrun문으로 JSX가 반환된다.

<br>

7. 그이후 다시 useEffect로 돌아와 콘솔에 4가 찍히고 마무리 된다.

<br>

## 3. useReducer

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

## 4. useMemo

<br>

useMemo를 사용하면 함수형 컴포넌트 내부에서 발생하는 연산을 최적화 할 수 있다.

랜더링 과정에서 특정 값이 바뀌었을때만 연산을 실행하고, 원하는 값이 바뀌지 않았으면

이전에 연산한 결과를 다시 사용한다.

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

useMemo()함수안에

첫번째 인수로 특정한 값이 변경할때만 실행할 함수를 넣어준다.

두번째 인수로 특정값이 어떤 것 일지 배열로 넣어서 정해주어야 한다.

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

## 5. useCallback

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

## 6. useRef

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
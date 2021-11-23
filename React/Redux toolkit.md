# Redux toolkit

**Redux Toolkit은 Redux 로직을 작성하기 위해 Redux가 공식적으로 추천하는 방법이다.**

<br>

Redux의 액션 타입, 액션 생성자 함수, 리듀서의 **로직이 복잡하거나 중복되는 경우들이 있다.**

그래서 예를들어

- **[redux-actions](https://github.com/redux-utilities/redux-actions) 라이브러리 사용**
- **[immer](https://immerjs.github.io/immer/) 라이브러리 사용**
- **[thunk middleware](https://github.com/reduxjs/redux-thunk) 사용**

등등 이 있다.

<br>

**Redux toolkit 은 복잡한 로직과 중복되는 코드를 개선한 Redux의 상위 버전이라고 생각하면된다.(당연히 선수지식인 [Redux](https://ko.redux.js.org/introduction/getting-started/)부터 공부해오자.)**

<br>

Redux toolkit에 대해 **간단하게** 사용하는 법에 대해 알아보자.

- 자세한 내용은 [**공식홈페이지**](https://redux-toolkit.js.org/introduction/getting-started) 추천

<br>

설치

```bash
npm install @reduxjs/toolkit
```

<br>

Redux에서 여러가지 편리한 라이브러리가 덧 붙여진 형태이기 때문에, 기본적으로는 Redux가 동작하는 프로세스는 동일하다.

<br>

![리덕스](../Images/리덕스/리덕스-1.gif)

<br>

## configureStore

store 생성하는 방법에 대해 알아보자.

(앞으로 Redux-toolkit은 원래 버전인 Redux와 비교해서 설명하겠다.)

<br>

**기존의 Redux 에서는 store 생성하기위해 createStore 함수를 호출했다.**

```jsx
import { Provider } from "react-redux";
import { createStore } from "redux";
import rootReducer from "./redux/rootReducer";

const store = createStore(rootReducer); // <-- 이 부분

ReactDOM.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById("root")
);
```

<br>

**Redux-toolkit에서는(이제부터 RTK라고 부르겠다.) configureStore을 사용한다.**

```jsx
import { Provider } from "react-redux";
import rootReducer from "./redux/rootReducer";
import { configureStore } from "@reduxjs/toolkit";

const reducer = {
  reducer: rootReducer,
};
const store = configureStore(reducer);

ReactDOM.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById("root")
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();
```

<br>

그렇다면 createStore와 다른점도 없는데 왜 사용하는 걸까?

configureStore에는 여러가지 유용한 옵션이 포함되어 있다.

<br>

### MiddleWare

**configureStore은 옵션으로 middleWare을 포함하고 있다.**

```jsx
const store = configureStore({
  reducer: rootReducer,
  middleware: [thunk, logger],
});
```

<br>

기본으로 포함되어있는 middleWare으로

- [Immutability Middleware](https://redux-toolkit.js.org/api/immutabilityMiddleware) : **state가 mutable하게 변경되면 error 던진다.**
- [Serializability Middleware](https://redux-toolkit.js.org/api/serializabilityMiddleware) : state, action이 **serialize(직렬화) 되면 console에 알려준다.**
  - **serialize(직렬화)란 타입이 다른 타입으로 변경되는 것을 말한다.**
  - **예를들어) localStorage에 값으로 string만 저장해야하므로 object을 json.stringify을 통해 object을 스트링화 한다. 이것을 직렬화라고 한다.**
  - **데이터 일관성**을 위해 넣은 middleWare이다.
- [react-thunk](https://github.com/reduxjs/redux-thunk) : 흔하게 잘 알고있는 비동기 처리 Thunk middleWare이다.

가 있다.

<br>

### devTools

**[Redux DevTools extension](https://github.com/zalmoxisus/redux-devtools-extension) 또한 기본 옵션으로 추가되어있다.**

```jsx
const store = configureStore({
  reducer: rootReducer,
  middleware: [thunk, logger],
  devTools: true,
});
```

<br>

true 값을 넣으면 DevTools Extension이 사용된다.

**default는 true 값이다.**

<br>

다른 옵션은 [RTX ConfigureStore](https://redux-toolkit.js.org/api/configureStore#preloadedstate) 에서 볼 수 있다.

<br>

### Provide

기존의 Redux와 같이 Provider을 통해 App 컴포넌트들이 모두 store을 사용할 수 있게 전달한다.

```jsx
import { Provider } from "react-redux"; // <--- 이 부분
import rootReducer from "./redux/rootReducer";
import { configureStore } from "@reduxjs/toolkit";

const reducer = {
	reducer: rootReducer,
};
const store = configureStore(reducer);

ReactDOM.render(
  <React.StrictMode>
    <Provider store={store}> // <--- 이 부분
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById("root")
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();
```

<br>

## createAction

기존 Redux에서는 **액션 타입과 액션 생성자 함수 각각 분리해서 만들었다.**

```jsx
// 액션 타입
const INCREASE = "INCREASE";
const DECREASE = "DECREASE";

// 액션 생성자 함수
export const increase = () => ({ type: INCRESE });
export const decrease = () => ({ type: DECRESE });
```

**액션 타입과 액션 생성자 함수를 모두 만들기 때문에 이름조차 중복되는 로직을 만든다.**

<br>

**RTK는 액션 타입과 액션 생성자 함수를 동시에 수행한다.**

```jsx
// 액션 생성자 함수
export const increase = createAction("INCREASE");
export const decrease = createAction("DECREASE");
```

<br>

**액션 생성자 함수를 만들면 자동으로 액션 타입을 선언한것과 똑같이 동작한다.**

<br>

Reducer에서 **액션 타입을 참조하는 방법은 두 가지가 있다.**

1. **`increase.type`**
2. **`increase.toString()`**

두 가지 모두 액션 생성자 함수에서 **이미 정의되어 있어 사용가능하다.**

```jsx
// 액션 생성자 함수
export const increase = createAction("INCREASE");
export const decrease = createAction("DECREASE");

increase.toString(); // --> 'INCREASE'
increase.type; // --> 'INCREASE'
```

<br>

### payload function

**payload을 customize 해서 다른 값으로 만들때는 createAction의 두번째 인수에 콜백함수로 전달하면된다.**

payload에 여러가지 값을 전달할때 쉽게 만든다.

```jsx
export const increase = createAction("INCREASE", (num) => {
  return {
    payload: {
      num,
      id: Math.random(),
      createdAt: new Date().toString(),
    },
  };
});
```

<br>

## createReducer

기존의 Redux에서 **리듀서를 만들때 switch을 사용했다.**

```jsx
// 리듀서 생성
const counter = (state = initialState, action) => {
  switch (action.type) {
    case INCREASE:
      return { number: state.number + 1 };
    case DECREASE:
      return { number: state.number - 1 };
    default:
      return state;
  }
};
```

Reducer을 만들때, **switch문의 단점은 보일러 플레이트 코드가 있어서 에러가 발생**하기 쉽다.

- `case..`
- `return..`
- `default`를 마지막에 꼭 넣어주어야 하는데 잊어먹기 쉬움
- **initialState** 를 넣는 것도 잊어먹기 쉬움

<br>

**RTX에서는 createReducer을 사용하면된다.**

createReducer 에는 **두가지 인수를 가진다.**

1. **initialState**
2. 액션 타입을 핸들링 하는 **lookup table(key, value로 구성되어 가독성이 좋게 만들어진 테이블)**

```jsx
// 액션 생성자 함수
export const increase = createAction("INCREASE");
export const decrease = createAction("DECREASE");

// 리듀서 생성
const counterReducer = createReducer(
  { number: 0 },
  {
    [increase.type]: (state) => ({ number: state.number + 1 }),
    [decrease.type]: (state) => ({ number: state.number - 1 }),
  }
);
```

<br>

**액션 생성자 함수의 타입으로 구분한다.**

**하지만 자동으로 `.toString()`을 호출하기 떄문에 `.type`을 호출하지 않아도 된다.**

```jsx
const counterReducer = createReducer(
  { number: 0 },
  {
    [increase]: (state) => ({ number: state.number + 1 }),
    [decrease]: (state) => ({ number: state.number - 1 }),
  }
);
```

<br>

### mutable state

**React는 Reducer을 순수한 함수로 유지하기 원하고 immutable한 state 변경을 지향한다.**

하지만 immutable 한 state 변경 때문에 로직이 복잡해지는 경우가 있다.

```jsx
const addTodo = createAction('todos/add')
const toggleTodo = createAction('todos/toggle')

const todosReducer = createReducer([], {
  [addTodo]: (state, action) => {
    const todo = action.payload
    return [...state, todo]
  },
  [toggleTodo]: (state, action) => {
    const index = action.payload
    const todo = state[index]
    return [
      ...state.slice(0, index),
      { ...todo, completed: !todo.completed }
      ...state.slice(index + 1)
    ]
  }
})
```

<br>

toggleTodo 액션은 단지 **중간의 state을 변경하려고** 하지만 굉장히 복잡한 로직이 만들어진다.

<br>

**RTK에서는 immer libary가 기본으로 포함되어있다.**

**state 자체를 변경하여 보다 가독성 높고 쉬운 로직으로 작업 가능하다.**

```jsx
const addTodo = createAction("todos/add");
const toggleTodo = createAction("todos/toggle");

const todosReducer = createReducer([], {
  [addTodo]: (state, action) => {
    const todo = action.payload;
    state.push(todo);
  },
  [toggleTodo]: (state, action) => {
    const index = action.payload;
    const todo = state[index];
    todo.completed = !todo.completed;
  },
});
```

<br>

**※ 주의! : 하나의 리듀서에서 immutable 또는 mutable 둘중 하나는 일관성 있게 사용해야한다.**

- 예를들어) **addTodo는 mutable하게 return을 하고 toggleTodo는 immutable 하게 state을 변경한다면 에러가 발생한다.**

<br>

※ 사실 createReducer에서 **action을 핸들링 하는 방법으로** **"Builder Callback" 방법을 RTK에서는 추천한다.**

- 하지만 **TypeScript을 사용했을때, 케미가 좋기 때문에 추천하는 것**이므로 자세한 내용은 **[RTK Builder Callback 공홈](https://redux-toolkit.js.org/api/createReducer#usage-with-the-builder-callback-notation)** 를 추천한다.

<br>

## createSlice

**RTK에서 Redux-logic을 만들때, 굉장히 강추하고 있는 방법이다.**

<br>

**createSlice을 통해 모든 redux 보일러 플레이트 코드를 없애준다.**

createSlice 한번의 호출로

- **initialState**
- **액션 타입**
- **액션 생성자 함수**
- **리듀서 함수**

코드를 하나로 통합시켜준다.

<br>

createReducer와 비교해보자.

```jsx
// 액션 생성자 함수
export const increase = createAction("INCREASE");
export const decrease = createAction("DECREASE");

// 리듀서 생성
const counterReducer = createReducer(
  { number: 0 },
  {
    [increase.type]: (state) => ({ number: state.number + 1 }),
    [decrease.type]: (state) => ({ number: state.number - 1 }),
  }
);
```

<br>

createSlice

```jsx
import { createSlice } from "@reduxjs/toolkit";

const initialState = { number: 0 };

const counterSlice = createSlice({
  name: "counter",
  initialState,
  reducers: {
    increase: (state) => {
      state.number += 1;
    },
    decrease: (state) => {
      state.number -= 1;
    },
  },
});

export const { increase, decrease } = counterSlice.actions;
export default counterSlice.reducer;
```

<br>

initialState을 createSlice에서 정의한다.

<br>

reducer 마찬가지로 내부에서 정의한다.

<br>

**액션 타입은 createSlice name과 reducer key 가 조합되어 네이밍 된다.**

- 예를 들어) 위의 예제에서는 **액션타입이** **counter/increase, counter/decrease 가 된다.**

<br>

**액션 생성자 함수가 리듀서의 key string 이름으로 자동으로 생성되어진다.**

**액션 생성자 함수를 createSlice.actions에서 가져와서 export 해준다.**

- 위의 예제에서)

```jsx
export const { increase, decrease } = counterSlice.actions;
```

<br>

createAction, createReducer을 합친게 createSlice 이다보니, createSlice만 사용하면 될것 같다는 생각이든다.

<br>

간단하게 redux-toolkit에 대해 알아보았다.

더 자세한 옵션들과 추가적인 기능들은 [**redux-toolkit 공식 홈페이지**](https://redux-toolkit.js.org/introduction/getting-started)를 추천한다.

<br>

참고

- [https://redux-toolkit.js.org/](https://redux-toolkit.js.org/)
- [https://ko.redux.js.org/introduction/getting-started/](https://ko.redux.js.org/introduction/getting-started/)
- [https://ymc-crow.github.io/basic/redux와-serializable/](https://ymc-crow.github.io/basic/redux%EC%99%80-serializable/)
- [https://www.youtube.com/watch?v=NpNqPqShX0c](https://www.youtube.com/watch?v=NpNqPqShX0c)

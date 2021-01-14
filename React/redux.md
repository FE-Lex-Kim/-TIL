# redux

<br>

```bash
npm i redux
```

<br>

Context API가 개선되기 이전에는 사용방식이 매우 불편하여 리덕스를 사용해 전역 상태를 관리하였다.

<br>

단순히 전역상태만 관리하면 Context API를 사용하는 것 만으로도 충분하다.

<br>

리억스는 상태관리를 더욱 체계적으로 관리하고 프로젝트 규모가 클경우 리덕스를 사용하는 편이 좋다.

<br>

또한 미들웨어라는 기능을 제공하여 비동지 처리도 수월하게 할 수 있다.

<br>

**장점**

1 . 코드 유지보수성 높아짐

2 . 작업 효율 극대화

3 . 비동기 처리 수월하다.

<br>

## 액션

상태 변화가 일어나면 액션이라는 것이 발생한다.

액션은 하나의 객체로 표현한다.

```jsx
{
type : 'TOGGLE_VALUE'
}
```

<br>

액션에 type값을 가져 그 액션이 처리할 일에대한 이름을 달아주어서

액션에 따라 처리할 행동이 달라진다.

type의 값은 문자열인 대문자로 만들어야한다(컨벤션).

<br>

하지만 문자열로 만들다보니 에러도 찾기 힘들기때문에

type의 값을 문자열을 따로 변수에 할당해서 넣어주는 작업을 하면

나중에 참조에 대한 에러가 날 수 있으므로 좀더 엄격하게 관리할 수 있다.

<br>

그외 에 값들은 상태를 업데이트를 할때 사용하는 값을 넣어주면된다.

```jsx
{
type : 'TOGGLE_VALUE'
data : {
	id: 1,
	text: '리덕스 배우기'
	}
}
```

<br>

## 액션 생성 함수

<br>

액션을 객체 리터럴로 그대로 만들어 주어도 되지만 재사용성과 

반복되는 작업으로 인한 실수 및 유지 보수를 쉽게 함수로써 액션을 만든다.

```jsx
function addTodo(data) {
	return{
		type: 'ADD_TODO',
		data
	}
}
화살표 함수로 만들어도 된다.

cosnt addTodo = (data) => ({
	type: 'ADD_TODO',
	data
})
```

<br>

## 리듀서

<br>

액션을 통해서 그액션에 대한 내용으로 새로운 상태를 반환해 만들어준다.

리듀서는 현재 상태와 액션 객체를 파라미터로 받아온다.

```jsx
const action = {
  type: 'TOGGLE_VALUE',
  data: {
    id: 1,
    text: '리덕스 배우기',
  },
};

const initialState = {
  counter: 1,
};

function reducer(state = initialState, action) {
  switch (action.type) {
    case INCREMENT:
      return {
        counter: state.counter + 1,
      };
    default:
      return state;
  }
}
```

<br>

action을 받아와 그에따른 type으로 처리해야할 내용을 switch문으로 만든다.

반환하는 값은 기존에 사용하지 않은 불변성을 지키기위한 새로운 객체 상태값을 반환한다.

<br>

## 스토어

<br>

리덕스를 적용하기 위해서는 스토어가 필요하다.

스토어는 그안에 내장함수가 있어서 상태에대한 여러가지 작동을 하게 해준다.

스토어 안에는 현재 상태와 리듀서가 들어가 있으며 프로젝트 하나에는 단 하나의 스토어만 가질 수 있다.

<br>

## 디스패치

<br>

디스패치는 위의 스토어의 내장함수중 하나이다.

디스패치는 액션을 만든뒤 그 액션을 발생시키는 것이다.

**액션을 발생시킨다** 는 의미는 리듀서를 실행시켜 새로운 상태를 만든다는 의미이다.

```jsx
dispatch(action)
```

<br>

파라미터 값으로 액션이 들어와야한다.

**이함수가 호출되면 스토어는 리듀서 함수를 실행시켜 새로운 상태를 만든다.**

<br>

## 구독

<br>

마찬가지로 위의 스토어의 내장 함수 이다.

subscribe 함수안에 리스너 함수를 파라미터로 넣어 호출해주면,

<br>

액션이 디스패치되어 리듀서를 실행시켜 새로운 상태를 만들때마다(업데이트) 호출된다.

<br>

리스너 함수란 액션이 실행될때

실행되어지는 함수를 리스너 함수라고 한다.

```jsx
const listener = () => {
  console.log('상태가 업데이트됨');
};
const unsubscribe = store.subscribe(listener);

unsubscribe();
```

<br>

구독이 일어난뒤 메모리 누수가 일어나지 않게 항상

unsubscribe를 해주어야한다.

<br>

## 예제

<br>

액션은 함수로 만들어준다.

```jsx
const toggleSwitch = () => {
  {
    type: TOGGLE_SWITCH;
  }
};
const increase = (difference) => {
  {
    type: INCREASE;
    difference;
  }
};
const decrease = () => {
  {
    type: DECREASE;
  }
};
```

<br>

리듀서 함수추가

```jsx
const divToggle = document.querySelector('.toggle');
const counter = document.querySelector('h1');
const btnIncrease = document.querySelector('#increase');
const btnDecrease = document.querySelector('#decrease');

const TOGGLE_SWITCH = 'TOGGLE_SWITCH';
const INCREASE = 'INCREASE';
const DECREASE = 'DECREASE';

const toggleSwitch = () => {
  {
    type: TOGGLE_SWITCH;
  }
};
const increase = (difference) => {
  {
    type: INCREASE;
    difference;
  }
};
const decrease = () => {
  {
    type: DECREASE;
  }
};

const initialState = {
  toggle: false,
  counter: 0,
};

function reducer(state = initialState, action) {
  switch (action.type) {
    case TOGGLE_SWITCH:
      return {
        ...state,
        toggle: !state.toggle,
      };
    case INCREASE:
      return {
        ...state,
        toggle: state.counter + action.difference,
      };
    case DECREASE:
      return {
        ...state,
        toggle: state.counter - 1,
      };
    default:
      return state;
  }
}
```

<br>

리듀서는 상태의 불변성을 지키면서 데이터에 변화를 주어야한다.

따라서 스프레드 문법을 통해 불변성을 관리하므로 리덕스 상태가 최대한 깊지 않은 구조로 진행해야

업데이트 하는데 번거롭지 않고 코드의 가독성도 좋아진다.

<br>

만약 복잡해진다면 inmmer 라이브러리르 사용하면 리듀서를 좀더 쉽게 작성할 수 있다.

```jsx
const store = createStore(reducer);
```

<br>

마지막에 스토어에 리듀서함수를 매개변수의 값으로 넣어주어 리듀서를 정해준다.

```jsx
import { createStore } from 'redux';
const divToggle = document.querySelector('.toggle');
const counter = document.querySelector('h1');
const btnIncrease = document.querySelector('#increase');
const btnDecrease = document.querySelector('#decrease');

const TOGGLE_SWITCH = 'TOGGLE_SWITCH';
const INCREASE = 'INCREASE';
const DECREASE = 'DECREASE';

const toggleSwitch = () => {
  {
    type: TOGGLE_SWITCH;
  }
};
const increase = (difference) => {
  {
    type: INCREASE;
    difference;
  }
};
const decrease = () => {
  {
    type: DECREASE;
  }
};

const initialState = {
  toggle: false,
  counter: 0,
};

function reducer(state = initialState, action) {
  switch (action.type) {
    case TOGGLE_SWITCH:
      return {
        ...state,
        toggle: !state.toggle,
      };
    case INCREASE:
      return {
        ...state,
        toggle: state.counter + action.difference,
      };
    case DECREASE:
      return {
        ...state,
        toggle: state.counter - 1,
      };
    default:
      return state;
  }
}

const store = createStore(reducer);

const render = () => {
  const state = store.getState(); // 현재 상태를 불러옵니다.
  // 토글 처리
  if (state.toggle) {
    divToggle.classList.add('active');
  } else {
    divToggle.classList.remove('active');
  }
  // 카운터 처리
  counter.innerText = state.counter;
};

render();

store.subcribe(render);

divToggle.onclick = () => {
  store.dispatch(toggleSwitch());
};
divToggle.onclick = () => {
  store.dispatch(increase(1));
};
divToggle.onclick = () => {
  store.dispatch(decrease());
};
```

<br>

subcribe dispatch가 일어날때마다 render함수가 발생하게한다.

클릭 이벤트가 되면 dispatch로 액션을 발생시켜 리듀서 함수가 실행되어

새로운 상태 객체를 만들어 render가 호출되어 값을 변경시킨다.

- 이글은 책 [리액트를 다루는 기술](http://www.yes24.com/Product/Goods/78233628?OzSrank=1)을 참고하여 정리한 글입니다.
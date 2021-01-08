# redux-saga

<br>

- 이 글은 [책 리액트를 다루는 기술](http://www.yes24.com/Product/Goods/78233628?OzSrank=1) 과 [redux-saga 공식홈페이지](https://mskims.github.io/redux-saga-in-korean/introduction/BeginnerTutorial.html) 를 참고하여 정리한 글입니다.

<br>

redux-saga는 액션을 모니터링 하고 특정 액션이 발생하면 특정 작업을 하는것으로 볼 수 있다.

<br>

redux-saga는 다음과 같은 상황일때 사용하는 것이 좋다

1. 기존 요청을 취소 처리
2. 특정 액션이 발생했을때 다른 액션을 발생, 리덕스와 관계없는 코드를 실행할때
3. 웹소켓을 사용할떄
4. API요청 실패시 재요청해야 할때

<br>

redux-saga는 제너레이터 문법을 사용해서 구현한다.

redux-saga에는 여러 유용한 유틸함수가 있어 액션을 쉽게 처리 할 수 있다.

<br>

reudux-thunk에 비교할때 콜백 헬을 끝내지 않고, 단지 비동기 흐름을 쉽게 테스트 가능하고 action을 순수하게 유지할 수 있다.

<br>

**설치 방법**

```bash
npm i redux-saga
```

<br>

## saga 사용법

```jsx
import { createAction, handleActions } from 'redux-actions';
import { delay, put, takeEvery, takeLatest } from 'redux-saga';

// 액션 타입 설정
const INCREASE = 'counter/INCREASE';
const DECREASE = 'counter/DECREASE';
const INCREASE_ASYNC = 'counter/INCREASE_ASYNC';
const DECREASE_ASYNC = 'counter/DECREASE_ASYNC';

// 액션 생성 함수
export const increase = createAction(INCREASE);
export const decrease = createAction(DECREASE);

export const incraseAsync = createAction(INCREASE_ASYNC, () => undefined);
export const decraseAsync = createAction(DECREASE_ASYNC, () => undefined);

function* increaseSaga() {
  yield delay(1000); // 1초를 기다린다.
  yield put(increase()); // 특정 액션을 디스패치한다.
}

function* decreaseSaga() {
  yield delay(1000); // 1초를 기다린다.
  yield put(decrease()); // 특정 액션을 디스패치한다.
}

export function* counterSaga() {
  yield takeEvery(INCREASE_ASYNC, increaseSaga);
  yield takeLatest(DECREASE_ASYNC, decreaseSaga);
}

// 상태 초기값
const initialState = 100;

// 리듀서
const counter = handleActions(
  {
    [INCREASE]: (state) => state + 1,
    [DECREASE]: (state) => state - 1,
  },
  initialState,
);

export default counter;
```

<br>

**`delay`** 라는 명령은 몇초동안 기다리라는 이펙트이다.

이 delay라는 이펙트를 호출하면 순수 자바스크립트 객체를 반환한다.

이 객체를 미들웨어로서 분석하여 프로미스의 resolve되게까지 기다리게하는 이펙트이다.

설정된 시간이후에 resolve를 하는 promise 객체를 반환한다. 제너레이터를 정지하는데 사용한다.

<br>

`**call`** 이펙트는 받아온 요청방식과 api를 요청하여 그에대한 resolve를 반환한다.

이때 반환한 값을 yield하여 변수에 할당해준다.

<br>

call 이펙트를 통해 resolve된것을 받고 추가적으로 dispatch된 작업을 수행하기 위해서는 `**put**` 이라는 이펙트를 사용해야한다. 

put은 특정액션을 dispatch하라는 이펙트이다.

<br>

**`put`** 은 dispatch와 비슷하다 increase를 호출해서 increase 액션객체를 만들고

그 액션을 dispatch하도록 리덕스 사가 effect에게 명령하는것이다.

<br>

**`takeEvery`** 를 사용하여 takeEvery안의 

첫번째 파라미터값인 액션타입이 디스패치되면 

두번째 파라미터값인 제너레이터 함수(사가)를 실행시켜준다.

<br>

즉, takeEvery를 포함한 사가는 특정 액션이 일어났을때 사가를 호출하여 실행시키도록 해준다.

<br>

위의 예제에 따르면

첫번째 파라미터값인 `INCREASE_ASYNC` 액션이 계속 호출됨에 따라 `increaseSaga` 도계속 호출되고 `INCREASE` 액션도 계속 호출된다.

그이유는 takeEvery를 사용하면 dispatch가 될때마다 saga가 계속 실행되기 때문이다. 그에반면에 takeLates는

<br>

**`takeLates`** 는 가장 마지막에 실행된 작업만 수행한다.

즉, 첫번째 파라미터값인 액션타입이 디스패치되면

기존의 비동기 상태 대기중이던 작업들을 모두 최소하고 가장 마지막으로 실행된 작업만 수행한다.

<br>

위의 예제에 따르면

`DECREASE_ASYNC` 액션은 계속 실행되어도 여러 액션이 중첩 되어있을때는 기존의 것들은 무시하고 가장 마지막 액션만 제대로 처리한다.

떄문에 `DECREASE_ASYNC` 는 계속 액션이 되지만 `DECREASE` 는 단한번 마지막에 액션이 실행된다.

<br>

**`takeleading`** 은 가장 먼저 들어온 작업만 수행한다.

가장먼저 들어온 작업이 비동기 일때 이 비동기 작업의 가장 첫번째 작업이 종료 될때까지 그이후 작업들은 취소된다. 

<br>

그이후 처음 비동기 작업이 다 완료 된이후에는 또다시 다음 비동기 작업을 할 수 있다.

<br>

여러가지 saga를 가지고 루트 사가를 만들어야 하므로

내보내주어야 하는 모니터중인 saga를 만들어 export해준다.

```jsx
export function* counterSaga() {
  yield takeEvery(INCREASE_ASYNC, increaseSaga);
  yield takeLatest(DECREASE_ASYNC, decreaseSaga);
}
```

<br>

modules/index.js

루트 리듀서를 만든것처럼 루트 사가를 만들어주어야한다.

```jsx
import { all } from 'redux-saga';

export function* rootSaga() {
  // all 함수는 여러가지 사가를 합쳐준다.

  yield all([counterSaga()]);
}
```

<br>

추후에 다른 리듀서에서도 사가를 만들어 등록할 것이기 때문에 합쳐준다.

`all`은 export 하는 사가를 넣어주어 호출시켜주면된다. 

그 뒤에 다른사가들을 더 추가하고 싶으면 배열에 추가해주면된다.

<br>

**지금까지 만든 모든 사가들을 한번에 시작하기 위한 단일 entryPoint이다.**

**두 사가들이 병렬로 시작한다라는 것을 의미한다.**

<br>

### index.js saga적용

<br>

src/index.js

```jsx
import createSagaMiddleWare from 'redux-saga';

const sagaMiddleware = createSagaMiddleWare();
const store = createStore(
  rootReducer,
  applyMiddleware(logger, ReduxThunk, sagaMiddleware),
);
sagaMiddleware.run(rootSaga);
```

<br>

1 . createSagaMiddleWare함수를 redux-saga라이브러리에서 불러와 

호출하여 변수에 할당한뒤 미들웨어로 등록한다.

<br>

2 . 변수에 할당한 sagaMiddleware를 run메소드의 파라미터에 rootSaga를 넣어준다.

<br>

### 예제)

<br>

modules/sample

```jsx
import { createAction, handleActions } from 'redux-actions';
import { call, put, takeLatest } from 'redux-saga/effects';
import * as api from '../lib/api';
import { startLoading, finishLoading } from './loading';

const GET_POST = 'sample/GET_POST';
const GET_POST_SUCCESS = 'sample/GET_POST_SUCCESS';
const GET_POST_FAILURE = 'sample/GET_USERS_FAILURE';

const GET_USERS = 'sample/GET_USERS';
const GET_USERS_SUCCESS = 'sample/GET_USERS_SUCCESS';
const GET_USERS_FAILURE = 'sample/GET_USERS_FAILURE';

export const getPost = createAction(GET_POST, (id) => id);
export const getUsers = createAction(GET_USERS);

function* getPostSaga(action) {
  yield put(startLoading(GET_POST)); // 로딩 시작
  try {
    const post = yield call(api.getPost, action.payload);
    yield put({
      type: GET_POST_SUCCESS,
      payload: post.data,
    });
  } catch (e) {
    yield put({
      type: GET_POST_FAILURE,
      payload: e,
      error: true,
    });
  }
}

function* getUsersSaga(action) {
  yield put(startLoading(GET_USERS)); // 로딩 시작
  try {
    const post = yield call(api.getUsers);
    yield put({
      type: GET_USERS_SUCCESS,
      payload: post.data,
    });
  } catch (e) {
    yield put({
      type: GET_USERS_FAILURE,
      payload: e,
      error: true,
    });
  }
  yield put(finishLoading(GET_USERS));
}

export function* sampleSaga() {
  yield takeLatest(GET_POST, getPostSaga);
  yield takeLatest(GET_USERS, getUsersSaga);
}

// 초기 상태
// 요청이 로딩중일떄는 loading이라는 객체에서 관리
const initalState = {
  post: null,
  users: null,
};

const sample = handleActions(
  {
    [GET_POST_SUCCESS]: (state, action) => ({
      ...state,
      post: action.payload,
    }),

    [GET_USERS_SUCCESS]: (state, action) => ({
      ...state,
      users: action.payload,
    }),
  },
  initalState,
);

export default sample;
```

<br>

GET_POST 액션의 경우에는 API 요청을 할 때 어떤 id로 조회 할지 정해주어야한다.

redux-saga를 사용할때 id처럼 추가적으로 요청에 필요한 값을 액션의 payload로 넣어 주어야한다.

사가 첫번쨰 파라미터의 값으로 action값을 받아 올 수 있다.

<br>

또한 API를 호출하야 하는 상황에서는 사가 내부에서 직첩 호출 하지 않는다.

대신에 call함수를 사용한다.

call함수의 첫번째 인수는 호출하고 싶은 함수이다.

그뒤에 오는 인수는 해당함수의 넣어주고 싶은 인수가 오면된다.

<br>

payload로 넣어주기 위해서는 payload값을 API를 호출하는 함수의 인수로 넣어주어야한다.

```jsx
const SampleContainer = ({
  post,
  users,
  loadingPost,
  loadingUsers,
  getPost,
  getUsers,
}) => {
  useEffect(() => {
    getPost(1);
    getUsers(1);
  }, [getUsers, getPost]);
  return (
    <Sample
      post={post}
      users={users}
      loadingPost={loadingPost}
      loadingUsers={loadingUsers}
    />
  );
};
```

위코드에서 getPost(1)로 payload의 값을 1로 넣어주었다.

<br>

예제 리팩토링

```jsx
import { createAction, handleActions } from 'redux-actions';
import { takeLatest } from 'redux-saga/effects';
import * as api from '../lib/api';
import createRequestSaga from '../lib/createRequestSaga';

const GET_POST = 'sample/GET_POST';
const GET_POST_SUCCESS = 'sample/GET_POST_SUCCESS';

const GET_USERS = 'sample/GET_USERS';
const GET_USERS_SUCCESS = 'sample/GET_USERS_SUCCESS';

export const getPost = createAction(GET_POST, (id) => id);
export const getUsers = createAction(GET_USERS);

const getPostSaga = createRequestSaga(GET_POST, api.getPost);
const getUsersSaga = createRequestSaga(GET_USERS, api.getUsers);

export function* sampleSaga() {
  yield takeLatest(GET_POST, getPostSaga);
  yield takeLatest(GET_USERS, getUsersSaga);
}

// 초기 상태
// 요청이 로딩중일떄는 loading이라는 객체에서 관리
const initalState = {
  post: null,
  users: null,
};

const sample = handleActions(
  {
    [GET_POST_SUCCESS]: (state, action) => ({
      ...state,
      post: action.payload,
    }),

    [GET_USERS_SUCCESS]: (state, action) => ({
      ...state,
      users: action.payload,
    }),
  },
  initalState,
);

export default sample;
```

<br>

lib/createRequestSaga.js

```jsx
import { call, put } from 'redux-saga/effects';
import { startLoading, finishLoading } from '../modules/loading';

export default function createRequestSaga(type, request) {
  const SUCCESS = `${type}_SUCCESS`;
  const FAILURE = `${type}_FAILURE`;

  return function* (action) {
    yield put(startLoading(type)); // 로딩 시작
    try {
      const response = yield call(request, action.payload);
      yield put({
        type: SUCCESS,
        payload: response.data,
      });
    } catch (e) {
      yield put({
        type: FAILURE,
        payload: e,
        error: true,
      });
    }
    yield put(finishLoading(type));
  };
}
```

<br>

modules/index 사가 합쳐주기

```jsx
export function* rootSaga() {
  // all 함수는 여러가지 사가를 합쳐준다.
  yield all([counterSaga(), sampleSaga()]);
}
```

<br>

redux-saga에는 여러기능을 제공한다.

[redux-saga 메뉴얼](https://redux-saga.js.org/)을 참고하면 좋다.

<br>

![redux-saga](../Images/redux-saga/리덕스%20미들웨어-1.png)

<br>

**정리**

<br>

**리덕스 사가 적용방법**

```jsx
import createSagaMiddleware from "redux-saga";

import Counter from "./Counter";
import reducer from "./reducers";

const sagaMiddleware = createSagaMiddleware();

const store = createStore(reducer, applyMiddleware(sagaMiddleware));

sagaMiddleware.run();
```

<br>

1 . redux-saga에서 createSagaMiddleware을 import한다. `import createSagaMiddleware from "redux-saga";`

2 . createSagaMiddleware를 호출하여 변수에 할당한다. `const sagaMiddleware = createSagaMiddleware();`

3 . 스토어에 미들웨어로 적용시킨다. `const store = createStore(reducer, applyMiddleware(sagaMiddleware));`

4 . 사가 미들웨를 작동시킨다. `sagaMiddleware.run();`
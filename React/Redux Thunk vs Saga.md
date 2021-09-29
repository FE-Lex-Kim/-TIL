# Redux Thunk vs Saga

리덕스 상태관리 미들웨어 중에서 Thunk와 Saga가 많이 사용된다.

이 두가지 미들웨어는 많이 사용된다는 점에서 자신의 프로젝트에서 어떤 미들웨를 사용하는게 좋을지 고민하게 된다.

딱히 사람들이 많이 사용해서 해당 미들웨어를 쓰다기보다, 의미를 알고 내가 능동적으로 사용하는것이 트렌드에 따라기기 쉽고 새로운 기술이 들어올때도 쉽게 적응한다.

<br>

이미 Thunk와 Saga를 사용해보고 공부했다는 가정하에 비교해보자.

<br>

## Saga

```jsx
// 액션 타입 설정
const INCREASE = "counter/INCREASE";
const DECREASE = "counter/DECREASE";
const INCREASE_ASYNC = "counter/INCREASE_ASYNC";
const DECREASE_ASYNC = "counter/DECREASE_ASYNC";

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
```

- Saga는 Thunk와 다르게 특정 작업 없이 해당 액션이 dispatch 된다면 saga가 실행된다.
- 비동기 데이터 흐름을 쉽게 테스트 한다.
- 사가의 흐름은 특정 액션이 발생하는지 모니터한다. 따라서 특정 로직이 발생하면 그에 따른 로직을 발생시킬수 있다. 해당 액션에 따른 테스트가 가능하다는 점이 있다.

<br>

## Thunk

<br>

```jsx
const GET_POST = "sample/GET_POST";
const GET_POST_SUCCESS = "simple/GET_POST_SUCCESS";
const GET_POST_FAILURE = "simple/GET_POST_FAILURE";

// thunk 함수 생성
// thunk 함수 시작, 성공, 실패 했을때 액션을 디스패치 한다.

export const getPost = (id) => async (dispatch) => {
  dispatch({ type: GET_POST }); // 요청 시작을 알림
  try {
    const response = await api.getPost(id);
    dispatch({
      type: GET_POST_SUCCESS,
      payload: response.data,
    }); // 요청 성공시때
  } catch (e) {
    // 요청 실패시
    dispatch({ type: GET_POST_FAILURE, payload: e, error: true });
    throw e;
  }
};
```

- 떵크는 새로운 액션 생성함수를 만들어야한다.
- 해당 액션 생성함수가 반환하는 값은 함수이다.
- 이 함수는 다양한 로직을 담는다.
- 따라서 실제 특정 action에 따른 특정 로직을 수행하는 것이 아니라, 어떠한 action을 발생시키면 패키지 같은 개념으로 로직들을 실행시킨다는 개념이다.

<br>

## 정리

Saga와 Thunk는 비슷한 로직과 개념인것 같지만 한끗차이로 다르다.

<br>

Thunk는 특정 action에 대한 응답을 주지 못하고, 새로운 action 생성함수를 만들어서 리듀서를 거치지 않고 미들웨어에서 여러가지 로직들을 실행한다.

Saga는 Store을 구독하고 모니터 해서 특정 action이 dispatch 되었을때, 실행되도록 하는 개념이다.

<br>

사실 Saga와 Thunk가 무엇이 다른지 계속 고민하고 구글링한지 꽤 오래되었지만 큰 차이를 느끼지 못하고 있다.

<br>

많은 블로그나 아티클에서는

- Thunk는 중소규모의 프로젝트, Saga는 대형 프로젝트
- Thunk는 적은 boilerplate code, Saga는 Thunk보다 좀더 많은 boilerplate code
- Thunk는 Saga에 비해 이해하기 쉬움, Saga는 Thunk에 비해 제너레이터 함수를 사용한다는 점에서 어려움
- Thunk는 action 생성함수가 많아지는 로직, Saga는 action 생성 함수가 순수하게 그 기능만 가지는 로직

이라고 한다.

<br>

하지만 내가느끼는 점은 기능적인면에서 그렇게 두가지의 큰 차이점을 느끼지 못하고 있다.

<br>

다만 확실히 Thunk보다 Saga는 action 생성함수에서 Thunk와 다르게 함수를 만들지 않는다.

따라서 순수하게 리듀서에 들어갈 action 생성함수를 만들다 보니 로직이 더 깔끔하다.

깔끔하다는 것은 로직의 흐름을 쉽게 볼수 있어 유지보수, 가독성, 코드 추적이 쉽다는 점에서 부여하는 의미가 크다.

<br>

아무리 그래도 Thunk는 자주 사용되지 않는 제너레이터 함수를 사용하지 않는 다는 점, 로직 자체가 굉장히 심플하다는점에서 나는 큰 장점을 주고 싶다.

<br>

아무래도 나는 사용한다면 Thunk가 좋지 않을까 쉽다.

<br>

※ side effect : 비동기 요청, 브라우저 캐시, 로컬스토리지 등등 JS 코드가 외부 세계에 영향을 주거나 받는 것을 말한다.

참고

- [https://tudip.com/blog-post/why-react-saga-is-better-then-thunk/](https://tudip.com/blog-post/why-react-saga-is-better-then-thunk/)
- [https://www.eternussolutions.com/2020/12/21/redux-thunk-redux-saga/](https://www.eternussolutions.com/2020/12/21/redux-thunk-redux-saga/)
- [https://velog.io/@dongwon2/Redux-Thunk-vs-Redux-Saga를-비교해-봅시다-](https://velog.io/@dongwon2/Redux-Thunk-vs-Redux-Saga%EB%A5%BC-%EB%B9%84%EA%B5%90%ED%95%B4-%EB%B4%85%EC%8B%9C%EB%8B%A4-)

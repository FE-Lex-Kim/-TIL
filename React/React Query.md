- [React Query](#react-query)
  - [React Query이 생겨난 배경](#react-query이-생겨난-배경)
    - [Server state, Client state 분리](#server-state-client-state-분리)
    - [추가적 장점](#추가적-장점)
  - [Redux Thunk vs React Query](#redux-thunk-vs-react-query)
    - [Redux thunk](#redux-thunk)
    - [React Query](#react-query-1)
  - [React Query 캐싱](#react-query-캐싱)
  - [정리](#정리)

<br>

# React Query

**React Query는 비동기 데이터 처리를 쉽게해주는 라이브러리이다.**

<br>

React Query에 대한 **사용법은 이번글에 작성하지 않았다.**

React Query가 **생겨난 배경, 실제 코드를 비교하여 유용한지 확인, 캐싱에 대한 장점을 작성한 글이다.**

이러한 React Query의 **배경을 공부한뒤 다음 글에서 사용법에 대해 공부해보자.**

<br>

## React Query이 생겨난 배경

### Server state, Client state 분리

**서버 상태와 클라이언트 상태를 하나의 상태 저장소에서 관리한다.**

- 서버 상태는 **백엔드에서 불러온 데이터를 의미한다.**
- 클라이언트 상태는 **UI를 위한 상태를 의미한다.**

예를 들어 **Redux store에 각각 컴포넌트 별로 필요로 하는 상태들을 저장한다.**

**즉, data-fetcing을 통해 가져온 서버 상태와 UI을 위한 클라이언트 상태를 동시에 관리한다.**

<br>

이것이 **문제되는 이유는**

**서버 상태와 클라이언트 상태의 특성이 다르기 때문이다.**

- 서버 상태는 클라이언트에서 관리하지않고 **서버에서 상태를 제어하고 소유한다는 점이다.**
- 서버 상태는 **비동기적으로 API 요청을 해야한다는 점이다.**
- 서버 상태는 동기적으로 변경되므로, **클라이언트가 모르는 사이에 다른 사용자가 변경 할 수 있다.**
- 서버 상태는 **언제 유효기간이 만료될지 모른다.**

<br>

이러한 **서버 상태와 클라이언트 상태는** 다른 방식으로 다뤄져야 **효율적으로 작업할 수 있다.**

<br>

**React Query를 통해 분리해서 관리 할 수 있다.**

### 추가적 장점

- 복잡하고 이해하기 어려운 **많은 라인의 코드를** **React Query를 사용해 대체해서 줄일 수 있게 도와준다.**
- **서버의 리소스가 추가되어도 걱정없이, 더 쉽고 관리하기 좋게 새로운 기능을 추가할 수 있다.**
- **사용자가 어플리케이션이 즉각 반응하고 더 빠르다고 느끼게 성능이 개선된다.**
- **네트워크 통신을 최소한으로 아끼고 메모리 성능이 향상된다.**

<br>

이번엔 Server-state를 관리하기 위해 사용하는 **여러가지 라이브러리중 가장 많이 쓰이는 Thunk와 비교해보자.**

<br>

## Redux Thunk vs React Query

<br>

[\*\*HNPWA API](https://github.com/tastejs/hacker-news-pwas/blob/master/docs/api.md)를 사용해서 news을 가져오는 로직을 만들어 비교해보자.\*\*

<br>

### Redux thunk

**Loding, Error 상태도 추가해 로직이 더 길어졌다.**

**Thunk 같은 경우는 보일러 플레이트 코드가 너무 길다.**

- 액션 타입
- 액션 생성자 함수
- 초기값
- 리듀서
- 루트 리듀서

<br>

특히 액션 타입과 액션 생성자 함수의 네이밍은 똑같다.(RTK를 사용하면 다르지만)

**또한 단순히 서버로 부터 데이터를 불러와서 저장소에 저장하기만한다.**

<br>

newsReducer.js

```jsx
import axios from "axios";
import { createAction, handleActions } from "redux-actions";

const API_URL = "https://api.hnpwa.com/v0/news/1.json";

// 액션 타입
const GET_NEWS = "GET_NEWS";
const GET_NEWS_SUCCESS = "GET_NEWS_SUCCESS";
const GET_NEWS_FAILURE = "GET_NEWS_FAILURE";

// 액션 생성자 함수
export const getNews = createAction(GET_NEWS);
export const getNewsSuccess = createAction(GET_NEWS_SUCCESS, (news) => news);
export const getNewsFailure = createAction(GET_NEWS_FAILURE, (error) => error);

export const getNewsThunk = () => async (dispatch) => {
  dispatch(getNews());
  try {
    const res = await axios.get(API_URL);
    dispatch(getNewsSuccess(res.data));
  } catch (error) {
    dispatch(getNewsFailure(error.message));
  }
};

// 초기값
const init = {
  isLoading: false,
  news: [],
  isError: false,
};

// 리듀서
const news = handleActions(
  {
    [GET_NEWS]: (state, action) => ({
      ...state,
      isLoading: true,
    }),
    [GET_NEWS_SUCCESS]: (state, action) => ({
      ...state,
      isLoading: false,
      news: action.payload,
    }),
    [GET_NEWS_FAILURE]: (state, action) => ({
      news: action.payload,
      isLoading: false,
      isError: true,
    }),
  },
  init
);

export default news;
```

<br>

News.jsx

```jsx
import React from "react";
import { useEffect } from "react";
import { useDispatch, useSelector } from "react-redux";
import { getNewsThunk } from "../redux/news";

const News = () => {
  const { isLoading, news, isError } = useSelector((state) => state.news);
  const dispatch = useDispatch();
  useEffect(() => {
    dispatch(getNewsThunk());
  }, [dispatch]);

  return (
    <div>
      {isLoading && "로딩중..."}
      {!isLoading && isError && "점검중 입니다."}
      {!isLoading && !isError && (
        <div>
          {news.map((v) => (
            <p key={v.title}>{v.title}</p>
          ))}
        </div>
      )}
    </div>
  );
};

export default News;
```

<br>

### React Query

React query에서는

- **loading, error 상태 관리**
- **error가 발생시 다시 데이터 페칭을 시도**
- **요청이 성공, 실패시 callback을 따로 정의 가능**

**.. 등등 다양한 기능을 내장하고 있다.**

<br>

물론 위의 **Thunk 코드에서도 위와 같은 기능을** **직접 코드를 구현할 수 있다.**

하지만 위의 코드는 사실상 코드를 작성하는데 **코드의 줄이 길어지고 굉장한 피로감을 준다.**

**React query의 내장된 기능으로 코드의 길이를 굉장히 단축시킬 수 있다.**

<br>

이제 위의 코드를 React query로 대체해보자.

<br>

NewsQuery.jsx

```jsx
import axios from "axios";
import React from "react";
import { useQuery } from "react-query";
const API_URL = "https://api.hnpwa.com/v0/news/1.json";

const NewsQuery = () => {
  const result = useQuery("HackerNews", () => {
    return axios.get(API_URL);
  });

  const { isLoading, error, data } = result;

  if (isLoading) {
    return <h2>로딩중...</h2>;
  }

  if (error) {
    return <h2>점검중...</h2>;
  }

  return (
    <div>
      {data.data.map((v) => (
        <p key={v.title}>{v.title}</p>
      ))}
    </div>
  );
};

export default NewsQuery;
```

<br>

Thunk가 가지고 있던 **이해하기 어렵고 가독성이 떨어지는 긴 코드들이 싹 사라졌다.**

또한

서버 리소스가 필요한 **해당 컴포넌트 내부에서 직접 관리하고 있음을 알 수 있다.**

<br>

## React Query 캐싱

**useQuery Argument(인수)를 통해 여러가지 캐싱 옵션을 설정할 수 있다.**

- **API 데이터 만료시간**
- **리프레싱 주기**
- **데이터 캐싱 유효시간**
- **브라우저 리-포커스시 데이터 재요청**
- **데이터 요청 성공 / 실패에 대한 Callback 함수 정의, 호출 가능**

**.. 등등 제어가 가능하다.**

<br>

이러한 강력한 캐싱 기능을 통해 어떤 것을 할 수 있는지 **간단한 예제를 알아보자.**

1. **서버에서 변경될 가능성이 낮은 데이터의 캐싱 만료기간을 무한으로 설정해, 추가적으로 API 요청을 방지한다.** 이로써 로딩 성능이 굉장히 향상된다.
2. **캐시 유효기간을 적절히 설정하여 사용자가 페이지네이션을 반복해서 이용할 경우, 이미 방문한 페이지의 API 요청을 방지해** 로딩성능을 향상시킨다.
3. **브라우저 사용자가 서버에 새로운 데이터 추가시(ex 게시판 추가), 캐시를 강제로 무효화 시켜, 새로고침후 다시 가져와 캐싱을 시키게한다.**

<br>

이렇게 몇가지 간단한 예제가 있지만, 다른 **여러 상황에 캐싱을 옵션 설정해서 성능을 많이 높일 수 있다.**

<br>

## 정리

- **기존 Redux를 사용한 data-fetching의 보일러 플레이트 로직을 압도적으로 줄였다.**
- **Client State, Server State를 분리해서 관리해 효율적인 코드를 작성할 수 있다.**
- **강력한 캐싱 기능을 사용하여 로딩 성능을 향상 시킬 수 있다.**

<br>

다음 글은 사용법에 대해 알아보자.

<br>

참고

- [https://react-query.tanstack.com/overview](https://react-query.tanstack.com/overview)
- [https://maxkim-j.github.io/posts/react-query-preview](https://maxkim-j.github.io/posts/react-query-preview)
- [https://dev.to/g_abud/why-i-quit-redux-1knl](https://dev.to/g_abud/why-i-quit-redux-1knl)
- [https://blog.rhostem.com/posts/2021-02-01T00:00:00.000Z](https://blog.rhostem.com/posts/2021-02-01T00:00:00.000Z)
- [https://swoo1226.tistory.com/216](https://swoo1226.tistory.com/216)
- [https://www.youtube.com/watch?v=Nm0inP3B_zs&list=PLC3y8-rFHvwjTELCrPrcZlo6blLBUspd2&index=2](https://www.youtube.com/watch?v=Nm0inP3B_zs&list=PLC3y8-rFHvwjTELCrPrcZlo6blLBUspd2&index=2)

- [React Query 튜토리얼(1) - 환경 설정, useQuery, handle Error, devtools](#react-query-튜토리얼1---환경-설정-usequery-handle-error-devtools)
  - [React Query 사용하기](#react-query-사용하기)
  - [useQuery 사용](#usequery-사용)
  - [Error 처리하기](#error-처리하기)
    - [데이터 재요청](#데이터-재요청)
  - [Query Devtools](#query-devtools)
  - [정리](#정리)

# React Query 튜토리얼(1) - 환경 설정, useQuery, handle Error, devtools

[React Query의 장점들을 알았으니](https://github.com/FE-Lex-Kim/-TIL/blob/master/React/React%20Query.md), 직접 사용하는 방법에 대해 알아보자.

<br>

설치

```bash
npm i react-query
```

<br>

## React Query 사용하기

React Query를 사용하기 위해

1. **`QueryClientProbiver`을 하위 컴포넌트들을 감싸준다.**
2. **`new QueryClient` 인스턴스를 client props로 전달해준다.**

<br>

App.js

```jsx
const queryClient = new QueryClient(); // <------- 추가된 코드

function App() {
  return (
    <QueryClientProvider client={queryClient}> // <------- 추가된 코드
      ...Child Components
    </QueryClientProvider>
  );
}

export default App;
```

<br>

## useQuery 사용

**useQuery는 비동기 데이터를 요청할때 사용하는 Hook이다.**

useQuery는 **2개의 Argument(인수)를 사용한다.**

1. 첫 번째 Argument는 유일무이한 **문자열로 입력해준다.**(해당 문자열은 이후에 **캐싱된 값을 찾기위한 key**로 사용된다.)
2. 두 번째 Arguement는 **Promise를 반환하는 Callback 함수를 입력해준다.**

   **→ Callback 함수 내부에서 비동기 데이터 요청을 처리한다.**

<br>

```jsx
const result = useQuery("super-heroes", () => {
  return axios.get("http://localhost:4000/superheroes");
});

console.log(result);
```

<br>

**Query에 대한 정보들이 반환된다.**

![React Query 튜토리얼(1) - 환경 설정, useQuery, handle Error, devtools](<../Images/React%20Query%20튜토리얼(1)%20-%20환경%20설정,%20useQuery,%20handle%20Error,%20devtools/React%20Query%20튜토리얼(1)%20-%20환경%20설정,%20useQuery,%20handle%20Error,%20devtools-1.png>)

<br>

**가장 기본적으로 사용되는 값을 알아보자.**

- **isLoading : 데이터 요청 중임을 알려주는 값으로 Boolean이 나온다.**
- **data : Server에 요청한 데이터가 이곳에 담긴다.**
- **다른 값들은 차차 알아보자.**

<br>

**isLoading, data을 사용해서 예제 코드를 살펴보자.**

```jsx
import axios from "axios";
import { useQuery } from "react-query";

const fetchSuperHeroes = () => {
  return axios.get("http://localhost:4000/superheroes");
};

export const RQSuperHeroesPage = () => {
  const { isLoading, data } = useQuery("super-heroes", fetchSuperHeroes);

  if (isLoading) {
    return <h2>로딩중...</h2>;
  }

  return (
    <h2>
      {data?.data.map((hero) => (
        <div key={hero.id}>{hero.name}</div>
      ))}
    </h2>
  );
};
```

<br>

위와 같이 **데이터 요청하는데 있어 굉장히 짧고 쉽게 구현이 가능하다.**

**useQuery 내부에 내장되어 있는 기능들이 있기 때문에 굉장히 편리하고 효율적이다.**

<br>

## Error 처리하기

기존에 error을 추적하고 사용하기 위해, state를 사용했다.

**useQuery는 내장되어 있는 값을 통해 error을 쉽게 처리할 수 있다.**

<br>

**예제코드)**

```jsx
import axios from "axios";
import { useQuery } from "react-query";

const fetchSuperHeroes = () => {
  return axios.get("http://localhost:4000/superheroes");
};

export const RQSuperHeroesPage = () => {
  const { isLoading, data, isError, error } = useQuery(
    // <----- 추가된 코드
    "super-heroes",
    fetchSuperHeroes
  );

  if (isLoading) {
    return <h2>로딩중...</h2>;
  }

  if (isError) {
    return <h2>{error.message}</h2>; // <----- 추가된 코드
  }

  return (
    <h2>
      {data?.data.map((hero) => (
        <div key={hero.id}>{hero.name}</div>
      ))}
    </h2>
  );
};
```

<br>

**위의 예제 코드에서 isError, error가 추가되었다.**

- **isError : error가 발생했는지에 대한 Boolean 값이 반환된다.**
- **error : error에 대한 정보가 반환된다.**

<br>

### 데이터 재요청

위의 예제코드를 브라우저에 실행해보자.

**에러 메세지가 브라우에 뜨는데 시간이 오래걸린다.**

<br>

**그 이유는 useQuery가 데이터 요청 실패시, 자동으로 재요청을 한다.**

![React Query 튜토리얼(1) - 환경 설정, useQuery, handle Error, devtools](<../Images/React%20Query%20튜토리얼(1)%20-%20환경%20설정,%20useQuery,%20handle%20Error,%20devtools/React%20Query%20튜토리얼(1)%20-%20환경%20설정,%20useQuery,%20handle%20Error,%20devtools-2.png>)

**4번정도 재요청을 하는것으로 보인다.**

**또한 브라우저에 다시 포커싱이 갈때, 재요청을 자동으로 실행한다.**

- ex) 다른 크롬 탭에서 다시 돌아왔을때
- 해당 viewport를 다시 클릭했을때,

<br>

## Query Devtools

**React Query는 전용 Devtools을 가지고 있다.**

**디버깅 시간을 줄여주고 시각화를 도와준다.**

<br>

적용방법에 대해 알아보자.

가장 먼저 **ReactQueryDevtools 컴포넌트를 불러온다.**

```jsx
import { ReactQueryDevtools } from "react-query/devtools";
```

<br>

이후 **QueryProvider의 마지막 자식 컴포넌트로 넣어준다.**

```jsx
import { ReactQueryDevtools } from "react-query/devtools";

const queryClient = new QueryClient();

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      ...Child Components
			<ReactQueryDevtools initialIsOpen={false} position="bottom-right" /> // <-- 추가된 코드
    </QueryClientProvider>
  );
}

export default App;
```

<br>

옵션이 여러가지 있다.

그 중에 기본적인것만 알아보자.

- **initialIsOpen** **: React를 실행했을때, devtools가 열린 상태로 만들려면, true를 설정하면 된다.**
- **postion : devtools을 열수있는 로고의 위치를 설정해준다.**
  - top-left, top-right, bottom-left, bottom-right
  - 4가지가 있다.

<br>

※ devtools는 development mode 일때만 포함되어 사용가능하다. 따라서 번들했을때, production build에 포함 되는것에 걱정하지 않아도 된다.

<br>

이제 React를 실행해보면, 오른쪽 아래에 React Query 로고가 있다.

![React Query 튜토리얼(1) - 환경 설정, useQuery, handle Error, devtools](<../Images/React%20Query%20튜토리얼(1)%20-%20환경%20설정,%20useQuery,%20handle%20Error,%20devtools/React%20Query%20튜토리얼(1)%20-%20환경%20설정,%20useQuery,%20handle%20Error,%20devtools-3.png>)

<br>

해당 로고를 클릭하면, devtool이 나타난다.

![React Query 튜토리얼(1) - 환경 설정, useQuery, handle Error, devtools](<../Images/React%20Query%20튜토리얼(1)%20-%20환경%20설정,%20useQuery,%20handle%20Error,%20devtools/React%20Query%20튜토리얼(1)%20-%20환경%20설정,%20useQuery,%20handle%20Error,%20devtools-4.png>)

<br>

왼쪽 박스의 배열안에 super-heroes가 있다.

**useQuery의 첫번째 Argument로 입력한 문자열이 키 값으로 들어간 것이다.**

```jsx
const { isLoading, data } = useQuery("super-heroes", () => {
  return axios.get("http://localhost:4000/superheroes");
});
```

<br>

**키는 이후에 공부할 캐싱때 사용된다.**

오른쪽 박스는 query에 대한 여러가지 정보들이 나와있다.

<br>

## 정리

- React에서 기존에 비동기 요청시 사용하는것보다 **훨씬 코드의 줄어든다.**
- useQuery에 내장된 값들로 **효율적이고 편리하게 loading, error, data 로직을 구성한다.**
- 전용 devtools로 **디버깅 시간을 줄여주고 시각화해준다.**

<br>

**다음 튜토리얼은 React Query의 가장 강력한 기능인 캐싱에 대해 알아보자.**

<br>

참고

- [https://react-query.tanstack.com/overview](https://react-query.tanstack.com/overview)
- [https://www.youtube.com/watch?v=Ev60HKYFM0s&list=PLC3y8-rFHvwjTELCrPrcZlo6blLBUspd2&index=3](https://www.youtube.com/watch?v=Ev60HKYFM0s&list=PLC3y8-rFHvwjTELCrPrcZlo6blLBUspd2&index=3)

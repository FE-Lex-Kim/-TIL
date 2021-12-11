- [React Query 튜토리얼(4) - 데이터 요청 함수 이벤트 핸들러 등록 , Success and Error Callback, UI를 위한 데이터 가공 옵션 select, Custom Query Hook](#react-query-튜토리얼4---데이터-요청-함수-이벤트-핸들러-등록--success-and-error-callback-ui를-위한-데이터-가공-옵션-select-custom-query-hook)
  - [데이터 요청 함수 이벤트 핸들러 등록](#데이터-요청-함수-이벤트-핸들러-등록)
  - [Success and Error Callback](#success-and-error-callback)
    - [Success Callback](#success-callback)
    - [Error Callback](#error-callback)
  - [데이터 가공 옵션 select](#데이터-가공-옵션-select)
  - [Custom Query Hook](#custom-query-hook)

# React Query 튜토리얼(4) - 데이터 요청 함수 이벤트 핸들러 등록 , Success and Error Callback, UI를 위한 데이터 가공 옵션 select, Custom Query Hook

<br>

1. 지금까지의 튜토리얼에서는 Mount시 서버 데이터를 불러왔다. **이제는 이벤트 핸들러를 등록해서, 이벤트 발생시 데이터 요청하는 방법부터 알아보자.**
2. **데이터 요청을 완료 또는 에러 후, Callback 함수를 호출해 Side Effect를 호출하는 방법에 대해 알아보자.**
3. 서버에서 가져온 데이터를 **UI를 위한 데이터로 가공하는 옵션에 대해 알아보자.**
4. **useQuery를 재사용할 수 있게 Custom Hook으로 만들어보자.**

<br>

## 데이터 요청 함수 이벤트 핸들러 등록

유저가 **동적으로 데이터를 요청**하는 방법에 대해 알아보자.

**수동적으로 refetch를 하는 함수가 있다.**

**useQuery에서 반환되는 값 중에 refetch 함수를 사용하면된다.**

<br>

이 함수를 이벤트 핸들러에 등록을 해서 유저가 동적으로 불러올 수 있게 해보자.

```jsx
const { refetch } = useQuery({
   'unique key',
   () => {
      return axios.get("http://localhost:4000/superheroes");
   },
   {
     enabled: false,
   }
 })

return (
	<button onClick={refetch}>Click this button</button>
)
```

<br>

이벤트 핸들러를 등록하면, 이제 동적으로 서버 데이터를 불러올 수 있다.

**옵션으로 `enabled : false` 하게 되면, 자동으로 데이터 요청되는 것을 방지한다.**

- 이 옵션을 설정해주지 않으면, 클릭하기도 전에 알아서 요청해버린다.

![refetch](<../Images/React%20Query%20튜토리얼(4)/React%20Query%20튜토리얼(4)-1.png>)

**이렇게 수동으로 refetch 하지 않으면, 캐시에 아무값도 들어가지 않게된다.**

<br>

이렇게 이벤트를 통해서 **데이터를 불러왔어도, 캐시에 저장이 된다.**

**재요청시, isLoading 값은 false 값이 나오는데, 어떻게 Loading 중임을 브라우저에 어떻게 표시할까?**

- 예를들어) Spinner, Text.. 등등

<br>

```jsx
const { refetch } = useQuery({
 'unique key',
 () => {
   return axios.get("http://localhost:4000/superheroes");
 },
 {
   enabled: false,
 }
})

if (isLoading || isFetching) {
  return <h2>로딩중...</h2>;
}
```

<br>

**캐시를 사용한 후, refetch를 할때도 loading 상태임을 보여주어야 하기 때문에, isFetching 일때도 조건을 넣어주면된다.**

<br>

이번 예시는 onClick 이벤트에만 걸었지만, 다양한 상황에서도 적용시키면 된다.

<br>

## Success and Error Callback

**만약 성공적으로 데이터 요청이 완료되거나, 에러가 발생했때, side Effect를 발생시키고 싶을 수 도있다.**

**옵션으로 성공, 오류로 callback을 지정할 수 있다.**

<br>

먼저 추가하는 방법에 대해 알아보자.

<br>

### Success Callback

```jsx
function onSuccess(data) {
  console.log("side effect after data fetching");
  console.log(data);
}

function onError(params) {
  console.log("side effect after error");
}

const { isLoading, data, error, isFetching } = useQuery(
  "super-heroes",
  () => {
    return axios.get("http://localhost:4000/superheroes1");
  },
  {
    onSuccess,
    onError,
  }
);
```

세 번째 Argument 객체에 **onSuccess 프로퍼티 값으로 Success 함수를 넣어주면된다.**

es6의 메서드 단축 표현으로 작성했다.

<br>

**이때 Prameter로 불러온 data를 받아 올 수 있다.**

**이제 데이터 요청이 완료되는 시점에 Success Callback이 호출된다.**

![Success and Error Callback](<../Images/React%20Query%20튜토리얼(4)/React%20Query%20튜토리얼(4)-2.png>)

<br>

### Error Callback

```jsx
function onSuccess(data) {
  console.log("side effect after data fetching");
  console.log(data);
}

function onError(params) {
  console.log("side effect after error");
}

const { isLoading, data, error, isFetching } = useQuery(
  "super-heroes",
  () => {
    return axios.get("http://localhost:4000/superheroes1");
  },
  {
    onSuccess,
    onError,
  }
);
```

세 번째 Argument 객체에 **onError 프로퍼티 값으로 Error 함수를 넣어주면된다.**

es6의 메서드 단축 표현으로 작성했다.

<br>

**이때 Prameter로 불러온 error 객체를 받아 올 수 있다.**

**이제 오류가 발생하는 시점에 Error Callback이 호출된다.**

![Success and Error Callback](<../Images/React%20Query%20튜토리얼(4)/React%20Query%20튜토리얼(4)-3.png>)

**오류 발생시, Error Callback을 호출하기전 세 번의 재시도를 한다.**

<br>

**이제 모든 Side Effect를 이곳에서 처리할 수 있다.**

<br>

## 데이터 가공 옵션 select

**백엔드에서 따로 API 데이터 형태가 있지만, 프론트엔드 UI 만을 위한 데이터의 형태는 다를 수 있다.**

useQuery 옵션에서 **불러온 데이터를 다른 형태로 가공하는 기능을 제공한다.**

<br>

아래 예시코드를 보면서 이해해보자.

<br>

db.json

```json
{
  "superheroes": [
    {
      "id": 1,
      "name": "Batman",
      "alterEgo": "Bruce Wayne"
    },
    {
      "id": 2,
      "name": "Superman",
      "alterEgo": "Clark Kent"
    },
    {
      "id": 3,
      "name": "Wonder Woman",
      "alterEgo": "Princess Diana"
    },
    {
      "id": 4,
      "name": "Alex",
      "alterEgo": "Eojin Kim"
    }
  ]
}
```

<br>

하지만 우리가 **사용하는 컴포넌트 UI에서는 alterEgo는 필요없고 name 만 필요하다고 가정하자.**

```jsx
const { isLoading, data } = useQuery(
  "super-heroes",
  () => {
    return axios.get("http://localhost:4000/superheroes1");
  },
  {
    select: (data) => {
      // <------- 추가한 코드
      return data.data.map((hero) => ({
        name: hero.name,
        id: hero.id,
      }));
    },
  }
);

console.log(data);
```

**옵션 객체에 select 메서드를 넣어주면된다.**

- 서버로 부터 불러온 **데이터를 Parameter로 받는다.**
- 불러온 데이터를 **가공해서 반환하면된다.**

<br>

**이제 useQuery 가 반환하는 data는 select에서 반환하는 값이 된다.**

![데이터 가공 옵션 select](<../Images/React%20Query%20튜토리얼(4)/React%20Query%20튜토리얼(4)-4.png>)

<br>

**지금은 단순히 data 중에 일부를 가져왔지만, 예를들어**

**filter를 사용해 불필요한 데이터를 걸러내고 사용할 수 도 있고,**

**데이터 일부를 반환하거나 선택하는 등 다양한 상황에 맞춰 사용하면된다.**

<br>

**매우 편리한 기능이므로, 항상 이 옵션을 염두에 두는게 좋아보인다.**

<br>

## Custom Query Hook

<br>

useQuery의

- 첫 번째 Argument는 **고유의 키이다.**
- 두 번째 Argument는 **data 요청 함수이다.**
- 세 번째 Argument는 **구체적인 옵션 객체이다.**

<br>

만약 **동일한 요청을하는, 서로다른 각각의 컴포넌트들이 있을때는 어떻게 재사용할 수 있을까?**

당연히 복사 붙여넣기를 하는것 보다, **최고의 방법은 Custom Hook을 사용해서 재사용하는 것이다.**

<br>

예시 코드를 보고 이해해보자.

<br>

RQSuperHeroes.js

```jsx
import axios from "axios";
import { useQuery } from "react-query";

export const RQSuperHeroesPage = () => {
  function onSuccess({ data }) {
    console.log("side effect after data fetching");
  }

  function onError(error) {
    console.log("side effect after error");
  }

  const { isLoading, data, isError, error, isFetching, refetch } = useQuery(
    "super-heroes",
    () => {
      return axios.get("http://localhost:4000/superheroes");
    },
    {
      onSuccess,
      onError,
      select: (data) => {
        return data.data.map((hero) => ({
          name: hero.name,
          id: hero.id,
        }));
      },
    }
  );

  if (isLoading || isFetching) {
    return <h2>로딩중...</h2>;
  }

  if (isError) {
    return <h2>{error.message}</h2>;
  }

  return (
    <>
      <h1>RQSuperHeroesPage</h1>
      <h2>
        <button onClick={refetch}>Get Heroes</button>
        {data.map((hero) => (
          <div key={hero.id}>{hero.name}</div>
        ))}
      </h2>
    </>
  );
};
```

<br>

위의 코드에서 데이터 요청하는 코드를 **Custom hook을 통해 재사용해보자.**

<br>

useSuperHeroesData.js

```jsx
import axios from "axios";
import { useQuery } from "react-query";

function useSuperHeroesData(onSuccess, onError) {
  const { isLoading, data, isError, error, isFetching, refetch } = useQuery(
    "super-heroes",
    () => {
      return axios.get("http://localhost:4000/superheroes");
    },
    {
      enabled: false,
      onSuccess,
      onError,
      refetchOnMount: false,
      select: (data) => {
        return data.data.map((hero) => ({
          name: hero.name,
          id: hero.id,
        }));
      },
    }
  );

  return { isLoading, data, isError, error, isFetching, refetch };
}

export default useSuperHeroesData;
```

<br>

RQSuperHeroes.js

```jsx
import useSuperHeroesData from "../hooks/useSuperHeroesData";

export const RQSuperHeroesPage = () => {
  function onSuccess({ data }) {
    console.log("side effect after data fetching");
  }

  function onError(error) {
    console.log("side effect after error");
  }

  const {
    isLoading,
    data,
    isError,
    error,
    isFetching,
    refetch,
  } = useSuperHeroesData(onSuccess, onError); // <------- 변경된 코드

  if (isLoading || isFetching) {
    return <h2>로딩중...</h2>;
  }

  if (isError) {
    return <h2>{error.message}</h2>;
  }

  return (
    <>
      <h1>RQSuperHeroesPage</h1>
      <h2>
        <button onClick={refetch}>Get Heroes</button>
        {data?.map((hero) => (
          <div key={hero.id}>{hero.name}</div>
        ))}
      </h2>
    </>
  );
};
```

<br>

**이제 Custom Hook을 통해 재사용, 가독성, 코드 관리하기가 쉬워졌다.**

<br>

참고

- [https://react-query.tanstack.com/reference/useQuery](https://react-query.tanstack.com/reference/useQuery)
- [https://www.youtube.com/watch?v=oX3HTT5e9SQ&list=PLC3y8-rFHvwjTELCrPrcZlo6blLBUspd2&index=10](https://www.youtube.com/watch?v=oX3HTT5e9SQ&list=PLC3y8-rFHvwjTELCrPrcZlo6blLBUspd2&index=10)

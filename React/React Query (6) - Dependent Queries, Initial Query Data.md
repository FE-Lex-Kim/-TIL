- [React Query (6) - Dependent Queries, Initial Query Data](#react-query-6---dependent-queries-initial-query-data)
    - [Dependent Queries](#dependent-queries)
  - [**Initial Query Data**](#initial-query-data)
  - [Paginated Queries](#paginated-queries)

# React Query (6) - Dependent Queries, Initial Query Data

<br>

### Dependent Queries

**순차적으로 실행되는 query** 대해 알아보자.

**두 번째 query를 실행하기전에, 첫 번째 요청한 query의 결과에 따라서 실행할지, 안할지를 옵션으로 설정할 수 있다.**

<br>

설명이 다소 어렵게 느껴지는데 예제코드를 보고 이해해보자.

```jsx
const { data: user } = useQuery(["user", email], getUserByEmail); // <--- 1.

const userId = user?.id;

// Then get the user's projects
const { isIdle, data: projects } = useQuery(
  // <--- 2.
  ["projects", userId],
  getProjectsByUser,
  {
    // The query will not execute until the userId exists
    enabled: !!userId,
  }
);
```

<br>

2번 쿼리는 1번 쿼리가 실행되고 **userId가 존재하는지에 따라서 실행되거나 안된다.**

이런 경우는 흔하게 일어난다.

<br>

예를들어

**로그인한뒤, 각각 아이디에 따라 가지고 있는 장바구니 아이템을 불러온다고 하자.**

1. 최초에 **아이디를 입력해** 로그인 요청을 한다.
2. 클라이언트에서 로그인이 된것을 확인되면, **아이디에 대한 장바구니 아이템**을 요청한다.

<br>

즉, 아이디 요청에 따라 장바구니 아이템을 요청한다.

**장바구니 아이템을 요청하는 query는 아이디 요청 query에 의존성이 생기게된다.**

이렇게 순차적으로 발생하는 query를 설정하는 옵션으로 간단하게 구현가능하다.

<br>

예제 코드를 보고 이해해보자.

```jsx
import axios from "axios";
import React from "react";
import { useQuery } from "react-query";

function fetchUserByloginEmail(loginEmail) {
  return axios.get(`http://localhost:4000/users/${loginEmail}`);
}

function fetchCartBycartId(cartId) {
  return axios.get(`http://localhost:4000/carts/${cartId}`);
}

const DependentQueries = ({ loginEmail }) => {
  const { data: user } = useQuery(["users", loginEmail], () =>
    // <---- 1.
    fetchUserByloginEmail(loginEmail)
  );

  const cartId = user?.data.cartId; // <---- 2.

  const { data: cart } = useQuery(
    ["users", cartId],
    () => fetchCartBycartId(cartId),
    {
      enabled: !!cartId, // <--------------- 3.
    }
  );

  return (
    <>
      {cart?.data.foods.map((food) => (
        <h2 key={food}>{food}</h2>
      ))}
    </>
  );
};

export default DependentQueries;
```

<br>

1. loginEmail을 props로 전달받아, **user 데이터를 요청했다.**
2. user 데이터를 응답 받은후, **cartId가 존재하는지에 따라,**
3. cart 데이터를 **요청할지, 안할지를** 결정하게된다.

<br>

query devTools에는 어떻게 보이는지 알아보자.

![Dependent Queries](<../Images/React%20Query%20(6)/React%20Query%20(6)-1.png>)

1. 최초에 cartId가 없지만, 추적해서 query에 저장한다. `["users", null]`
2. 이후 cartId가 존재해 데이터를 다시 요청하고, query에 저장후 브라우저에 표시한다.

<br>

## **Initial Query Data**

캐시에 초기 데이터를 설정하는 방법이있다.

**초기 데이터를 설정해, 로딩 표시를 하지 않게 해서 좀더 나은 UX로 만들 수 있다.**

<br>

사용방법에 대해 알아보자.

예제코드)

```jsx
function Todos() {
  const result = useQuery("todos", () => fetch("/todos"), {
    initialData: initialTodos,
  });
}
```

<br>

위와 같이 useQuery의 세번째 인수에 옵션 객체 프로퍼티에 `**initialData` 프로퍼티를 설정해주면된다.\*\*

캐쉬 query에 최초 데이터를 설정함으로써 loading 상태를 넘길 수 있다.

<br>

이를 좀더 활용하면, **이미 query 캐시에 있는 값을 intialData에 넣는 방법이 있다.**

<br>

예를들면) A 페이지에서 query 캐시를 했다고 하자.

그리고 **B 페이지에서 A 페이지의 query 캐시의 값 중 일부를 중복되서 사용한다면,**

굳이 **서버의 데이터에 요청하지 않고 `initialData` 로 설정하면**, **네트워크 요청도 줄고 로딩 상태도 표시되지 않는 장점이 생긴다.**

<br>

위의 설명을 그대로 코드로 만들어보자.

<br>

아래 코드를 보여주기 앞서 이해하기 쉽게 **미리 페이지에 대한 설명을 하겠다.**

1. **Super-heroes 페이지에서** 몇개의 hero들의 정보들을 가져온다. 이곳에서 각각의 **hero의 링크가 나온다.**

   ![Initial Query](<../Images/React%20Query%20(6)/React%20Query%20(6)-2.png>)

2. **클릭해게 되면 해당하는 영웅의 정보들이 나오게 된다.**

   ![Initial Query](<../Images/React%20Query%20(6)/React%20Query%20(6)-3.png>)

3. 영웅의 정보를 표시하는 페이지를 **SuperHeroesDetail 페이지라고 정했다.**

<br>

예제코드)

```jsx
import axios from "axios";
import React from "react";
import { useQuery, useQueryClient } from "react-query";

const SuperHeroesDetail = ({ heroId }) => {
  const queryClient = useQueryClient();

  // 해당 id에 대한 data 요청
  const { isLoading, data, isError, error } = useQuery(
    ["super-hero", heroId],
    () => {
      return axios.get(`http://localhost:4000/superheroes/${heroId}`);
    },
    {
      initialData: () => {
        const hero = queryClient
          .getQueryData("super-heroes")
          ?.data?.find((hero) => hero.id === Number(heroId));

        if (!!hero) {
          return { data: hero };
        } else {
          return undefined;
        }
      },
    }
  );

  return (
    <>
      <h2>name : {data.data.name}</h2>
      <h2>ID : {data.data.id}</h2>
    </>
  );
};

export default RQSuperHeroesDetail;
```

<br>

위의 코드를 이해하기전 몇 가지 사전 지식을 알아보자.

1. `useQueryClient()` : `useQueryClient`의 반환값은 **현재 `QueryClient`의 인스턴스가 나온다.**
2. `queryClient.getQueryData(queryKey)` : **현재 존재하는 쿼리 캐시를 가져오는데 사용하는 함수이다.** 만약 쿼리가 존재하지 않는다면, `undefiend`를 반환한다. queryKey 인수를 넣어, 그에 해당하는 query 캐시를 가져온다.

<br>

`useQueryClient`를 사용한 이유는 **현재 쿼리 캐시의 값에 접근 하기 위해서 사용했다.**

- 여기 코드에서 `new QueryClient`를 사용하면 안되는 이유는 **이미 App.js에서 queryClient를 생성해서 이곳에서 query 캐시를 저장하기 떄문에** 다른 QueryClient를 생성하면 안된다.

<br>

이제 우리가 자세히 살펴봐야 하는 `initialData` 코드를 이해해보자.

```jsx
const SuperHeroesDetail = ({ heroId }) => {
...
 {
	initialData: () => {
    const hero = queryClient
      .getQueryData("super-heroes")
      ?.data?.find((hero) => hero.id === Number(heroId));

    if (hero) {
      return { data: hero };
    } else {
      return undefined;
    }
  },
 }
}
```

<br>

1. `queryClient.getQueryData("super-heroes")`을 통해 이전 페이지에 사용했던 "**super-heroes" 캐시 값을 불러온다.(이전 페이지에서는 여러가지 영웅들의 정보를 요청해 가져왔었다. 지금 페이지에서는 그 중 하나의 영웅 정보만 불러온다.)**

- super-heroes 캐시 값
  ```json
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
      }
    ],
  ```

1. `?.data` 옵셔널 체이닝 연산자를 통해 앞의 **캐시 값이 존재하지 않으면, undefiend를 반환하게해 런타임 에러를 방지한다.**
2. `?.find((hero) => hero.id === Number(heroId));` : 마찬가지로 런타임 에러를 방지하고, find 메서드를 사용해서 **props로 전달받은 heroId와 super-heroes 캐시 값들중 id가 일치한 값만 가져온다.(예를 들어 만약 props로 전달받은 heroId가 1인 값을 찾아서 가져온다.)**
3. 조건문을 통해 hero가 존재하면, { data : hero }를 반환해준다. **data 프로퍼티가 있는 객체를 반환하는 이유는 일반적으로 요청해서 받는 서버 데이터의 deps와 일치시키기 위함이다.**

   만약 hero가 존재하지 않아면, `undefiend`를 반환해 **런타임 오류를 방지한다.**

   ```jsx
   if (hero) {
     return { data: hero };
   } else {
     return undefined;
   }
   ```

<br>

이렇게 이전에 **방문했던 페이지의 정보중 일부를 가져올 수 있는 방법**에 대해 알아보았다.

**사용자에게 로딩 상태를 보여주지 않아 사용자 경험이 보다 나아진다.**

<br>

## Paginated Queries

**페이지네이션을 사용할때, 보다 사용자 경험을 향상시키는 방법을 제공하는 옵션이 있다.**

로딩 상태가 표시된후, 다음 페이지의 데이터가 보여질때, **레이아웃 시프트가 발생한다는 점이 UX에 나쁘다.**

![pagenation Query](<../Images/React%20Query%20(6)/React%20Query%20(6)-4.gif>)

<br>

**가장 최근에 성공적으로 가져온 데이터를 사용하고 그와 동시에 query key가 변경된 상태여도 새로운 데이터를 요청한다.**

**이후 성공적으로 새로운 데이터를 받아오면, 그때 이전 데이터에서 새로운 데이터로 갈아끼우게된다.**

따라서 **레이아웃 시프트가 발생하지 않는다**는 점에서 사용자 경험을 보다 향상시킨다.

<br>

코드를 사용해보고 어떤 식으로 변하는지 확인해보자.

```jsx
function fetchColors(pageNum) {
  return axios.get(`http://localhost:4000/colors?_limit=2&_page=${pageNum}`);
}

const PagenationPage = () => {
  const [pageNum, setPageNum] = useState(1);
  const { data, isLoading, isFetching } = useQuery(
    ["colors", pageNum],
    () => fetchColors(pageNum),
    {
      keepPreviousData: true,
    }
  );

  if (isLoading) {
    return <h1>로딩중...</h1>;
  }

  return (
    <>
      <div>
        {data.data.map(({ id, color }) => (
          <h2 key={id}>{`${id} : ${color}`}</h2>
        ))}
      </div>
      <button
        onClick={() => setPageNum((page) => page - 1)}
        disabled={pageNum === 1}
      >
        prev
      </button>

      <button
        onClick={() => setPageNum((page) => page + 1)}
        disabled={pageNum === 4}
      >
        next
      </button>
      <p>{isFetching && "로딩중..."}</p>
    </>
  );
};

export default PagenationPage;
```

<br>

위의 코드에서 객체 옵션 값에 `keepPreviousData: true` 을 넣어주었다.

그에따라

1. 다음 페이지를 클릭하더라도, **이전 데이터를 계속 사용하고**
2. **loading 상태 표시가 나오지 않는다.**
3. 이후 완전히 새로운 데이터를 받아오면, 그때 이전 데이터에서 **새로운 데이터로 갈아끼운다.**

![pagenation Query](<../Images/React%20Query%20(6)/React%20Query%20(6)-5.gif>)

<br>

**이전 데이터를 그대로 유지한채, background-data-fetching을 한다.**

따라서 Layout Shift를 발생시키지 않고 페이지 이동이 가능하다.

<br>

loading 상태 표시를 하고싶다면, **background-data-fetching을 하는 중인것을 나타내야 하므로**

**isFetching을 통해 loading 상태 표시 로직을 구성하면된다.**

```jsx
<p>{isFetching && "로딩중..."}</p>
```

<br>

참고

- [https://react-query.tanstack.com/guides/dependent-queries](https://react-query.tanstack.com/guides/dependent-queries)
- [https://react-query.tanstack.com/reference/QueryClient#\_top](https://react-query.tanstack.com/reference/QueryClient#_top)
- [https://react-query.tanstack.com/guides/paginated-queries#better-paginated-queries-with-keeppreviousdata](https://react-query.tanstack.com/guides/paginated-queries#better-paginated-queries-with-keeppreviousdata)
- [https://www.youtube.com/watch?v=yOjHT-oTFww&list=PLC3y8-rFHvwjTELCrPrcZlo6blLBUspd2&index=16&t=2s](https://www.youtube.com/watch?v=yOjHT-oTFww&list=PLC3y8-rFHvwjTELCrPrcZlo6blLBUspd2&index=16&t=2s)

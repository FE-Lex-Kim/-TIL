- [React Query (6) - Dependent Queries, Initial Query Data](#react-query-6---dependent-queries-initial-query-data)
    - [Dependent Queries](#dependent-queries)

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

참고

- [https://react-query.tanstack.com/guides/dependent-queries](https://react-query.tanstack.com/guides/dependent-queries)
- [https://react-query.tanstack.com/reference/QueryClient#\_top](https://react-query.tanstack.com/reference/QueryClient#_top)
- [https://www.youtube.com/watch?v=yOjHT-oTFww&list=PLC3y8-rFHvwjTELCrPrcZlo6blLBUspd2&index=16&t=2s](https://www.youtube.com/watch?v=yOjHT-oTFww&list=PLC3y8-rFHvwjTELCrPrcZlo6blLBUspd2&index=16&t=2s)

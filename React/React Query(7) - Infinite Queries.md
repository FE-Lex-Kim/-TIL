- [React Query(7) - Infinite Queries, useMutation, Query Invalidation](#react-query7---infinite-queries-usemutation-query-invalidation)
  - [Infinite Queries](#infinite-queries)
    - [getNextPageParams](#getnextpageparams)
    - [hasNextPage](#hasnextpage)
    - [fetchNextPage](#fetchnextpage)
    - [data](#data)
  - [useMutation](#usemutation)
  - [Query Invalidation](#query-invalidation)

<br>

# React Query(7) - Infinite Queries, useMutation, Query Invalidation

## Infinite Queries

**무한 스크롤, 추가적으로 데이터를 계속 제공하는 "더 보기" 와 같은 UI 패턴은 흔하게 사용된다.**

React Query는 이 기능 위해 `useQuery` 보다 **더 유용한 버전의 인 `useInfinityQuery` Hook을 제공한다.**

<br>

```jsx
function fetchColors(parameter) {
  const { pageParam = 1 } = parameter;
  return axios.get(`http://localhost:4000/colors?_limit=2&_page=${pageParam}`);
}

const {
  data,
  isLoading,
  isError,
  error,
  hasNextPage,
  fetchNextPage,
} = useInfiniteQuery(["colors"], fetchColors, {
  getNextPageParam: (lastPage, allPage) => {
    if (allPage.length < 4) {
      return allPage.length + 1;
    } else {
      return undefined;
    }
  },
});
```

<br>

`useInfinityQuery`를 사용할때, **알아야하는 몇 가지가 있다.**

<br>

### getNextPageParams

**hasNextPage, fetchNextPage을 관리하는데 사용하는 함수이다.**

반환되는 값에 따라 영향을 주게 된다.

<br>

**새로운 데이터를 요청한뒤 받을 때마다, 매번 사용되는 2개의 인자는 업데이트 된다.**

1. 첫 번째 인자는 **가장 최근에 요청한 페이지의 데이터이다.**
2. 두 번째 인자는 **지금까지 요청한 페이지의 모든 데이터 값이 배열로 들어가 있다.**

```jsx
getNextPageParam: (lastPage, allPage) => {
   if (allPage.length < 4) {
     return allPage.length + 1;
   } else {
     return undefined;
   }
},
```

<Image alt="Infinity Queries" src="../Images/React%20Query(7)/React%20Query(7)-1.png" width=200 />

<br>

<Image alt="Infinity Queries" src="../Images/React%20Query(7)/React%20Query(7)-2.png" width=500 />

<br>

<Image alt="Infinity Queries" src="../Images/React%20Query(7)/React%20Query(7)-3.png" width=500 />

<br>

<br>

### hasNextPage

**다음 페이지에 불러올 데이터가 있는지, 없는지 알 수 있는 값이다.**

<br>

```jsx
const InfinityQueryPage = () => {
	const { ..., hasNextPage } =
	useInfiniteQuery(
		["colors"],
		fetchColors,
		{
	    getNextPageParam: (lastPage, allPage) => {
		      if (allPage.length < 4) {
		      return allPage.length + 1;
		    } else {
		      return undefined;
		    }
		  },
		}
	);
	...
	return (
		...
		<button onClick={fetchNextPage} disabled={!hasNextPage}>
      더 보기
    </button>
	)
}
```

getNextPageParams의 **반환되는 값이**

- **undefined 이면, hasNextPage는 false 값을 가지고**
  - **더 이상 불러올 페이지가 없는, 현재가 마지막 페이지라는 의미이다.**
- **undefiend을 제외한 나머지 값이 반환되면, true 값을 가진다.**
  - **다음 페이지에 불러올 데이터가 더 있다는 의미이다.**

<br>

**마지막 페이지 일때, 더 이상 요청할 데이터가 없다면, '더 보기' 버튼을 disabled 시킬때 사용한다.**

<Image alt="Infinity Queries" src="../Images/React%20Query(7)/React%20Query(7)-4.png" width=300 />

<br>

### fetchNextPage

**다음 페이지를 불러올때 사용하는 함수이다.**

<br>

**`getNextPageParams`의 return 값이 데이터 요청 함수의 인자(Parameter)인 `pageParam`의 값으로 들어간다.**

**중요한 점은 위의 과정을 실행하게 하려면, `fetchNextPage`을 호출해야한다.**

```jsx
function fetchColors(parameter) {
  console.log("parameter: ", parameter);
  const { pageParam = 1 } = parameter;
  return axios.get(`http://localhost:4000/colors?_limit=2&_page=${pageParam}`);
}

const InfinityQueryPage = () => {
	const { ..., fetchNextPage } =
	useInfiniteQuery(
		["colors"],
		fetchColors,
		{
	    getNextPageParam: (lastPage, allPage) => {
		      if (allPage.length < 4) {
		      return allPage.length + 1;
		    } else {
		      return undefined;
		    }
		  },
		}
	);
	...
	return (
		...
		<button onClick={fetchNextPage} disabled={!hasNextPage}>
      더 보기
    </button>
	)
}
```

<Image alt="Infinity Queries" src="../Images/React%20Query(7)/React%20Query(7)-5.png" width=500 />

<br>

코드 해석을 조금 하자면,

1. 현재 `allPage.length` 값은 `1`이다.
2. `fetchNextPage` 함수가 호출되고,
3. 조건문에 의해 반환되는 값인 `allPage.length + 1` 는 즉, `2`가 된다.
4. `pageParam` 이 `2`가 된 값으로 데이터를 요청하게 된다.

<br>

### data

`useInfinityQuery`의 **반환 값인 `data`는 infinity query를 위한 여러가지 기능을 포함한 객체이다.**

**일반적인 `useQuery`가 반환하는 `data`와 다르다.**

<br>

```jsx
const { ..., data } = useInfiniteQuery(...)
```

<Image alt="Infinity Queries" src="../Images/React%20Query(7)/React%20Query(7)-6.png" width=500 />

<br>

`data.pages` 는 현재까지 요청한 **모든 페이지의 데이터이다.**

`data.pageParams` 는 지금까지 `getNextPageParams` 함수의 **반환된 값이 배열로 들어간다.**

<br>

지금 배운것을 바탕으로 코드를 만든다면,

```jsx
import axios from "axios";
import React, { Fragment } from "react";
import { useInfiniteQuery } from "react-query";

function fetchColors({ pageParam = 1 }) {
  return axios.get(`http://localhost:4000/colors?_limit=2&_page=${pageParam}`);
}

const InfinityQueryPage = () => {
  const {
    data,
    isLoading,
    isError,
    error,
    hasNextPage,
    fetchNextPage,
  } = useInfiniteQuery(["colors"], fetchColors, {
    getNextPageParam: (_lastPage, allPage) => {
      if (allPage.length < 4) {
        return allPage.length + 1;
      } else {
        return undefined;
      }
    },
  });

  if (isLoading) {
    return <h1>로딩중...</h1>;
  }

  if (isError) {
    return <h1>{error.message}</h1>;
  }

  return (
    <>
      <div>
        {data?.pages.map(({ data }, i) => {
          return (
            <Fragment key={i}>
              {data.map(({ id, label }) => (
                <h2 key={id}>{`${id} ${label}`}</h2>
              ))}
            </Fragment>
          );
        })}
      </div>
      <button onClick={fetchNextPage} disabled={!hasNextPage}>
        더 보기
      </button>
    </>
  );
};

export default InfinityQueryPage;
```

<br>

## useMutation

**useMutation은 서버 데이터를 추가, 업데이트, 삭제하는 기능으로 사용한다.**

<br>

useMutation 반환 값을 통해 데이터 요청, 상태 등등.. 을 알 수 있다.

```jsx
const addFavoriteColor = (color) => {
  return axios.post("http://localhost:4000/colors", color);
};

const FavoriteColor = () => {
  const { mutate, isLoading, isError, error } = useMutation(addFavoriteColor);

	...

  function onClick() {
    mutate(color);
  }

	...

	if(isLoading){
		return <h2>데이터 추가하는중..</h2>
	}

	if (isError) {
    return <h2>{error.message}</h2>;
  }

  return (
    <>
      <div>
				...
        <button onClick={onClick}>추가하기</button>
				...
      </div>
    </>
  );
};

export default FavoriteColor;
```

<br>

**mutate**

- `useMutation(RequestDataFn)` 인자에 전달한 **데이터 요청 함수를 실행시키는 함수이다.**
- 만약 데이터 요청 함수에 **인수를 전달하고 싶으면, mutate의 인수에 전달 하면된다.**

<br>

**isLoading**

- useMutation **데이터 요청 함수가 진행 중** 이라면 `true`, **아니라면** `false` 값이 나온다.

<br>

**isError, error**

- **에러가 발생한것에 따라** `isError` 이 Boolean 값으로 나온다.
- `error`는 **에러가 발생한 정보**가 들어가 있다.

<br>

**isSuccess**

- **성공적으로 데이터 요청을 했다면**, `true`, 아니라면 `false` 값이 나온다.

<br>

**data**

- **데이터 요청 한후 서버에서, 응답한 데이터 값이다.**
- <Image alt="Infinity Queries" src="../Images/React%20Query(7)/React%20Query(7)-6.png" width=500 />

<br>

## Query Invalidation

**사용자가 서버 데이터를 추가하거나, 삭제, 수정한 경우, 화면에 보여지는 데이터들은 다시 변경되어야한다.**

**하지만 query가 변경되지 않으므로, 강제로 다시 refetch 해주어야한다.**

<br>

이럴때, `queryClient.invalidateQueries(queryKey)` 을 사용하면된다.

```jsx
const { data } = useQuery("colors", fetchFavoriteColor);
const { mutate, isLoading, isError, error } = useMutation(addFavoriteColor, {
  onSuccess: () => {
    queryClient.invalidateQueries(["colors"]);
  },
});

mutate(color);
```

<br>

`useMutation` 옵션 객체중 **`onSuccess`는 데이터 요청 함수가 성공했을때, 호출된다.**

`queryClient.invalidateQueries(["colors"])`을 호출해서 **성공한 직후, refetch 하게 해주었다.**

<br>

인자로 들어가는 값은 **refetch 하고 싶은 query의 key를 넣어주면된다.**

해당 query는 다시 refetch 하게된다.

<br>

**주의할 점은, 해당 query key를 가지고 있는 다른 query들도 모두 refetch 한다는 점이다.**

```jsx
queryClient.invalidateQueries("colors");

// 아래의 두 query는 모두 refetch 되어진다.
const res1 = useQuery("colors", addFavoriteColor);
const res2 = useQuery(["colors", { page: 1 }], addFavoriteColor);
```

<br>

**특정한 query를 구체적으로 refetch 하는 방법이 있다.**

```jsx
queryClient.invalidateQueries(["colors", { page: 1 }]);

// res2 query만 refetch 되어진다.
const res1 = useQuery("colors", addFavoriteColor);
const res2 = useQuery(["colors", { page: 1 }], addFavoriteColor);
```

<br>

**정확하게 해당 query key만 가지고 있는 query을 refetch 하는 방법도 있다.**

```jsx
queryClient.invalidateQueries("colors", { exact: true });

// res2 query만 refetch 되어진다.
const res1 = useQuery("colors", addFavoriteColor);
const res2 = useQuery(["colors", { page: 1 }], addFavoriteColor);
```

<br>

참고

- [https://react-query.tanstack.com/guides/infinite-queries](https://react-query.tanstack.com/guides/infinite-queries)
- [https://react-query.tanstack.com/reference/useInfiniteQuery#\_top](https://react-query.tanstack.com/reference/useInfiniteQuery#_top)
- [https://react-query.tanstack.com/reference/useMutation#\_top](https://react-query.tanstack.com/reference/useMutation#_top)
- [https://react-query.tanstack.com/guides/query-invalidation](https://react-query.tanstack.com/guides/query-invalidation)
- [https://www.youtube.com/watch?v=s92apk05kT4&list=PLC3y8-rFHvwjTELCrPrcZlo6blLBUspd2&index=20](https://www.youtube.com/watch?v=s92apk05kT4&list=PLC3y8-rFHvwjTELCrPrcZlo6blLBUspd2&index=20)
- [https://www.youtube.com/watch?v=NYCG1o38oEQ&list=PLC3y8-rFHvwjTELCrPrcZlo6blLBUspd2&index=21](https://www.youtube.com/watch?v=NYCG1o38oEQ&list=PLC3y8-rFHvwjTELCrPrcZlo6blLBUspd2&index=21)

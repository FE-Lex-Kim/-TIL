- [React Query 튜토리얼(5) - Query Id, useQuery 여러번 호출](#react-query-튜토리얼5---query-id-usequery-여러번-호출)
  - [Query Id](#query-id)
  - [useQuery 여러번 호출](#usequery-여러번-호출)

# React Query 튜토리얼(5) - Query Id, useQuery 여러번 호출

<br>

## Query Id

**데이터를 불러올때, id를 통해 불러오는 경우가 많다.**

보통 **URL에 path 또는 query에 특정 정보에 대한 id를 할당해, 데이터 GET 요청시 사용된다.**

이번에는

- 각각의 **URL Path id를 통해, 데이터를 불러오고**
- 데이터를 캐싱 한뒤, 저장할때 **등록한 Query key를 통해 캐시를 사용하는 방법**에 대해 알아보자.

<br>

예시를 보고 이해해보자.

먼저 db.json를 확인해보자.

```jsx
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

각각의 페이지를 들어갈때마다, **마지막의 URL Path는 해당 페이지에 필요한 데이터의 id가 들어가게된다.**

![React Query Id](<../Images/React%20Query%20튜토리얼(5)/React%20Query%20튜토리얼(5)-1.png>)

<br>

1. Batman 페이지로 들어가 보자.

![React Query Id](<../Images/React%20Query%20튜토리얼(5)/React%20Query%20튜토리얼(5)-2.png>)

![React Query Id](<../Images/React%20Query%20튜토리얼(5)/React%20Query%20튜토리얼(5)-3.png>)

<br>

- Batman을 들어가게 되면, 실제 데이터를 요청할때 필요한 **id가 URL Path의 마지막**에 들어가게 설정해놨다.

<br>

SuperHeroesDetail.js(각각 SuperHeroe 정보 페이지)

```jsx
const RQSuperHeroesDetail = () => {
  // usePath id 값져와서
  const { heoreId } = useParams();

  // 해당 id에 대한 data 요청
  const { isLoading, data, isError, error } = useQuery(
    ["super-heroe", heoreId],
    () => {
      return axios.get(`http://localhost:4000/superheroes/${heoreId}`);
    }
  );

  if (isLoading) {
    return <h2>로딩중...</h2>;
  }

  if (isError) {
    return <h2>{error.message}</h2>;
  }
  return (
    <>
      <h2>{data.data.name}</h2>
    </>
  );
};

export default RQSuperHeroesDetail;
```

<br>

위의 코드에서 특이한 점은 useQuery의 첫 번째 Argument인 **Query key가 배열인 점이다.**

배열로 존재하게 해, **Query key의 depth를 깊게 만들어 페이지별로 고유한 Query key로 만들었다.**

- 위와 같이 만든다면, Query key는 다음과 같이 나온다.
  ![React Query Id](<../Images/React%20Query%20튜토리얼(5)/React%20Query%20튜토리얼(5)-4.png>)

- 각각 페이지 별로 id가 고유하므로, **캐시 값을 찾을때 중복될 일이 없어지게된다.**
- 즉, **유일무이한 Query key를 만들기 위해 데이터 id를 사용한것이다.**

<br>

또 다른 방법으로 데이터 요청 함수에 **queryKey를 파라미터로 받을 수 있다.**

```jsx
const { heoreId } = useParams();

// 해당 id에 대한 data 요청
const { isLoading, data, isError, error } = useQuery(
  ["super-hero", heoreId],
  ({ queryKey }) => {
    const id = queryKey[1];
    return axios.get(`http://localhost:4000/superheroes/${id}`);
  }
);
```

queryKey는 **캐시 key 배열이 들어가게된다.(`['supuer-hero", "1"]`)**

`queryKey[1]` 는 데이터에 대한 **id**이므로, 그대로 Get 요청할때 사용하는 URL에 입력해 사용할 수 있다.

<br>

**최초 요청시에 Query Key가 없어도, Query Key를 등록하고 요청하기 때문에 최초 요청시 상관없다.**

1. 최초 요청시 **Query Key가 비어있음.**
2. 없는 것을 파악한 즉시, **Query Key를 등록함**
   - **`['supuer-hero", "1"]`**
3. 데이터 요청 함수를 실행함

   ```jsx
   ({ queryKey }) => {
     const id = queryKey[1];
     return axios.get(`http://localhost:4000/superheroes/${id}`);
   };
   ```

   - 따라서 `queryKey[1]` 값은 **최초 요청시에도 항상 들어 있음**

<br>

## useQuery 여러번 호출

**하나의 컴포넌트에서 데이터 요청을 여러번 하는 경우도 있다.**

React Query에서 굉장히 쉽게 구현할 수 있다.

<br>

useQuery를 통해 데이터를 호출하고 반환하는 **data 변수 [이름을 alias를 통해 변경](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#%EC%83%88%EB%A1%9C%EC%9A%B4_%EB%B3%80%EC%88%98_%EC%9D%B4%EB%A6%84%EC%9C%BC%EB%A1%9C_%ED%95%A0%EB%8B%B9%ED%95%98%EA%B8%B0)시키면된다.**

예제를 보고 이해해보자.

```jsx
const { data: friends } = useQueries("friends", () => {
  return axios.get("http://localhost:4000/friends");
});

const { data: superheroes } = useQueries("superheroes", () => {
  return axios.get("http://localhost:4000/superheroes");
});
```

<br>

위와 같이 각각 반환되는 **data 변수 이름을 새로운 변수 이름으로 변경해 사용하면된다.**

<br>

참고

- [https://react-query.tanstack.com/reference/useQuery](https://react-query.tanstack.com/reference/useQuery)
- [https://www.youtube.com/watch?v=2s2iJLLDwgk&list=PLC3y8-rFHvwjTELCrPrcZlo6blLBUspd2&index=14](https://www.youtube.com/watch?v=2s2iJLLDwgk&list=PLC3y8-rFHvwjTELCrPrcZlo6blLBUspd2&index=14)

- [React Query 튜토리얼(2) - React Query 캐시](#react-query-튜토리얼2---react-query-캐시)
  - [React Query 캐시](#react-query-캐시)
  - [캐싱 process](#캐싱-process)
  - [Background re-fetching](#background-re-fetching)
  - [isFetching](#isfetching)
    - [1. **최초 데이터 요청**](#1-최초-데이터-요청)
    - [2. **재요청으로 인한, 캐싱된 데이터 사용**](#2-재요청으로-인한-캐싱된-데이터-사용)
  - [캐시 유효시간 변경](#캐시-유효시간-변경)
  - [장점](#장점)

# React Query 튜토리얼(2) - React Query 캐시

[**React Query 튜토리얼(1)**](<https://github.com/FE-Lex-Kim/-TIL/blob/master/React/React%20Query%20%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC(1)%20-%20%ED%99%98%EA%B2%BD%20%EC%84%A4%EC%A0%95%2C%20useQuery%2C%20handle%20Error%2C%20devtools.md#%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%9E%AC%EC%9A%94%EC%B2%AD>)에 이어서 공부해보자.

<br>

## React Query 캐시

**React Query의 가장 강력한 기능이라고 하면, 서버로 부터 불러온 데이터를 자동으로 캐싱하는 기능일 것이다.**

캐싱을 하기위해 복잡한 로직을 작성할 필요가 없이, **단순히 useQuery를 통해 데이터를 불러오기만 하면 된다.**

<br>

예제를 보고 공부해보자.

아래의 예제는 2**개의 페이지가 있다.**

- home
- RQSuperHeroes

<br>

App.js

```jsx
function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <ul>
        <li>
          <Link to="/">Home</Link>
        </li>
        <li>
          <Link to="/rq-super-heroes">RQ Super Heroes</Link>
        </li>
      </ul>
    </QueryClientProvider>
  );
}

export default App;
```

<br>

이 중에 RQSuperHeroes 페이지 코드만 참고해보자.(home 페이지는 `<h1>home<h1>` 밖에 없다.)

RQSuperHeroesPage.js

```jsx
const fetchSuperHeroes = () => {
  return axios.get("http://localhost:4000/superheroes");
};

export const RQSuperHeroesPage = () => {
  const { isLoading, data, isError, error } = useQuery(
    "super-heroes",
    fetchSuperHeroes
  );

  if (isLoading) {
    return <h2>로딩중...</h2>;
  }

	...code

  return (
    <>
      <h1>RQSuperHeroesPage</h1>
      <h2>
        {data?.data.map((hero) => (
          <div key={hero.id}>{hero.name}</div>
        ))}
      </h2>
    </>
  );
};
```

<br>

먼저,

1. **RQSuperHeroes 페이지**를 방문하고
2. **home**으로 이동한뒤
3. **RQSuperHeroes** **페이지로 재방문할 예정이다.**

<br>

**위의 프로세스를 단계별로 실행하면서, 캐싱이 되는 것을 직접확인해보자.**

1. **RQSuperHeroes 페이지**를 방문

   a. **최초에** 해당 페이지를 방문시, **데이터를 요청하는 사이에 Loading 중임을 나타내는 Text가 보여진다.**

   ![React Query 튜토리얼](<../Images/React%20Query%20튜토리얼(2)%20-%20React%20Query%20캐시/React%20Query%20튜토리얼(2)%20-%20React%20Query%20캐시-1.png>)

   b. **이후에 데이터를 응답받아**, Batman, Superman, Wonder Woman을 브라우저에 보여줬다.

   ![React Query 튜토리얼](<../Images/React%20Query%20튜토리얼(2)%20-%20React%20Query%20캐시/React%20Query%20튜토리얼(2)%20-%20React%20Query%20캐시-2.png>)

<br>

2. **home**으로 이동한뒤

   ![React Query 튜토리얼](<../Images/React%20Query%20튜토리얼(2)%20-%20React%20Query%20캐시/React%20Query%20튜토리얼(2)%20-%20React%20Query%20캐시-3.png>)

<br>

3. **RQSuperHeroes** **페이지로 재방문했다.**

   ![React Query 튜토리얼](<../Images/React%20Query%20튜토리얼(2)%20-%20React%20Query%20캐시/React%20Query%20튜토리얼(2)%20-%20React%20Query%20캐시-2.png>)

- **최초에 RQSuperHeroes 페이지를 방문했을때랑 다르게, Loading Text가 나타나지 않는다.**

  → **참고) 캐싱된 데이터를 사용한다면, isLoading은 항상 false 값을 가진다.**

<br>

위와 같은 상황처럼, **후속요청을 방지하기위해 default로 5분동안 유효하는 캐싱 기능을 넣었다.**

이런 캐싱 기능을 통해 후속요청을 방지해 **로딩 성능이 크게 향상된다.**

<br>

그렇다면, 이런 **캐싱되는 과정은 어떻게 진행되는 것일까?**

<br>

## 캐싱 process

1. **요청이 완료되면 데이터를 가져온다.**
2. useQuery의 **첫 번째 인수로 입력한 string을 키로 등록하고 가져온 데이터를 캐싱한다.**

   ```jsx
   const { isLoading, data, isError, error } = useQuery(
       "super-heroes",
   		() => {
   			return axios.get("http://localhost:4000/superheroes"
   		},
   );
   ```

3. 이후 **재요청을 하는 상황이 온다면,**

   a. **재요청을 하기전에 해당 키가 캐시에 존재하는지 확인한다.**

   ![React Query 튜토리얼](<../Images/React%20Query%20튜토리얼(2)%20-%20React%20Query%20캐시/React%20Query%20튜토리얼(2)%20-%20React%20Query%20캐시-4.png>)

   b. **존재한다면 해당 캐시를 사용한다.**

<br>

**하지만!(중요)**

**캐싱을 했지만, 이후에 실제 서버의 데이터가 업데이트 되었다면 어떻게 될까?**

**재요청시 이미 캐싱된 데이터를 사용하게되고, 업데이트된 서버의 데이터와 다르게 구식의 데이터를 사용하게 된다.**

<br>

useQuery는 이런 일을 **대비해 좋은 기능을 내장하고 있다.**

<br>

## Background re-fetching

위와 같은 상황을 어떻게 **해결하는지 과정을 알아보자.**

아래의 과정은 캐싱된 데이터를 사용한 직후, 항상 발생한다.

<br>

1. **먼저 캐싱된 데이터를 가져와 브라우저에 보여준다.**
2. **암묵적으로 다시 서버에 재요청을해 데이터를 가져온다.**
3. 만약 가져온 **데이터가 변경되었었다면, 캐시를 업데이트하고 UI도 같이 변경된다.**
   - 만약 변경되지 않았다면, 캐시를 변경하지않고 UI도 그대로 놔둔다.

<br>

## isFetching

이제 다른 의문이 생긴다.

**암묵적으로 다시 서버에 요청했기 때문에, 서버에 요청했는지 안했는지 알 수 없다.**

<br>

isLoading을 통해 요청했는지, 안했는지 알려고 했다.

하지만 **캐싱된 데이터를 사용한다면, isLoading은 true로 설정되지 않는다.**

<br>

**이것을 위한것이 isFetching 이다.**

**isFetching은 데이터 요청시 true 값으로 변경된다.**

이제 network 흐름을 추적하기에 도움이된다.

<br>

우리는 2가지 상황에 따라 **isFetching이 어떻게 변하는지 확인해보자.**

1. **최초 데이터 요청**
   1. RQSuperHeroes 페이지를 최초 방문.
2. **재요청으로 인한, 캐싱된 데이터 사용**
   - home 페이지를 들렸다가, **다시 RQSuperHeroes 페이지 재방문 했다고 해보자.**

<br>

```jsx
const { isLoading, data, isError, error, isFetching } = useQuery(
  "super-heroes",
  fetchSuperHeroes
);

console.log({ isLoading, isFetching });
```

<br>

### 1. **최초 데이터 요청**

1. ![React Query 튜토리얼](<../Images/React%20Query%20튜토리얼(2)%20-%20React%20Query%20캐시/React%20Query%20튜토리얼(2)%20-%20React%20Query%20캐시-1.png>)

2. ![React Query 튜토리얼](<../Images/React%20Query%20튜토리얼(2)%20-%20React%20Query%20캐시/React%20Query%20튜토리얼(2)%20-%20React%20Query%20캐시-2.png>)

- ![React Query 튜토리얼](<../Images/React%20Query%20튜토리얼(2)%20-%20React%20Query%20캐시/React%20Query%20튜토리얼(2)%20-%20React%20Query%20캐시-5.png>)

<br>

- **최초 데이터 요청 시** (RQSuperHeroes 페이지를 최초 방문)
  - **isLoading, isFetching 모두 true로 설정되었다.**
  - 따라서 최초에 **isLoading Text가 표시된다.**
  - 데이터 요청했기 때문에, **isFetching 마찬가지로 true로 설정됬다.**
- **데이터 요청이 완전히 완료된 직후,**
  - **isLoading, isFetching 모두 false로 설정되었다.**

<br>

### 2. **재요청으로 인한, 캐싱된 데이터 사용**

1. ![React Query 튜토리얼](<../Images/React%20Query%20튜토리얼(2)%20-%20React%20Query%20캐시/React%20Query%20튜토리얼(2)%20-%20React%20Query%20캐시-2.png>)

2. ![React Query 튜토리얼](<../Images/React%20Query%20튜토리얼(2)%20-%20React%20Query%20캐시/React%20Query%20튜토리얼(2)%20-%20React%20Query%20캐시-6.png>)

- ![React Query 튜토리얼](<../Images/React%20Query%20튜토리얼(2)%20-%20React%20Query%20캐시/React%20Query%20튜토리얼(2)%20-%20React%20Query%20캐시-7.png>)

<br>

- **재요청으로 인한, 캐싱된 데이터 사용** (home 페이지 방문후, 다시 RQSuperHeroes 페이지 재방문)
  - **캐싱되어 있는 데이터를 사용했기 때문에, isLoading은 그대로 false 유지**
  - **캐싱된 데이터를 사용했지만, 암묵적으로 재요청을 했으므로, isFetching은 true로 설정되었다.**
  - 위의 사진을 다시 보면, **처음에는 캐싱되었을때의 데이터를 사용했기 때문에, Batman으로 나왔지만,**
  - **이후 다시 암묵적 재요청후, 서버에서 업데이트 된 데이터인 Batman123으로 변경된것을 볼 수 있다.**
- **데이터 요청이 완전히 완료된 직후**
  - **isLoading, isFetching 모두 false로 설정되었다.**

<br>

## 캐시 유효시간 변경

**캐싱된 값의 유효기간은 5분이다.**

해당시간후 GarbageCollector에 의해 삭제된다.

만약 그 값을 변경하고 싶다면, **useQuery의 3번째 Argument를 추가해주면된다.**

- **객체안의 프로퍼티로 cacheTime의 값을 원하는 유효기간으로 설정하면된다. 해당 값은 ms 단위이다.**

```jsx
const { isLoading, data, isError, error, isFetching } = useQuery(
  "super-heroes",
  fetchSuperHeroes,
  {
    cacheTime: 5000,
  }
);
```

<br>

## 장점

- **미리 캐싱된 데이터로 UI가 표시된후, 암묵적 요청으로 업데이트된 데이터로 UI가 보여주므로, 사용자 경험이 향상된다.**
  - **유저는 로딩 표시를 매번 보지 않고 지루하지 않게 먼저 UI를 볼 수 있다.**
- **캐싱된 유효기간을 Custom 할 수 있다. default 값인 5분도 충분히 다시 후속요청을 방지하기 위한 좋은 시간이다.**

<br>

참고

- [https://react-query.tanstack.com/overview](https://react-query.tanstack.com/overview)
- [https://www.youtube.com/watch?v=Ev60HKYFM0s&list=PLC3y8-rFHvwjTELCrPrcZlo6blLBUspd2&index=3](https://www.youtube.com/watch?v=Ev60HKYFM0s&list=PLC3y8-rFHvwjTELCrPrcZlo6blLBUspd2&index=6)

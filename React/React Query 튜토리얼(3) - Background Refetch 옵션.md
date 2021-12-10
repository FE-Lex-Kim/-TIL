- [React Query 튜토리얼(3) - Background Refetch 옵션](#react-query-튜토리얼3---background-refetch-옵션)
  - [Stale Time](#stale-time)
    - [예제](#예제)
    - [1. RQSuperHeroes 페이지를 최초 방문한다.](#1-rqsuperheroes-페이지를-최초-방문한다)
    - [2. home 페이지 방문](#2-home-페이지-방문)
    - [3. 다시 RQSuperHeroes 페이지를 방문](#3-다시-rqsuperheroes-페이지를-방문)
    - [4. RQSuperHeroes 페이지에서 30초 기다린후](#4-rqsuperheroes-페이지에서-30초-기다린후)
  - [refetchOnMount](#refetchonmount)
  - [refetchOnWindowFocus](#refetchonwindowfocus)
    - [일반적인 비동기 요청 방식](#일반적인-비동기-요청-방식)
    - [useQuery 사용시](#usequery-사용시)
  - [자동 refetch](#자동-refetch)
    - [refetchInterval](#refetchinterval)
    - [refetchIntervalInBackground](#refetchintervalinbackground)

# React Query 튜토리얼(3) - Background Refetch 옵션

반드시 React Query **캐시에 대한 이해가 완벽하게 된 이후 공부하자.**

<br>

- 아직 잘 모른다면, [**React Query 튜토리얼(2)**](<https://github.com/FE-Lex-Kim/-TIL/blob/master/React/React%20Query%20%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC(2)%20-%20React%20Query%20%EC%BA%90%EC%8B%9C.md>)을 공부해보자.

<br>

**Background Refetch에 대한 여러 유용한 옵션들이 있다.**

※ 앞으로 이 글에서 사용하는 용어중 **Background-refetching 에 대해 간단하게** **설명하겠다.**

- 브라우저가 **암묵적으로 해당 캐시에 대한 데이터를 재요청**하는 것을 의미한다.
- 이 기능으로써, **캐시가 서버의 데이터와 같게, 항상 최신으로 유지하게 만드는 방법이다.**
- 자세한 내용은 튜토리얼(2)에서.

<br>

## Stale Time

서버에서 불러온 데이터를 캐싱을 한다면, **다시 해당 페이지를 방문했을때, 캐시에 대한 값을 사용한다.**

따라서 유저는 **Loading 표시가 나타나지 않아 사용자 경험이 크게 향상된다.**

<br>

**캐시를 사용하면서 매번 Background-refetching을 한뒤, 변경 사항이 있으면 그때 UI를 변경한다.**

따라서 **매번 네트워크 요청이 발생한다는 점이 있다.**

<br>

**하지만!**

만약 **서버 데이터가 자주 변경되지 않는다는 점을 개발자가 알고있다면, 굳이 매번 Background-refetching을 할 필요가 없다.**

<br>

이러한 기능을 **'Stale Time'** 이라고 부르는 옵션으로 **Background-refetching 시간 조정이 가능하다.**

<br>

먼저 Stale 이란 의미를 네이버 사전을 통해보자.

- 1.신선하지 않은, (만든 지) 오래된

<br>

Stale Time을 캐시에 대해 의미를 부여하면, **캐시가 얼마나 오래됬는지를 말한다.**

<br>

**Stale Time을 30초로 설정하면,**

- **캐시로 저장한지 30초가 지나고, 캐시가 오래됬다고 판단해 Background re-fetching(암묵적 요청)을 한다.**
- 다시 말하면, **캐시로 저장한 이후, 30초 동안은 Fresh 하다고 판단하고 Background re-fetching 하지 않는다.**

<br>

useQuery 세번째 Argument에 `statleTime` 프로퍼티 속성을 추가해주면된다.

<br>

**사용방법**

```jsx
const { isLoading, data, isError, error, isFetching } = useQuery(
  "super-heroes",
  () => {
    return axios.get("http://localhost:4000/superheroes");
  },
  {
    staleTime: 30000,
  }
);
```

<br>

### 예제

직접 예제를 보면서 **staleTime이 어떻게 동작하는지 이해 해보자.**

<br>

아래의 예제는 2**개의 페이지가 있다.**

- home
- RQSuperHeroes

<br>

우리는 다음 과정을 보면서, **staleTime에 따른 background-refetching이 어떻게 동작하는지 알아보자.**(**우리는 staleTime을 30s로 맞춘 상태이다.)**

1. RQSuperHeroes 페이지를 **최초 방문한다.**
2. 이후 home 페이지 방문한다.
3. **다시** RQSuperHeroes 페이지를 **방문한다.**
4. **30초를** 해당 페이지에서 **기다린다.**

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

RQSuperHeroesPage.js

```jsx
export const RQSuperHeroesPage = () => {
  const { isLoading, data, isFetching } = useQuery(
    "super-heroes",
    () => {
      return axios.get("http://localhost:4000/superheroes");
    },
    {
      staleTime: 30000,
    }
  );

  console.log({ isLoading, isFetching });

  if (isLoading) {
    return <h2>로딩중...</h2>;
  }

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

### 1. RQSuperHeroes 페이지를 최초 방문한다.

- **서버에 데이터 요청이 완료된 직후, 캐싱을 한다.**

  ![React Query Stale Time](<../Images/React%20Query%20튜토리얼(3)%20-%20Background%20Refetch%20옵션/React%20Query%20튜토리얼(3)%20-%20Background%20Refetch%20옵션-1.png>)

<br>

- **최초 요청이기 때문에, isLoading, isFetching이 true이다.**

  ![React Query Stale Time](<../Images/React%20Query%20튜토리얼(3)%20-%20Background%20Refetch%20옵션/React%20Query%20튜토리얼(3)%20-%20Background%20Refetch%20옵션-2.png>)

<br>

- 우리가 눈여겨 봐야할점은 **캐시된 'super-heroes' 상태가 초록색 fresh 설정되어 있다는점이다.**
  - 이제 해당 캐시는 **30초 동안 fresh 하다고 판단한다.**
  - **다시 해당 페이지를 방문한다고 해도 캐시를 사용하고**
  - **background-refetching을 하지 않는다.**
    ![React Query Stale Time](<../Images/React%20Query%20튜토리얼(3)%20-%20Background%20Refetch%20옵션/React%20Query%20튜토리얼(3)%20-%20Background%20Refetch%20옵션-3.png>)

<br>

### 2. home 페이지 방문

![React Query Stale Time](<../Images/React%20Query%20튜토리얼(3)%20-%20Background%20Refetch%20옵션/React%20Query%20튜토리얼(3)%20-%20Background%20Refetch%20옵션-4.png>)

<br>

home 페이지 방문 이후에, 다시 RQSuperHeroes 페이지를 방문해보자.

<br>

### 3. 다시 RQSuperHeroes 페이지를 방문

<br>

Loading Text가 브라우저에 표시되지 않는다.

바로 캐싱된 데이터를 사용한다.

![React Query Stale Time](<../Images/React%20Query%20튜토리얼(3)%20-%20Background%20Refetch%20옵션/React%20Query%20튜토리얼(3)%20-%20Background%20Refetch%20옵션-5.png>)

<br>

![React Query Stale Time](<../Images/React%20Query%20튜토리얼(3)%20-%20Background%20Refetch%20옵션/React%20Query%20튜토리얼(3)%20-%20Background%20Refetch%20옵션-6.png>)

<br>

**'super-heroes' 키는 여전히 fresh 상태이기 때문에, 해당 캐시는 최신상태인것을 의미해서 background-refetching을 하지않는다.**

따라서 isLoading, isFetching의 값이 모두 false이다.

![React Query Stale Time](<../Images/React%20Query%20튜토리얼(3)%20-%20Background%20Refetch%20옵션/React%20Query%20튜토리얼(3)%20-%20Background%20Refetch%20옵션-7.png>)

<br>

### 4. RQSuperHeroes 페이지에서 30초 기다린후

<br>

**fresh 상태 였던 캐시가 stale 상태로 변경되었다.**

**이제는 오래된 캐시라는 의미이다.**

**따라서 다시 브라우저에 포커스하면, 캐싱된 데이터를 사용하고 난후 background-refeching 한다.**

![React Query Stale Time](<../Images/React%20Query%20튜토리얼(3)%20-%20Background%20Refetch%20옵션/React%20Query%20튜토리얼(3)%20-%20Background%20Refetch%20옵션-8.png>)

<br>

**브라우저 리-포커스하면,**

- **일단 캐싱된 값을 사용했기 때문에, isLoading은 false 값이고**
- **이후 background-refetching을 했기 때문에 isFetching은 true 값이되었다.**

![React Query Stale Time](<../Images/React%20Query%20튜토리얼(3)%20-%20Background%20Refetch%20옵션/React%20Query%20튜토리얼(3)%20-%20Background%20Refetch%20옵션-9.png>)

<br>

이제 해당 쿼리 캐시의 **네트워크 요청이 줄어들게했다.**

그만큼 **로딩 성능이 향상**된다는 의미이다.

<br>

다른 **중요한 점은 staleTime의 defalut는 0s 라는 점이다.**

**Why?**

- **0초라는 staleTime은 캐시를 가장 최신 데이터로 유지할 수 있는 안전한 값이다.**

<br>

따라서 **서버 데이터가 자주 변경되는지에 따라, 개발자가 적절하게 설정해야하는 옵션이다.**

<br>

## refetchOnMount

중요한 옵션은 아니다.

하지만 간단하게 알아보도록 하자.

<br>

refetchOnMount는 **3가지 옵션이 있다.**

- true → default
- false
- always

<br>

**Component Mount일때, 캐시가 존재해야하고 이때 stale 상태 일때,**

- **refetch 할지, 안할지 결정하는 옵션이다.**
- Component Mount인데 캐시가 있다는것은, 당연히 이전에 방문을 했다는 의미이다.

```jsx
const { isLoading, data, isFetching } = useQuery(
  "super-heroes",
  () => {
    return axios.get("http://localhost:4000/superheroes");
  },
  {
    staleTime: 10000,
    refetchOnMount: false,
  }
);
```

<br>

**default인 true 부터 알아보자.**

예시를 보면 이해하기 쉽다.

1. 페이지를 **최초 방문하고, 데이터를 캐싱했다고 하자.**
2. 이후 재방문시 **Component는 Mount 가 되고, 이때 캐시가 있으면 사용한다.**
3. 이후 캐시 상태를 확인하는데, **캐시가 stale 상태라면, refetch를 한다는 의미이다.**
   1. fresh 이면 refetch를 하지 않는다.

<br>

**false 일때를 알아보자.**

1. **페이지를 최초 방문하고, 데이터를 캐싱했다.**
2. 이후 재방문시 **Component Mount 가 되고, 이때 캐시를 사용한다.**
3. **이후 캐시 상태가 어떻든지 간에**, refetch를 **하지 않는다.**

<br>

**always 일때를 알아보자.**

1. 페이지를 **최초 방문하고, 데이터를 캐싱했다고 하자.**
2. 이후 재방문시 **Component는 Mount 가 되고, 이때 캐시가 있으면 사용한다.**
3. **이후 캐시 상태가 어떻든지 간에, refetch를 한다.**

<br>

약간 헷갈릴 수 있는데, **브라우저 포커스시에 refetch하는 것과 상관없는 옵션이다.(나는 햇갈렸다 Mount, stale 이라는 것에 주의하자.)**

**가장 좋은것은 default인 true 값이다.**

<br>

## refetchOnWindowFocus

위의 옵션보다는 꽤나 중요한 옵션이다.

<br>

### 일반적인 비동기 요청 방식

useQuery를 사용하지 않았을때, **일반적인 비동기 요청을 하는 방식을 먼저 알아보자.**

<br>

예시를 보고 알아보자.

![React Query refetchOnWindowFocus](<../Images/React%20Query%20튜토리얼(3)%20-%20Background%20Refetch%20옵션/React%20Query%20튜토리얼(3)%20-%20Background%20Refetch%20옵션-10.png>)

<br>

현재 Batman, Superman, Wonder Woman은 Server data 이다.

최초에 데이터를 가져왔을때는 위의 데이터이지만, **갑자기 Server data가 아래와 같이 변경되었다고하자.**

- **Alex**
- Superman
- Wonder Woman

<br>

Batman 에서 Alex로 변경되었다.

하지만 **새로고침을 하지 않는 이상 데이터는 변경되지 않는다.**

변경된 데이터를 가져오려면, 새로고침을 눌러야만 가져오고 적용된다.

<br>

이제 useQuery를 사용했을때와 비교해보자.

<br>

### useQuery 사용시

우리가 useQuery를 사용하면, **window에 focus가 가면 자동으로 refetch를 한다.**

<br>

**위의 예제와 비교하면,**

**굳이 새로고침을 하지 않아도, window에 다시 focus를 한다면, refetch를 해 데이터를 가져와 UI가 업데이트 된다.**

<br>

※ window 에 다시 focus를 한다라는 것은

- 다른 크롬 탭에서, 다시 돌아온다거나
- alt tab으로 다른 앱을 사용하다가 돌아온다거나
- 이런것들이다.

<br>

마찬가지로 3가지 옵션이 있다.

1. true → default
2. false
3. always

<br>

**true는 defalut 옵션이다.**

**window에 focus시**

**캐시의 상태가 stale 일때만,** **refetch를 한다.**

<br>

false 일때

window에 focus를 해도 refetch가 되지 않는다.

<br>

**always 일때**

**window에 focus시,**

**캐시의 상태가 어떤 상태이던간에 항상 refetch를 한다.(fresh 상태여도 refetch를 수행한다.)**

<br>

우리는 useQuery를 처음 접했을때, 자동으로 서버 데이터를 가져온다는 것에 신기해했을것이다.

이제는 어떤 옵션을 통해서 refetch를 수행하는지 알게 되었다.

<br>

## 자동 refetch

**우리는 2가지 경우에 data-refetch가 일어나는 것을 알게되었다.**

- **onMount** : Component Mount 일때,
- **onWindowFocus** : 브라우저가 포커스 되었을때,

<br>

이번에는 **자동으로 refetch를 실행시키는 방법에 대해 알아보자.**

<br>

### refetchInterval

**일정 시간마다 자동으로 refetch를 시켜준다.**

**ms 단위로 설정해줄 수 있다.**

<br>

- defalut는 false 이다.

```jsx
const { isLoading, data, isFetching } = useQuery(
  "super-heroes",
  () => {
    return axios.get("http://localhost:4000/superheroes");
  },
  {
    refetchInterval: 2000,
  }
);
```

<br>

**중요한 점은 브라우저에 focus 되어있어야만, 일정 시간마다 자동으로 refetch가 실행된다는 것이다.**

하지만 **focus 안되어 있을때도 작동하게 하는 방법이 있다.**

<br>

### refetchIntervalInBackground

**브라우저에 focus 되어있지 않아도 refetch가 되게해준다.**

**true 값을 설정해주면된다.**

<br>

```jsx
const { isLoading, data, isFetching } = useQuery(
  "super-heroes",
  () => {
    return axios.get("http://localhost:4000/superheroes");
  },
  {
    refetchInterval: 2000,
    refetchIntervalInBackground: true,
  }
);
```

<br>

refetchInterval와 함께 쓰이는 프로퍼티 옵션이다.

**이제 수시로 데이터가 변경되는 경우라면, 정말 좋은 사용자 경험을 제공할 수 있게된다.**

<br>

React Query에서 이러한 **모든 작업은 단지, 2가지 프로퍼티만 추가하면 된다.**

<br>

참고

- [https://react-query.tanstack.com/overview](https://react-query.tanstack.com/overview)
- [https://www.youtube.com/watch?v=0BtcMLJ_Zdc&list=PLC3y8-rFHvwjTELCrPrcZlo6blLBUspd2&index=7](https://www.youtube.com/watch?v=0BtcMLJ_Zdc&list=PLC3y8-rFHvwjTELCrPrcZlo6blLBUspd2&index=7)

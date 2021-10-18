# SWR

<br>

> "SWR"이라는 이름은 HTTP RFC 5861에 의해 알려진 HTTP 캐시 무효 전략인 stale-while-revalidate에서 유래되었습니다. SWR은 먼저 캐시(스태일)로부터 데이터를 반환한 후, fetch 요청(재검증)을 하고, 최종적으로 최신화된 데이터를 가져오는 전략입니다.

- SWR 공식홈페이지
  >

<br>

## 그렇다면 **stale-while-revalidate** 이 무엇일까?

<br>

> ‘stale-while-revalidate’는 캐시가 가지고 있는 stale response가 유효한 지 백그라운드에서 재검증하는 동안 해당 stale response를 즉시 리턴하여 네트워크 지연 시간을 숨기는 전략입니다.

- 명세 RFC 5861
  >

<br>

**stale-while-revalidate**은 캐쉬 값을 사용해 불필요한 요청을 방지해 성능을 올릴 수 있다.

**stale-while-revalidate**을 사용하기 위해선 max-age가 있어야한다.

<br>

`Cache-Control: max-age=10, stale-while-revalidate=59`

<br>

`Cache-Control :max-age=10` 의 의미는 캐쉬의 유효 시간, 초단위로 설정한다.

캐쉬의 유효기간은 10초라는 의미이고 10초안에 다시 요청을 한다면 해당 캐쉬값을 재사용한다.

<br>

장점

- 캐시 덕분에 캐시 가능 시간동안 **네트워크를 사용하지 않아도됨**
- **비싼 네트워크 사용량을 줄일 수 있다.**
- 브라우저 **로딩 속도가 매우 빠르다.**
- **빠른 사용자 경험**

<br>

`stale-while-revalidate=59` 의 의미는 10~70초 사이에 다시 요청이 들어온다면,

1. 우선은 캐시로부터 오래된(stale) 캐쉬 값을 리턴받아 사용한다.
2. 동시에 캐시를 새로운 값으로 재검증 요청을 한다.

<br>

이렇게 캐쉬를 통해서 UI가 빠르게 불필요한 요청을 방지할 수 있다.

<br>

설치

```bash
npm i swr
```

<br>

## 사용

```bash
import useSWR from 'swr'

function Profile() {
  const { data, error } = useSWR('/api/user', fetcher)

  if (error) return <div>failed to load</div>
  if (!data) return <div>loading...</div>
  return <div>hello {data.name}!</div>
}
```

<br>

첫번째 인수는 URL을 받는다.

두번째 함수인 fetcher은 첫번째 인수값이 fetcher의 인자로 들어가게된다.

<br>

useSWR을 사용함으로써 hook, redux 같은 상태를 선언하거나 관리하지 않아도된다.

useSWR에서 반환된 `data, error..` 등등 으로 사용가능 하기 때문이다.

<br>

이후 데이터를 서버에 요청하면 캐쉬에 저장하게 된다.

이후에 서로 다른 컴포넌트에서도 URL이 같다면 동일한 캐쉬에서 가져와서 사용하기 때문에,

불필요한 요청을 하지 않아 성능 최적화가 가능해진다.

<br>

swr 사용전

```jsx
import axios from "axios";
import React, { useEffect, useState } from "react";

const API_URL = "https://api.hnpwa.com/v0/news/1.json";

function News() {
  const [news, setNews] = useState([]);

  useEffect(() => {
    async function reqeustGet(url) {
      try {
        const res = await axios(url);
        setNews(res.data);
      } catch (error) {
        console.log(error);
      }
    }

    reqeustGet(API_URL);
  }, []);

  return (
    <div>
      {news.map((v) => (
        <p>{v.title}</p>
      ))}
    </div>
  );
}

export default News;
```

<br>

swr 사용후

```jsx
import axios from "axios";
import useSWR from "swr";

const API_URL = "https://api.hnpwa.com/v0/news/1.json";

function UseSWRNews() {
  const fetcher = async (url) => {
    const res = await axios.get(url);
    return res.data;
  };

  const { data } = useSWR(API_URL, fetcher);

  return <div>{data && data.map((v) => <p key={v.id}>{v.title}</p>)}</div>;
}

export default UseSWRNews;
```

<br>

## 자식 컴포넌트에게 데이터 전달 필요 없음

<br>

### useEffect 사용

```jsx
import axios from "axios";
import React, { useEffect, useState } from "react";
import ChildNewsOne from "./ChildNewsOne";
import ChildNewsTwo from "./ChildNewsTwo";

const API_URL = "https://api.hnpwa.com/v0/news/1.json";

function News() {
  const [news, setNews] = useState([]);

  useEffect(() => {
    async function reqeustGet(url) {
      try {
        const res = await axios(url);
        setNews(res.data);
      } catch (error) {
        console.log(error);
      }
    }

    reqeustGet(API_URL);
  }, []);

  return (
    <div>
      {news && (
        <>
          <ChildNewsOne news={news} />
          <ChildNewsTwo news={news} />
        </>
      )}
    </div>
  );
}

export default News;

// 자식 컴포넌트
import React from "react";

function ChildNewsOne({ news }) {
  return (
    <div>
      {news.map((v) => (
        <p>{v.title}</p>
      ))}
    </div>
  );
}

// 자식 컴포넌트
export default ChildNewsOne;

import React from "react";

function ChildNewsTwo({ news }) {
  return (
    <div>
      {news.map((v) => (
        <p>{v.title}</p>
      ))}
    </div>
  );
}

export default ChildNewsTwo;
```

<br>

위와 같이 자식 컴포넌트에게 news 데이터를 props로 전달 해주어야 한다.

각각 계층화가 생기고 독립적으로 컴포넌트가 동작하지 않은, 의존성이 생기게 된다.

<br>

### SWR 사용

<br>

customHook

```jsx
import axios from "axios";
import useSWR from "swr";

const API_URL = "https://api.hnpwa.com/v0/news/1.json";

function useGetUser() {
  const fetcher = async (url) => {
    const res = await axios.get(url);
    return res.data;
  };

  const { data } = useSWR(API_URL, fetcher);

  console.log(data);
  return data;
}

export default useGetUser;
```

<br>

```jsx
import axios from "axios";
import useSWR from "swr";
import useGetUser from "./useGetUser";
import UseSWRChildOne from "./UseSWRChildOne";
import UseSWRChildTwo from "./UseSWRChildTwo";

function UseSWRNews() {
  return (
    <div>
      <UseSWRChildOne />
      <UseSWRChildTwo />
    </div>
  );
}

export default UseSWRNews;

// 자식 컴포넌트
import React from "react";
import useGetUser from "./useGetUser";

function UseSWRChildOne() {
  const data = useGetUser();

  return <div>{data && data.map((v) => <p key={v.id}>{v.title}</p>)}</div>;
}

export default UseSWRChildOne;

// 자식 컴포넌트
import React from "react";
import useGetUser from "./useGetUser";

function UseSWRChildTwo(props) {
  const data = useGetUser();

  return <div>{data && data.map((v) => <p key={v.id}>{v.title}</p>)}</div>;
}

export default UseSWRChildTwo;
```

<br>

custom Hook을 사용해서 자식 컴포넌트에서 호출해서 사용가능하다.

이렇게 custom Hook을 호출해서 사용을 해도 캐쉬에서 불러와 사용하기 때문에

서버에 반복적으로 요청을 하지않아 가능하다는 점이다.

<br>

컴포넌트로 범위가 제한되었으며 모든 컴포넌트는 서로에게 독립적이다.

부모 컴포넌트들은 데이터나 데이터 전달에 관련된 것들을 알 필요가 없다.

그냥 렌더링할 뿐이다. 코드는 이제 유지하기에 더 간단하고 쉽다.

<br>

참고

- [https://swr.vercel.app/ko](https://swr.vercel.app/ko)
- [https://web.dev/stale-while-revalidate/](https://web.dev/stale-while-revalidate/)
- [https://tecoble.techcourse.co.kr/post/2021-05-23-swr/](https://tecoble.techcourse.co.kr/post/2021-05-23-swr/)
- [https://jooonho.com/web/2021-05-16-stale-while-revalidate/](https://jooonho.com/web/2021-05-16-stale-while-revalidate/)
- [https://www.youtube.com/watch?v=b7Uqx7NZpHw](https://www.youtube.com/watch?v=b7Uqx7NZpHw)

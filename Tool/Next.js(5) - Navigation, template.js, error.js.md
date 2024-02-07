- [Next.js(5) - Navigation, Template.js , Loading.js](#nextjs5---navigation-templatejs--loadingjs)
  - [Navigation](#navigation)
    - [usePathname()](#usepathname)
    - [useRouter()](#userouter)
  - [Template](#template)
  - [Loading.js](#loadingjs)
  - [Error.js](#errorjs)

# Next.js(5) - Navigation, Template.js , Loading.js

## Navigation

다른 URL 라우터로 접근 할 수 있게해준다.

- Click 을 한뒤 navigation 할 수 있다.
- 특정 액션을 한뒤 navigation 되게 할 수 있다.

HTML로 치면 <a> 태그와 같다.

```jsx
export default function Home() {
  return (
    <>
      <h1>Welcome home!</h1>
      <Link href="/blog">Blog</Link>
    </>
  );
}
```

<br>

### usePathname()

usePathname 은 **클라이언트 컴포넌트** 훅이다 URL의 pathname을 읽을 수 있게 해준다.

```jsx
"use client";

import Link from "../../node_modules/next/link";
import { usePathname } from "../../node_modules/next/navigation";

export default function Home() {
  const pathname = usePathname();
  return (
    <>
      <h1>Welcome home!</h1>
      <h2>Current pathname : {pathname}</h2> // Current pathaname : /<Link href="/blog">Blog</Link>
    </>
  );
}
```

<br>

### useRouter()

`useRouter`은 URL 라우트를 프로그래밍으로 변경할 수 있게 해준다.

```jsx
"use client";

import { useRouter } from "../../node_modules/next/navigation";

export default function Home() {
  const router = useRouter();

  function handleClick() {
    router.push("/blog");
  }

  return (
    <>
      <h1>Welcome home!</h1>
      <button onClick={handleClick}>Click me</button>
    </>
  );
}
```

- `router.push(’/home’)` : 라우트를 변경할 수 있다. 그리고 브라우저 히스토리 스택에 추가된다.
- `router.replace('/home')` : 라우트를 변경할 수 있다. 그리고 브라우저 히스토리 스택에 추가되지 않는다.
- `reouter.refresh()` : 현재 라우트를 새로고침한다.
- `reouter.back()` : 브라우저 히스토리 스택에서 뒤로 가기를 한다.
- `router.forward()` : 브라우저 히스토리 스택에서 앞으로 가기를 한다.

<br>

## Template

각각의 페이지 또는 자식 페이지를 감싸는 것은 `layout.tsx` 파일과 굉장히 비슷하다.

하지만 다른점이 있다.

layout 페이지는 감싸져 있는 자식 페이지의 라우트가 변경되어도 mount 되지 않고 state가 유지된다.

template 페이지는 감싸져 있는 자식 페이지 라우트가 변경되면 re-mount 되고 state가 유지되지 않는다.

<br>

예를 들면

`app/layout.tsx` 파일에 input 태그가 안에 value 값이 있다고 하자.

만약 감싸져 있는 자식 페이지들이 라우트가 변경된다고 하더라도 layout component는 mount 되지 않아 input 태그에 value 값이 사라지지 않는다.

<br>

반대로

`app/template.tsx` 파일에 input 태그 안에 value 가 똑같이 있을때,

감싸져 있는 자식 페이지들의 라우트가 변경되면 mount 되어 input 태그 안의 value 값은 사라진다.

```jsx
export default function Template({ children }: { children: React.ReactNode }) {
  return <div>{children}</div>;
}
```

<br>

## Loading.js

`loading.js` 파일은 loading UI을 만들 수 있게 해준다.

서버에서 라우트로 인해 컨텐츠가 load 될때동안 loading UI를 보여준다.

렌더링이 다되면 자동으로 받아온 새로운 컨텐츠로 변경되어진다(swap).

```jsx
export default function Loading() {
  // You can add any UI inside Loading, including a Skeleton.
  return <LoadingSkeleton />;
}
```

<br>

## Error.js

서버 컴포넌트 or 클라이언트 컴포넌트에서 예상치 못한 에러가 발생한 경우 Fallback UI를 보여주어 유용하게 사용할 수 있다.

```jsx
'use client' // Error components must be Client Components

import { useEffect } from 'react'

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  useEffect(() => {
    // Log the error to an error reporting service
    console.error(error)
  }, [error])

  return (
    <div>
      <h2>Something went wrong!</h2>
      <button
        onClick={
          // Attempt to recover by trying to re-render the segment
          () => reset()
        }
      >
        Try again
      </button>
    </div>
  )
}
```

not-found.js 파일과 다른점은 not-found.js는 라우팅을 잘못한 경우 보여지는것이고

eorror.js는 에러가 발생한 경우 보여지는 것이다.

`reset()` 으로 페이지를 다시 리렌더링 시켜서 페이지를 다시 정상적으로 동작하게 재시도 한다.

<br>

참고

- https://www.youtube.com/watch?v=YG98I2Bj7qw&list=PLC3y8-rFHvwjOKd6gdf4QtV1uYNiQnruI&index=26
- https://nextjs.org/docs/app/api-reference/file-conventions/error

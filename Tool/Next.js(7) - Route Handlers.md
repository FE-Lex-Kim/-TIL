- [Next.js(7) - Route Handlers](#nextjs7---route-handlers)
  - [Route Handlers](#route-handlers)
    - [예제)](#예제)
    - [Convention](#convention)
    - [GET](#get)
    - [POST](#post)
    - [Dynamic Route Handlers](#dynamic-route-handlers)
    - [URL Query Parameters](#url-query-parameters)
    - [Redirects in Route Handlers](#redirects-in-route-handlers)
    - [Headers in Route Handlers](#headers-in-route-handlers)
    - [Cookies in Route Handlers](#cookies-in-route-handlers)

# Next.js(7) - Route Handlers

<br>

## Route Handlers

Route Handler는 웹 요청, 응답 API를 사용해서 지정된 경로에 커스텀 request handler을 만들 수 있다.

![Next.js](<https://github.com/FE-Lex-Kim/-TIL/blob/master/Images/nextjs(7)/1.png?raw=true>)

- Route handler는 app directory에서만 사용 가능하다. Route handler는 page directory 내의 API 경로와 같으므로 API 경로와 Route handler을 같이 사용할 필요가 없다.

### 예제)

경로 → app/hello/route.ts

```
export async function GET() {
  return new Response("Hello world");
}
```

`app/hello/route.ts` 파일을 생성해 위와 같은 코드를 만들어주면, `route.ts`가 만들어진 directory 경로와 같은 경로로 브라우저에서 보여준다.

<br>

경로 → `app/hello/route.ts`

![Next.js](<https://github.com/FE-Lex-Kim/-TIL/blob/master/Images/nextjs(7)/2.png?raw=true>)

<br>

경로 → `app/user/alex/route.ts`

```jsx
export async function GET() {
  return new Response("Hello Alex");
}
```

![Next.js](<https://github.com/FE-Lex-Kim/-TIL/blob/master/Images/nextjs(7)/3.png?raw=true>)

`[localhost:3000/user/alex](http://localhost:3000/user/alex)` 경로로 들어가면 Hello Alex가 나온다.

<br>

만약 `page.js`와 같은 레벨에 `route.js`와 같이 중첩되서 존재하면다면, `route.js`가 `page.js`을 무시하고 덮어서 나온다.

`app/user/alex/page.tsx` , `app/user/alex/route.ts`이 같은 폴더 레벨에 있다면 있다면, `page.tsx` 있는 내용이 나오지 않고 route.ts이 나온다.

<br>

```jsx
// 경로 -> app/user/alex/page.tsx
const Alex = () => {
  return <h1>Hi Alex</h1>;
};

export default Alex;

// 경로 -> app/user/alex/route.ts
export async function GET() {
  return new Response("hello Alex");
}
```

![Next.js](<https://github.com/FE-Lex-Kim/-TIL/blob/master/Images/nextjs(7)/4.png?raw=true>)

위와 같이 `page.tsx`가 아니라 `route.ts`가 나온다.

따라서 `route.ts`을 다음과 같은 경로에 넣어서 사용해주어야한다.

경로 → `app/user/alex/api/route.ts`

```jsx
export async function GET() {
  return new Response("hello Alex");
}
```

![Next.js](<https://github.com/FE-Lex-Kim/-TIL/blob/master/Images/nextjs(7)/5.png?raw=true>)

그럼 [localhost:3000/user/alex/api](http://localhost:3000/user/alex/api) 에서 route.ts 가 나온다.

<br>

### Convention

- Route Handler는 app directory 내의 `route.js|ts` 안에서만 정의가능하다.
- Route Hanlder는 `page.js`, `layout.js`와 같이 app directory 안에서 중첩되게 정의할 수 있다. 그러나 `page.js`와 같은 route segment 레벨에서는 route.js 파일이 있으면 안된다.
- `GET`, `POST`, `PUT`, `PATCH`, `DELETE`, `HEAD`, `OPTIONS` 과 같은 HTTP 메서드를 지원해준다. 만약 지원하지 않는 메서드를 호출한다면, Next.j는 `405 Method Not Allowed` 을 응답한다.

<br>

### GET

```jsx
const comments = [
  { id: 1, text: "This is first" },
  { id: 2, text: "This is second" },
  { id: 3, text: "This is third" },
];

export async function GET() {
  const response = Response.json(comments);
  return response;
}
```

<br>

### POST

이번엔 POST API를 서버에서 만들어보자.

![Next.js](<https://github.com/FE-Lex-Kim/-TIL/blob/master/Images/nextjs(7)/6.png?raw=true>)

위의 Thunder client 라는 익스텐션으로 get, post 등등 API를 확인 할 수 있다.

http://localhost:3000/comments 주소로 body 값을 보내보자.

<br>

```jsx
import { comments } from "./data";

//const comments = [
  { id: 1, text: "This is first" },
  { id: 2, text: "This is second" },
  { id: 3, text: "This is third" },
];

export async function GET() {
  const response = Response.json(comments);
  return response;
}

export async function POST(request: Request) {
  const comment = await request.json();
  const newComment = {
    id: comments.length + 1,
    text: comment.text,
  };
  comments.push(newComment);
  return new Response(JSON.stringify(comments), {
    headers: {
      "Content-Type": "application/json",
    },
    status: 201,
  });
}
```

위 코드를 해석하면, `requert.json()`으로 위의 사진에서 봤던 요청할 값을 body에서 받아서 객체 형식으로 변경해준준다.

이후 comments에 넣어서 POST를 보낸뒤 응답값을 확인할 수 있다.

<br>

### Dynamic Route Handlers

만약 URL params가 있다면, 동적으로 URL이 변경될 수 있다. ex) user/1 , user/2 , user/3 ….

이때는 GET, POST..등등 함수의 두번째 파라미터를 받아오면 된다.

<br>

```jsx
import { comments } from "../data";

// const comments = [
  { id: 1, text: "This is first" },
  { id: 2, text: "This is second" },
  { id: 3, text: "This is third" },
];

export async function GET(_request: Request, { params }: { id: string }) {
  const id = +params.id; // 1, 2, 3, 4 ...
  const newComment = comments.find((comment) => comment.id === id);

  return Response.json(newComment);
}
```

params는 URL params의 값이 들어가 있다. 예제 → [localhost:3000/comments/`1`](http://localhost:3000/comments/1) 여기서 `1` 값이 들어감

<br>

### URL Query Parameters

Route Handler에 전달되는 요청 객체는 NextRequest 인스턴스가 있다.

NextRequest는 쿼리 파라미터를(query parameter) 보다 쉽게 처리하는 추가 편의 메서드 이다.

```jsx
import { type NextRequest } from "next/server";

export function GET(request: NextRequest) {
  const searchParams = request.nextUrl.searchParams;
  const query = searchParams.get("query");
  // query is "hello" for /api/search?query=hello
}
```

<br>

### Redirects in Route Handlers

리다이랙션을 해줄 수 있는 기능도 있다.

잘못된 경로로 요청했을때 사용할 수 있다.

```jsx
import { redirect } from "next/navigation";

export async function GET(request: Request) {
  redirect("https://nextjs.org/");
}
```

<br>

### Headers in Route Handlers

`next/headers` 에서 `headers`를 가져올 수 있다.

`headers`는 http headers 에 대한 정보를 가져올 수 있는 함수이다.

```jsx
import { headers } from "next/headers";

export async function GET(request: Request) {
  const headersList = headers();
  const Authorization = headersList.get("Authorization");
  // Authorization 서버에 클라이언트가 접근가능한지 인증하는데 사용하는 헤더를 가져올 수 있다.
}
```

<br>

### Cookies in Route Handlers

쿠키는 서버가 사용자의 웹브라우저로 전송하는 작은 데이터 조각이다.

브라우저는 쿠키를 저장하고 나중에 동일한 서버로 다시 보낼 수 있다.

쿠키는 다음 세가지 목적으로 주로 사용된다.

- 로그인 및 장바구니와 같은 세션 관리
- 사용자 기본 설정 및 테마와 같은 맞춤 설정
- 사용자 행동 기록 및 분석과 같은 추적

```jsx
import { cookies } from "next/headers";

export async function GET(request: Request) {
  const cookieStore = cookies();
  const token = cookieStore.get("token");

  return new Response("Hello, Next.js!", {
    status: 200,
    headers: { "Set-Cookie": `token=${token.value}` },
  });
}
```

<br>

참고

- https://www.youtube.com/watch?v=25yY2RVRq_M&list=PLC3y8-rFHvwjOKd6gdf4QtV1uYNiQnruI&index=34
- https://nextjs.org/docs/app/building-your-application/routing/route-handlers

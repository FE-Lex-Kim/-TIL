- [Next.js(8) - Middleware](#nextjs8---middleware)
  - [Middleware](#middleware)
    - [Example](#example)
    - [Convention](#convention)
    - [Matcher](#matcher)
    - [**Using Cookies**](#using-cookies)

# Next.js(8) - Middleware

<br>

## Middleware

미들웨어는 요청이 완료되기 전에 코드를 수행하게 해준다.

응답 헤더 또는 요청을 수정, 리다이렉트, 재작성을 통해 응답을 수정 할 수 있다.

캐쉬화된 컨텐츠 그리고 경로들이 일치하기전에 미들웨어를 실행할 수 있다.

<br>

### Example

```jsx
import { NextResponse } from "next/server";
import type { NextRequest } from "next/server";

// This function can be marked `async` if using `await` inside
export function middleware(request: NextRequest) {
  return NextResponse.redirect(new URL("/home", request.url));
}

// See "Matching Paths" below to learn more
export const config = {
  matcher: "/about/:path*",
};
```

middleware 함수는 `NextRequest` 타입의 `request` 객체를 파라미터로 받는다.

`NextResponse.redirect(URL 경로)` 메서드를 사용해서 리다이렉트 응답을 만든다.

→ `URL 경로`로 리다이렉션 되는 응답을 생성한다.

→ 아래 config matcher URL 경로로 접근하면 리다이렉션이 된다.

리다이렉트는 `new URL()`을 통해서 `“/home”` 경로를 생성해서 `request.url`을 base URL로 설정하고 `“/home”` 이라는 URL을 base URL에 붙여 넣어준다. 여기서 `request.url`은 현재 요청의 URL이다.

→ 예제 코드)

```jsx
let m = "https://developer.mozilla.org";
let a = new URL("/123", m); // => 'https://developer.mozilla.org/123'
```

config는 미들웨어의 설정값이다.

여기서는 “/about/:path” 가 matcher로 설정됐다.

→미들웨어가 `“/about/.”`으로 시작하는 경로라면 적용되어진다.

<br>

### Convention

미들웨어를 정의하기 위해 프로젝트의 루트에 `middleware.ts` 파일을 만든다.

예를들어 `pages` or `app` 같은 같은 레벨에 `있거나`src 내부에 있다.

<br>

### Matcher

`matcher`는 미들웨어가 특정 경로로 실행되게 필터링 할 수 있다.

<br>

src/middleware.js

```jsx
export const config = {
  matcher: "/about/:path*",
};
```

<br>

아래와 같은 여러개의 경로를 설정할 수 있다.

```jsx
export const config = {
  matcher: ["/about/:path*", "/dashboard/:path*"],
};
```

<br>

### **Using Cookies**

쿠키는 일반 헤더이다. 요청 시에는 쿠키 헤더에 저장된다. 응답 시에는 `Set-Cookie` 헤더에 저장된다..

Next.js는 쿠키 extension인 `NextRequest` 및 `NextResponse` 을 통해 이러한 쿠키에 접근하고 조작할 수 있는 편리한 방법을 제공한다.

<br>

1. request 경우, 쿠키는 다음과 같은 메소드를 가지고 있다 : `get`, `getAll`, `set`, `delete`, `has`, `clear`
1. response 경우,쿠키는 다음과 같은 메소드를 가지고 있다: `get`, `getAll`, `set`, `delete`

```jsx
import { NextResponse } from "next/server";
import type { NextRequest } from "next/server";

export function middleware(request: NextRequest) {
  // Assume a "Cookie:nextjs=fast" header to be present on the incoming request
  // Getting cookies from the request using the `RequestCookies` API
  let cookie = request.cookies.get("nextjs");
  console.log(cookie); // => { name: 'nextjs', value: 'fast', Path: '/' }
  const allCookies = request.cookies.getAll();
  console.log(allCookies); // => [{ name: 'nextjs', value: 'fast' }]

  request.cookies.has("nextjs"); // => true
  request.cookies.delete("nextjs");
  request.cookies.has("nextjs"); // => false

  // Setting cookies on the response using the `ResponseCookies` API
  const response = NextResponse.next();
  response.cookies.set("vercel", "fast");
  response.cookies.set({
    name: "vercel",
    value: "fast",
    path: "/",
  });
  cookie = response.cookies.get("vercel");
  console.log(cookie); // => { name: 'vercel', value: 'fast', Path: '/' }
  // The outgoing response will have a `Set-Cookie:vercel=fast;path=/` header.

  return response;
}
```

`NextResponse.next()` → 응답으로 들어온 것을 일찍 return(반환)해주고, 계속해서 라우팅하게 해준다.

<br>

참고

- https://nextjs.org/docs/app/building-your-application/routing/middleware
- https://developer.mozilla.org/ko/docs/Web/API/URL/URL

- [Next.js(3) - Routing, **Custom Errors,** 재사용 컴포넌트 Structure, Private folders](#nextjs3---routing-custom-errors-재사용-컴포넌트-structure-private-folders)
  - [Routing](#routing)
    - [Routing convetions](#routing-convetions)
    - [Nested Routing](#nested-routing)
    - [Dynamic Routes](#dynamic-routes)
    - [Nested Dynamic Routes](#nested-dynamic-routes)
    - [catch all segments](#catch-all-segments)
  - [**Custom Errors**](#custom-errors)
  - [재사용 컴포넌트 Structure](#재사용-컴포넌트-structure)
  - [Private Folders](#private-folders)

# Next.js(3) - Routing, **Custom Errors,** 재사용 컴포넌트 Structure, Private folders

<br>

## Routing

Next.js는 라우팅 메커니즘이 파일 시스템 바탕이다.

브라우저에서 유저가 접근할 수 있는 URL path는 파일과 폴더 베이스로 정의되어 진다.

<br>

### Routing convetions

- 모든 route는 반드시 app 폴더안에 위치해야한다.
- 라우트에 대응되는 모든 파일들은 반드시 `page.js` or `page.tsx` 로 이름이 지어져야한다.

![nextjs](https://github.com/FE-Lex-Kim/-TIL/blob/master/Images/nextjs/1.png?raw=true)

http://localhost:3000/about 라우팅이 가능해진다.

- 만약 라우팅이 없는 주소로 접근하면 404 에러 페이지가 나온다.

![nextjs](https://github.com/FE-Lex-Kim/-TIL/blob/master/Images/nextjs/2.png?raw=true)

<br>

### Nested Routing

라우트를 중복시키고 싶으면 폴더안에 폴더를 만들면된다.

만약 http://localhost:3000/blog/first 를 만들고 싶으면 아래와 같이 폴더와 페이지를 만들면 된다.

![nextjs](https://github.com/FE-Lex-Kim/-TIL/blob/master/Images/nextjs/3.png?raw=true)

<br>

### Dynamic Routes

동적 라우트를 하려면 폴더를 `[동적 라우트 경로]` 으로 이름을 만들면된다.

예를 들면

1. http://localhost:3000/products/1
2. http://localhost:3000/products/2
3. http://localhost:3000/products/3
4. …

뒤에 있는 동적라우트 경로가 productId라면 아래와 같이 폴더 구조를 만들면된다.

![nextjs](https://github.com/FE-Lex-Kim/-TIL/blob/master/Images/nextjs/4.png?raw=true)

productId를 Component에서 사용하려면 params.productId를 하면된다.

```jsx
import React from "react";

type params = {
  productId: string,
};

const PageDetails = ({ params }: params) => {
  return <h1>PageDetail {params.productId}</h1>;
};

export default PageDetails;
```

![nextjs](https://github.com/FE-Lex-Kim/-TIL/blob/master/Images/nextjs/5.png?raw=true)

<br>

### Nested Dynamic Routes

라우트를 더 복잡하게 만들게 되면 라우트 파일을 만드는게 어려울 수 있다.

예를 들면 http://localhost:3000/products/1/reviews/123 와 같은 라우트를 만든다고 하면

폴더 구조를 다음과 같이해야한다.

![nextjs](https://github.com/FE-Lex-Kim/-TIL/blob/master/Images/nextjs/6.png?raw=true)

`productId` 와 `reviewId` 은 params 프로퍼티에 담겨져있다.

```jsx
import React from "react";
type params = {
  productId: string,
  reviewId: string,
};
const ReviewDetails = ({ params }: params) => {
  return (
    <h1>
      Review {params.productId} for product {params.reviewId}
    </h1>
  );
};

export default ReviewDetails;
```

<br>

### catch all segments

만약 라우트에서 파라미터를 계속 지속적으로 만들고 요청 시, 빌드시 채워지는 파라미터라면 미리 URL 파라미터를 알 수 없다.

이때 모든 파라미터 값을 가져올 수 있는 방법이 있다.

예를 들어

`shop/clothes`, 그러나 더욱더 파라미터가 늘어난다면, `/shop/clothes/tops`, `/shop/clothes/tops/t-shirts` 같이 늘어나도 해당 파리미터의 값을 사용할 수 있는 방법이 있다.

`[...slug]` 을 파일이름으로 만들어주면 된다.

```jsx
import React from "react";
type params = string[];
const ShopProducts = ({ params }: params) => {
  // /shop/a/b => 	{ slug: ['a', 'b'] }
  //shop/a/b/c => 	{ slug: ['a', 'b', 'c'] }

  return <h1>product : {params.slug.map((product: string) => product + " ")}</h1>;
};

export default ShopProducts;
```

![nextjs](https://github.com/FE-Lex-Kim/-TIL/blob/master/Images/nextjs/7.png?raw=true)

<br>

## **Custom Errors**

에러 발생시 커스텀으로 에러 페이지를 만들 수 있다.

방법은 간단하다 `app/not-found.js` 파일을 만들면된다.

```jsx
import React from "react";

const Custom404 = () => {
  return <h1>404 - Page Not Found</h1>;
};

export default Custom404;
```

<br>

## 재사용 컴포넌트 Structure

만약 재사용하는 컴포넌트가 있다면, 특정되는 폴더에 컴포넌트를 만드는것은 좋지 않다. ex)app/product/shirt.tsx

따라서 가장 최상위 폴더인 `app/components` 폴더를 만든뒤 내부에서 재사용 컴포넌트를 만드는것이 모든 컴포넌트에서 접근하기 쉬워진다.

<br>

## Private Folders

프라이빗하게 사용하는 파일들이 있다면, 언더스코어( \_ )로 폴더이름을 시작하면, 그 아래 파일들은 라우팅으로 접근이 불가능하다.

`app/_lib/page.tsx` page.tsx파일을 한다고하더라도 URL 라우트에서는 볼 수 없는 페이지이고 404 에러가 발생한다.

<br>

참고

- https://www.youtube.com/watch?v=tW3rVL7xc8Y&list=PLC3y8-rFHvwjOKd6gdf4QtV1uYNiQnruI&index=10
- https://nextjs.org/docs/app/building-your-application/routing

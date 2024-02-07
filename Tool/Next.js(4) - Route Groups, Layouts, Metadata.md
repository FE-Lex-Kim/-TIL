- [Next.js(4) - Route Groups, Layouts,Metadata](#nextjs4---route-groups-layoutsmetadata)
  - [Route Groups](#route-groups)
  - [Layouts](#layouts)
    - [Nesting Layouts](#nesting-layouts)
    - [Route Group Layout](#route-group-layout)
  - [Metadata](#metadata)
    - [Metadata rules](#metadata-rules)
    - [title Metadata](#title-metadata)

# Next.js(4) - Route Groups, Layouts,Metadata

## Route Groups

관련된 라우팅 폴더 목록별로 묶는것은 개발자 경험(DX)을 향상 시킬 수 있는 방법이다.

만약 팀 프로젝트이라면 팀원끼리 특정 라우팅 페이지들을 묶어서 놓을 수 있다면, 좀 더 쉽게 파일들을 찾기 쉬울것이다.

<br>

다음과 같이 `(묶으려는 파일들 카테고리이름)` 으로 **괄호로** 폴더를 만들면 URL 라우팅이 되지않고 라우팅 폴더들을 묶을수 있다.

![next.js](<https://github.com/FE-Lex-Kim/-TIL/blob/master/Images/nextjs(4)/1.png?raw=true>)

URL 주소에 [http://localhost:3000/](http://localhost:3000/products/clothes/top)(auth) 를 해도 라우팅이 되지 않는다.

하지만 다음과 같은 URL 라우트는 접근 가능하다.

- http://localhost:3000/forgot-password
- http://localhost:3000/login
- http://localhost:3000/register

<br>

## Layouts

여러 컴포넌트에 공통되는 UI을 공유하는 방법이 있다.

예를들어 header, footer, navigation UI가 여러 페이지에 공통될때 사용하면 된다.

`layout.js` 이라는 파일을 만들어서 공통된 UI을 이곳에서 만들면된다.

/app/layout.tsx

```jsx
export default function RootLayout({
  children,
}: Readonly<{
  children: React.ReactNode,
}>) {
  return (
    <html lang="en">
      <body>
        <header>
          <h1>Header</h1>
        </header>
        {children}
        <footer>
          <h1>Footer</h1>
        </footer>
      </body>
    </html>
  );
}
```

`children`은 `layout.tsx` 파일이 있는 위치의 모든 페이지들이 이곳에 들어간다.

따라서 Header, Footer인 공통된 UI가 보여지게된다.

<br>

### Nesting Layouts

만약 모든 페이지에서 Layout UI가 보여지는 것이 아닌 특정 몇몇 페이지에서만 보여지고 싶다면,

해당 라우트 폴더에 `layout.tsx` 파일을 만들면 된다.

ex) app/product/layout.tsx

이러면 product 하위 라우터 페이지에서만 layout.tsx UI가 보여지게된다.

<br>

### Route Group Layout

만약 app/product 폴더에 user1, user2, user3 페이지가 있다고 하자.

이중에 user2, user3만 공통된 Layout을 가지고 있다면, user1을 제외하고 묶어서 layout을 보여주어야한다.

이럴땐 괄호( ) 폴더를 이용해서 묶어준뒤 `layout.tsx` 로 UI를 만들어 주면된다.

<br>

## Metadata

Metadata를 사용해서 SEO를 향상시킬 수 있다.

Next.js는 Metadata API를 통해서 각각 페이지 별로 metatage를 정의할 수 있다.

또한 관련된 페이지 끼리 metadata를 공통되게 사용할 수 있다.

<br>

- metadata 객체를 export해 사용한다.
- 동적 generateMetadata function 을 export해 사용한다.

<br>

### Metadata rules

1.

`layout.tsx`, `page.tsx` 둘다 Metadata를 사용할 수 있다.

만약 `layout.tsx` 에 Metadata를 사용하면 모든 페이지에 적용이 된다.

하지만 `page.tsx` 에 Metadata를 사용하면 해당 페이지에만 적용이 된다.

<br>

2.

Metadata를 읽는 순서는 root level에서 final page로 아래로 읽어나간다.

즉, final page를 최우선으로 정의해준다.

<br>

3.

여러곳에서 정의된 같은 라우트의 Metadata가 있다면, 모든 Metadata를 합쳐서 정의되어진다.

하지만, 만약 같은 Metadata 프로퍼티가 있다면 `page.tsx` 는 `layout.tsx` 에게 덮여진다.(같은 계층에 있다면, 그렇지않으면 2법칙이 적용됨)

<br>

app/layout.tsx

```jsx
export const metadata = {
  name: "Next.js",
  description: "Layout Page metadata",
};

export default function RootLayout({
  children,
}: Readonly<{
  children: React.ReactNode,
}>) {
  return (
    <html lang="en">
      <body>
        <header>
          <h1>Header</h1>
        </header>
        {children}
        <footer>
          <h1>Footer</h1>
        </footer>
      </body>
    </html>
  );
}
```

<br>

app/about/page.tsx

```jsx
export const metadata = {
  title: "About Page metadata",
};

const About = () => {
  return <div>About Page</div>;
};

export default About;
```

![next.js](<https://github.com/FE-Lex-Kim/-TIL/blob/master/Images/nextjs(4)/2.png?raw=true>)

`layout.tsx` 의 `name`과 `description` 이 `page.tsx`의 `title`과 합쳐져서 about 페이지에 보여진다.

### title Metadata

<br>

`title` metadata에는 3가지 프로퍼티 옵션이 있다.

- absolute : layout 에서 title을 정의 해도 반드시 여기 값이 title이 된다.
- default : `layout.tsx` 에서 여기에 값을 넣으면, 페이지들이 따로 title 값을 정의하지 않아도 여기 값이 default 가 된다.
- template : `layout.tsx` 에서 `%s` 을 작성하면, 같은 레벨 or 하위 레벨 페이지에서 작성한 titile 값이 `%s`에 넣어져 template 삼아서 만들어지게된다.
  - ex) template : %s | Good page → `app/layout.tsx`
    default : Home page → `app/page.tsx`
    결과 값으로 `Home page | Good page` 로 title 값이 나오게 된다.

```jsx
export const metadata = {
  title: {
    absolute: "",
    default: "Home2 Page metadata",
    template: "",
  },
  description: "Home Page metadata",
};

export default function Home() {
  return <h1>Welcome home!</h1>;
}
```

<br>

참고

- https://www.youtube.com/playlist?list=PLC3y8-rFHvwjOKd6gdf4QtV1uYNiQnruI
- https://nextjs.org/docs/app/building-your-application/routing/pages-and-layouts#nesting-layouts

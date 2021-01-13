# Next.js

<br>

## 설치

<br>

설치방법

```bash
npx create-next-app nextjs-blog --use-npm --example "https://github.com/vercel/next-learn-starter/tree/master/learn-starter"
```

<br>

`--example "https://github.com/vercel/next-learn-starter/tree/master/learn-starter"` 이 명령어는 템플릿 사이트를 보여준다.

<br>

- nextjs-blog 디렉토리로 이동한다.

```bash
cd nextjs-blog
```

<br>

- 3000포트의 Next.js app의 개발용 서버를 실행시켜준다

```bash
npm run dev
```

<br>

## 라우팅

<br>

Next.js 는 페이지들의 파일경로로 라우팅되는 파일 시스템을 메커니즘으로 가지고있다.

<br>

리액트 라우터는 App 컴포넌트에 <Route> 컴포넌트로 경로와 리액트 컴포넌트들을 정해주어야 가능하다.

<br>

하지만 Next.js는 Pages 디렉토리의 경로별로 라우팅이 되어 컴포넌트로 라우팅이 결정되는것이 아닌 경로로 라우팅을 결정하여 불필요한 코드가 없어 가독성에도 좋다.

<br>

### Index routes

pages 디렉토리에 파일을 추가하면 자동으로 라우트가 가능하게 된다.

파일의 경로별로 페이지가 라우팅된다.

<br>

**예제)**

- `pages/index.js` → `/`
- `pages/blog/index.js` → `/blog`

<br>

### 중첩 라우트

<br>

중첩되어져있는 라우트를 지원해준다.

만약 폴더안에 index가 아닌 네이밍을한 파일을 만들면 자동으로 라우틀 해준다.

<br>

**예제)**

- `pages/blog/first-post.js` → `/blog/first-post`
- `pages/dashboard/settings/username.js` → `/dashboard/settings/username`

<br>

### 동적 라우팅(with support for dynamic routes)

<br>

만약 동적인 라우팅을 하고싶은경우

파일 명을 대괄호안의 식별자 명을 넣어준다.

Ex) `[id].js` 같이 해주면된다.

<br>

이때 url의 파라미터에 따라 [id].js의 앞의 id가 결정된다.

<br>

**예제)**

- `pages/blog/[slug].js` →  `'/blog/:slug' (/blog/hello-world)`
- `pages/[username]/settings.js` → `'/:username/setting' (/Lex/setting)`
- `pages/post/[...all].js` → `/post/* (/post/2020/id/title)`

<br>

더많은 작업에 대한 내용은 [Next.js의 동적 라우터 문서](https://nextjs.org/docs/routing/dynamic-routes)를 참고하면 좋다.

<br>

### 컴포넌트안에서 링크 설정

<br>

클라이언트에서 다른 페이지의 라우터로 이동 시킬수 있다.

<br>

**사용방법**

```jsx
import Link from 'next/link'

<Link href='/'>
	<a>홈으로</a>
</Link>

<Link href="/about">
	<a>About</a>
</Link>

<Link href="/blog/hello-world">
	<a>Blog Post</a>
</Link>
```

<br>

- `/` → `pages/index.js`
- `/about` → `pages/about.js`
- `/blog/hello-world` → `pages/blog/[slug].js`

<br>

### path를 동적으로 연결

<br>

동적으로 path을 상태나 프롭스로를 받아 전달시킬 수있다.

<br>

**예제)**

```jsx
import Link from 'next/link'

function Posts({ posts }) {
  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>
          <Link href={`/blog/${encodeURIComponent(post.slug)}`}>
            <a>{post.title}</a>
          </Link>
        </li>))}
    </ul>)
}

export default Posts
```

<br>

위의 예제와 같이 encodeURICompoent를 해주어야한다.

encodeURICompoent는 utf-8로 호환되게 만들어준다.

만약 `?`  같은 코드가 링크의 주소로 들어가게 되면 제대로 적용되지 않는다.

<br>

떄문에 인코딩을 해준뒤 적용해주어야 원하는 URL 그대로 적용되어 라우팅된다.

<br>

[encodeURIComponent()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent) 에대한 자세한내용

<br>

**다른방법**

```jsx
import Link from 'next/link'

function Posts({ posts }) {
  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>
          <Link
            href={{
              pathname: '/blog/[slug]',
              query: { slug: post.slug },
            }}
          >
            <a>{post.title}</a>
          </Link>
        </li>
      ))}
    </ul>
  )
}

export default Posts
```

<br>

href에 객체를 전달한다.

`pathname`는 pages 디렉토리의 이름이라고 생각하면된다.

즉, 원하는 URL 경로를 적어준다.

<br>

`query` 는 동적으로 바뀌는 쿼리의 값을 우리가 사용하고싶어 하는 값으로 넣어주면된다.

<br>

- 이 글은 [Next.js](https://nextjs.org/) 공식홈페이지를 보고 정리한 내용입니다.
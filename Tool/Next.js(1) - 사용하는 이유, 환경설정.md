- [Next.js(1) - 사용하는 이유, 환경 설정](#nextjs1---사용하는-이유-환경-설정)
  - [사용하는 이유](#사용하는-이유)
  - [환경설정](#환경설정)

# Next.js(1) - 사용하는 이유, 환경 설정

## 사용하는 이유

**기존의 React를 사용해서 CSR을 하게되면 클라이언트에서 JS를 실행시키고 서버사이드에서 HTML 뼈대만 받게된다. 이후 클라이언트에서 CSS와 JS를 실행시킨다. 그에따라**

단점으로는

- 클라이언트에서 JS와 렌더링이 발생하여 초기 표시에 다소 시간이 걸린다.
- 서버 사이드에서 뼈대만 있는 HTML만 줌으로 SEO에 좋지않다.

Next.js는 이런 문제점을 보안해서

**서버 사이드 랜더링(SSR), 정적 웹사이트(SSG) 생성등 페이지별로 SSR,SSG를 편리하게 사용할 수 있는 기능이 있다.**

SSR는 서버 사이드에서 자바스크리브 실행하고 HTML을 모두 생성해서 반환한다.

그에 따라 CSR의 단점이였던 점을 보안할 수 있다.

- 렌더링을 서버사이드에서 수행하므로 **사이트를 빠르게 표시 할 수 있다.**
- 서버 사이드에서 콘텐츠를 생성하므로 **SEO를 향상시킬 수 있다.**

Next.js의 대표적인 기능으로는

- 리액트 프레임워크
- SPA/SSR/SSG의 쉬운 전환
- 간단한 페이지 라우팅
- 타입스크립트 기반
- 간단한 배포
- 낮은 학습비용
- 웹팩 설정 은폐
- 디렉터리 기반의 자동 라우팅
- 코드 분할 및 결합

이 있다.

## 환경설정

`—-ts` 옵션을 붙이면 타입스크립트용 프로젝트가 작성된다.

`create-next-app <프로젝트명>` 으로 생성할 수 있다.

설치방법

```bash
npx create-next-app@latest --ts next-sample
```

개발 서버 실행

```bash
npm run dev
```

빌드

```bash
// 프로젝트 빌드
npm run build

// 빌드된 생성물로 서버를 가동
npm run start
```

`build`를 하면 .next 아래로 저장한다.

`start`를 사용해 .next의 데이터를 기반으로 서버를 기동한다.

```bash
app
    ├── favicon.ico
    ├── globals.css
    ├── layout.tsx
    ├── page.module.css
    └── page.tsx
```

page.tsx는 해당 폴더의 이름으로 라우팅이되어 UI를 보여주는 페이지이다.

app폴더의 `page.tsx`이므로 프로젝트의 홈( / ) 페이지의 UI를 이곳에서 보여주게 된다.

참고

- **타입스크립트, 리액트, Next.js로 배우는 실전 웹 애플리케이션 개발** 책을 정리한 글 입니다.
- https://nextjs.org/docs/getting-started/project-structure
- https://www.youtube.com/watch?v=YuqB8D6eCKE

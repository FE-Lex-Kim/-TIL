- [Next.js(6) - Parallel Routes,Unmatched Routes,Default.js, Conditional Routes](#nextjs6---parallel-routesunmatched-routesdefaultjs-conditional-routes)
  - [Parallel Routes(병렬 라우팅)](#parallel-routes병렬-라우팅)
  - [Unmatched Routes](#unmatched-routes)
  - [Default.js](#defaultjs)
  - [Conditional Routes](#conditional-routes)

# Next.js(6) - Parallel Routes,Unmatched Routes,Default.js, Conditional Routes

<br>

## Parallel Routes(병렬 라우팅)

Parallel Routes는 하나 또는 그 이상의 페이지를 하나의 layout에서 렌더링 하게 해준다.

dashboards, feeds, social site 같은 곳에서 같은 동적 섹션 app에서 굉장히 유용하다.

<br>

예를 들어 병렬 경로를 사용하여 team 페이지와 analytics 페이지를 동시에 렌더링할 수 있다.

![Next.js](<https://github.com/FE-Lex-Kim/-TIL/blob/master/Images/nextjs(6)/1.png?raw=true>)

Parallel routes는 slot이라는 이름으로 사용되어 만들 수 있다. Slot 은 `@folder` 와 같은 컨벤션으로 정의되어진다.

<br>

Slot은 부모 `layout.jsx`에 props으로 전달된다.

예를들어) 위 사진의 예제에서는, `app/layout.js` 안의 컴포넌트는 `@analytics` ,`@team` 두개의 slot을 props로 전달되어진다. 그리고 두개의 slot은 `children` props와 함께 병렬적으로 렌더되어진다.

<br>

하지만 slot은 라우트 페이지와 URL 구조가 될 수 없다. 예를 들어

`/dashboard/@analytics/views` 는 될 수 없고 `/dashboard/view` 로 되어진다.

<br>

Parallel routes는 독립적으로 동작한다.

![Next.js](<https://github.com/FE-Lex-Kim/-TIL/blob/master/Images/nextjs(6)/2.png?raw=true>)

<br>

## Unmatched Routes

Parallel Routes을 통해서 각각 Component 들이 동작한다는 것을 알았다.

이것을 이용해서 유용하게 동적으로 컴포넌트UI를 변경하는 방법이 있다.

![Next.js](<https://github.com/FE-Lex-Kim/-TIL/blob/master/Images/nextjs(6)/3.png?raw=true>)

![Next.js](<https://github.com/FE-Lex-Kim/-TIL/blob/master/Images/nextjs(6)/4.png?raw=true>)

![Next.js](<https://github.com/FE-Lex-Kim/-TIL/blob/master/Images/nextjs(6)/5.png?raw=true>)

`@notification` 폴더에 ArchivedNotification 이라는 폴더를 만든뒤 `app/dashboard/@notification/ArchivedNotification/page.tsx`를 만든다.

`app/dashboard/@notifiaction/page.tsx`에서 ArchivedNotification 라우트로 가는 Link를 만든뒤 이동하면 `notficaiton/page.tsx` 만 변경이 되고 `user/page.tsx`, `revenu/page.tsx` 는 사라지거나 변경되지 않는다.

<br>

그리고 URL 라우트 또한 http://localhost:3000/dashboard/ArchivedNotification 으로 변경된다.

![Next.js](<https://github.com/FE-Lex-Kim/-TIL/blob/master/Images/nextjs(6)/6.png?raw=true>)

이렇게 component가 하나만 변경되게 할 수 있다.

<br>

## Default.js

브라우저 새로고침때, 만약 slot이 URL과 일치하지 않는다면, Next.js는 slot이 활성 상태 인것을 알아채지 못한다.

대신에 URL과 일치 하지않는 slot 파일들은 `default.js` 을 렌더한다.

이해하기 어렵지만 아래 예제를 보면 이해할만 하다.

예제)

1. ArchivedNotification 링크를 탄다.

![Next.js](<https://github.com/FE-Lex-Kim/-TIL/blob/master/Images/nextjs(6)/7.png?raw=true>)

2. [localhost:3000/dashboard/ArchivedNotification](http://localhost:3000/dashboard/ArchivedNotification) 으로 이동한다.

![Next.js](<https://github.com/FE-Lex-Kim/-TIL/blob/master/Images/nextjs(6)/8.png?raw=true>)

3. 이때 새로고침을 한다면, user, Revenue slot들은 현재 URL과 slot들의 URL이 일치하지 않으므로 `default.js`을 렌더한다.

   (use, Revenue 입장에서는 http://localhost:3000/dashboard 여야하는데 [localhost:3000/dashboard](http://localhost:3000/dashboard)/archivedNotification 이므로)

```jsx
// @user/default.js
const UserDefault = () => {
  return <div>UserDefault</div>;
};

export default UserDefault;

// @revenue/default.js
const RevenueDefault = () => {
  return <div>RevenueDefault</div>;
};

export default RevenueDefault;
```

![Next.js](<https://github.com/FE-Lex-Kim/-TIL/blob/master/Images/nextjs(6)/9.png?raw=true>)

(`default.js` 컴포넌트에만 보라색을 넣어보았다.)

<br>

## Conditional Routes

Parallel Routes을 사용해 특정 조건에 따라서 라우트 렌더를 다르게할 수 있다.

![Next.js](<https://github.com/FE-Lex-Kim/-TIL/blob/master/Images/nextjs(6)/10.png?raw=true>)

<br>

참고

- https://nextjs.org/docs/app/building-your-application/routing/parallel-routes
- https://www.youtube.com/watch?v=8I5-OTNOni0&list=PLC3y8-rFHvwjOKd6gdf4QtV1uYNiQnruI&index=28

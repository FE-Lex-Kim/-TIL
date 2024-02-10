- [Next.js(6) - Parallel Routes,Unmatched Routes,Default.js, Conditional Routes, Intercepting Routes](#nextjs6---parallel-routesunmatched-routesdefaultjs-conditional-routes-intercepting-routes)
  - [Parallel Routes(병렬 라우팅)](#parallel-routes병렬-라우팅)
  - [Unmatched Routes](#unmatched-routes)
  - [Default.js](#defaultjs)
  - [Conditional Routes](#conditional-routes)
  - [Intercepting Routes](#intercepting-routes)
    - [Convention](#convention)
    - [Example Modals](#example-modals)

# Next.js(6) - Parallel Routes,Unmatched Routes,Default.js, Conditional Routes, Intercepting Routes

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

`app/dashboard/@notification/page.tsx`에서 ArchivedNotification 라우트로 가는 Link를 만든뒤 이동하면 `notification/page.tsx` 만 변경이 되고 `@user/page.tsx`, `@revenue/page.tsx` 는 사라지거나 변경되지 않는다.

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

## Intercepting Routes

Intercepting routes 는 현재 layout에서 어플리케이션의 다른 부분의 라우트를 로드하는게 가능해진다.

이게 무슨말이냐면, 현재 보여지는 layout이 있다면, Link를 통해 다른 URL 라우트로 이동해도 현재 layout이 보여지는 동시에 그 위에(현재 layout 위에 다른 Route에서 보여지는 layout을 보여줄 수 있다는 뜻이다.

이러한 라우팅 파라다임은 굉장히 유용하다.

유저가 다른 컨텍스트로 변경하지 않아도 개발자가 의도하는 라우트의 컨텐츠를 그대로 보여줄 수 있기 때문이다.

<br>

예를 들어)

![Next.js](<https://github.com/FE-Lex-Kim/-TIL/blob/master/Images/nextjs(6)/11.png?raw=true>)

위와 같이 여러개의 사진이 있는 feed에서 사진 하나를 클릭했을때, feed가 있는 layout 위에 사진이 있는 모달을 보여줄 수 있다.

Next.js는 `/photo/123` 라우트를 intercept한후(가로챈후), 가로챈후 개발자가 보여주려고 의도한 segment를(segment를 잘모른다면 그냥 UI 컴포넌트라고 생각하면됨) `/feed` 위에 그대로 보여준다.

- 여기서 개발자가 보여주려고 의도한 segment는 `/feed/(..)phtoto/[id]/page.tsx` 파일이다.

<br>

### Convention

path convention `../` 과 비슷하다.

- (.)는 같은 레벨의 segment를 매치해준다.
- (..)는 하나위의 레벨의 segment를 매치해준다.
- (..)(..)는 두개위의 레벨의 segment를 매치해준다.
- (…)sms root app directly에 있는 segment를 매치해준다.

<br>

### Example Modals

intercepting Routes는 모달을 만들때 Parallel Routes와 함께 사용할 수 있다.

모달을 만들때 자주생기는 문제를 쉽게 해결할 수 있다. 예를 들면)

- URL을 통해 모달 컨텐츠를 공유할 수 있게 만들수 있다.
- 페이지를 새로고침때, 원래는 모달이 사라져야하지만 모달이 그대로 유지되게 할 수 있다.(URL이 있 깅에 가)함
- 뒤로가기를 통해 이전 라우트를 가는것으로 모달을 닫을 수 있다.(URL이 있기에 가능함)
- 앞으로가기를 했을때 모달을 다시 열수 있다.(URL이 있기에 가능함)

또한 이러한 UI 패턴을 사용하면, 유저가 브라우저 모달 URL을 공유해서 바로 브라우저 URL로 직접 접근했을때 모달을 열 수 있게 해준다.

![Next.js](<https://github.com/FE-Lex-Kim/-TIL/blob/master/Images/nextjs(6)/12.png?raw=true>)

위의 예제를 설명하면,

`feed`에서 `@modal`을 동시에 같이 보여주게 하는 parallel route 기법을 사용했다.

modal 이라는 route는 없게하고, photo 라우트를 통해 모달 URL을 만들어주는 것이다.

따라서 feed 에서 photo을 클릭했을때, `@modal/(..)photo/[id]/page.tsx`가 `photo/[id]/page.tsx` 라우트를 가로채서 보여준다.

<br>

참고

- https://nextjs.org/docs/app/building-your-application/routing/parallel-routes
- https://www.youtube.com/watch?v=8I5-OTNOni0&list=PLC3y8-rFHvwjOKd6gdf4QtV1uYNiQnruI&index=28

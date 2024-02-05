- [Next.js(2) - RSC 배경, RSC 특징, RSC 동작과정](#nextjs2---rsc-배경-rsc-특징-rsc-동작과정)
  - [RSC가 등장하게된 배경](#rsc가-등장하게된-배경)
  - [next.js RSC 특징](#nextjs-rsc-특징)
    - [RSC 특징](#rsc-특징)
  - [RSC 동작 과정](#rsc-동작-과정)

# Next.js(2) - RSC 배경, RSC 특징, RSC 동작과정

## RSC가 등장하게된 배경

next.js는 RSC(React Sever Component)를 기본(default)로 사용한다.

<br>

RSC란 서버에서 동작하는 리액트 컴포넌트이다.

<br>

기존에 리액트 컴포넌트는 브라우저,즉 클라이언트에서 동작했다.

따라서 클라이언트에서 리액트 컴포넌트를 실행한다.

이럴때 API waterfall 현상이 발생한다.

<br>

API waterfall 이란 예를들어

```jsx
function KakaopayHome({ memberId }) {
  return (
    <MemberDetails memberId={memberId}>
      <MoneyBalance memberId={memberId}></MoneyBalance>
      <PaymentHistory memberId={memberId}></PaymentHistory>
    </MemberDetails>
  );
}
```

각각 부모 컴포넌트, 자식 컴포넌트들이 API를 호출해 memberId를 받아와야한다.

이때 두가지 방법이 있다.

<br>

**첫번째는 MemberDetails(부모 컴포넌트)에서 API를 호출해 받아온뒤 자식 컴포넌트에게 전달하는 방법이다.**

그로인해 자식 컴포넌트와 결속이되 유지보수가 어려워진다.

만일 컴포넌트가 바뀌거나 다른 컴포넌트로 이동되는 경우 API를 다시 호출해야하는 불필요한 over-fetching이 발생한다.

<br>

**두번째는 각각 부모, 자식 컴포넌트에서 API를 호출하는 방법이다.**

클라이언트는 같은 API를 여러번 호출에 불필요한 서버요청이 발생된다.

또한 부모컴포넌트가 렌더링 된후 API를 받아오기 전까지는 자식 컴포넌트는 렌더링과 API 호출이 지연된다.

따라서 연속된 API 호출로 인한 waterfall 현상이 발생한다.

<br>

이를 해결하기 위해서 RSC가 등장했다.

서버에서 Render를 수행하기 때문에 클라이언트의 연속된 API 호출을 제거해서 client-server-waterfall을 막을 수 있다.

- (하지만 서버 컴포넌트의 데이터 요청도 중복되기 때문에 network waterfall은 여전히 존재한다.)

<br>

**!주의**

- RSC는 SSR이 아니라 CSR이다. 단순히 서버에서 API를 호출하는게 가능한것이지, SSR 처럼 HTML을 전부 렌더링해서 클라이언트에 보내는것이 아니다.
  - 따라서 필요한 경우 포커스, 인풋 입력값 같은 클라이언트 상태를 유지하며 여러 번 데이터를 가져오고 리렌더링하여 전달할 수 있다.
  - SSR은 refect가 필요한경우 HTML을 전체를 리렌더링 해야한다.
- RSC는 클라이언트에게 리액트 컴포넌트를 전달할때 리액트 컴포넌트를 리액트 json 엘리먼트 형태로 변환해서 보내준다.

<br>

## next.js RSC 특징

`getServerSideProps` 는 Next.js 데이터를 가져와 사용하고 페이지의 내용을 요청(request)할때 렌더하는 함수이다.

이전에는 RSC 개념이 없어서 API 데이터을 가져올때 일일이 `getServerSideProps` 를 이용해 함수를 사용해서 하위 컴포넌트에 넘겨주어야했다.

**RSC가 도입되면서 이제는 페이지의 최상단이 아닌 각각의 컴포넌트 안에서 API 데이터를 가져와 이용하는 것이 가능해졌다.**

<br>

### RSC 특징

- RSC 에서는 useState, useEffect 와 같이 클라이언트에서 사용 되는 훅을 사용할 수 없다.
- RSC 에서는 onClick, onFocus 와 같은 이벤트 핸들러는 사용이 불가능하다.
- RSC 가 렌더가 완료되기 전까지 클라이언트에서는 React 의 Suspense 로 RSC 자리에 다른 컴포넌트를 대신 보여주는 것이 가능하다.
- RSC 에서 사용되는 패키지들은 모두 서버에서 import 되고 서버에서만 사용되기 때문에 js 번들로 클라이언트에 전달되지 않다.

<br>

## RSC 동작 과정

1. 서버가 요청을 받는다.. (ex. 사용자가 브라우저 주소창에 주소를 입력하여 접속 시도)

2. 서버는 해당 페이지에서 그려야 할 서버 컴포넌트와 클라이언트 컴포넌트들을 tree 로 구성 및 JSON 형태로 직렬화 하고 이를 토대로 html 을 생성한다. (만약 이 때, Suspense 로 감싸진 Promise 를 반환하는 서버 컴포넌트일 경우에는 해당 컴포넌트가 렌더 완료 될 때까지 기다리지 않고 해당 자리에 template 을 대신 위치시키고 진행해 나간다.)

3. 생성된 html 과 클라이언트 컴포넌트의 하이드레이션을 위한 js 번들 파일들을 클라이언트에 전달한다.

4. 클라이언트에 html 렌더링 되고 클라이언트 컴포넌트들은 js 가 로드되며 하이드레이션이 진행된다.

5. Suspense 로 감싸져 있는 서버 컴포넌트가 있을 때, 해당 컴포넌트가 서버에서 렌더링이 완료되었다면 서버에서는 렌더링된 결과를 React 엘리먼트 JSON 형태로 클라이언트에 전달된다.

6. 클라이언트는 서버로부터 받은 React 엘리먼트 JSON 형태를 파싱하여 이를 html 로 역직렬화 하여 DOM 에 추가한다.

7. 해당 서버 컴포넌트가 렌더되어야 할 자리에 렌더링 되어 있던 template 이 제거되고 서버 컴포넌트가 최종적으로 렌더링 되게 된다.

<br>

참고해서 정리한 글

- https://nextjs.org/docs/pages/building-your-application/data-fetching/get-server-side-props
- https://tech.kakaopay.com/post/react-server-components/
- [https://velog.io/@duarufp06/리액트-서버-컴포넌트는-무엇이고-어떻게-다를까#hydration](https://velog.io/@duarufp06/%EB%A6%AC%EC%95%A1%ED%8A%B8-%EC%84%9C%EB%B2%84-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EB%8A%94-%EB%AC%B4%EC%97%87%EC%9D%B4%EA%B3%A0-%EC%96%B4%EB%96%BB%EA%B2%8C-%EB%8B%A4%EB%A5%BC%EA%B9%8C#hydration)
- https://funveloper.tistory.com/214

- [React SPA 라우터](#react-spa-라우터)
  - [React의 SPA 라우팅](#react의-spa-라우팅)
    - [리액트 라우터 설치](#리액트-라우터-설치)
    - [**리액트 라우터를 적용하는 방법**](#리액트-라우터를-적용하는-방법)
    - [Route 컴포넌트로 특정주소에 컴포너트 연결시켜주기](#route-컴포넌트로-특정주소에-컴포너트-연결시켜주기)
    - [Link](#link)
    - [Route 하나에 여러 개의 path 설정](#route-하나에-여러-개의-path-설정)
    - [URL 파라미터와 쿼리](#url-파라미터와-쿼리)
    - [URL 파라미터](#url-파라미터)
    - [URL 쿼리](#url-쿼리)
    - [서브 라우트](#서브-라우트)
  - [리액트 라우터 부가기능](#리액트-라우터-부가기능)
    - [history](#history)
    - [withRouter](#withrouter)
    - [Switch(V6에서 사라짐 대신 <Routes/> 로 변경됨)](#switchv6에서-사라짐-대신-routes-로-변경됨)

<br>

# React SPA 라우터

<br>

[SPA에대한 자세한설명](https://github.com/Alex-Eojin/-TIL/blob/master/Javascript/SPA.md)

<br>

SPA는 Single Page Application의 약자이다.

전통적인 웹방식은 서버에서 화면을 랜더링한후

브라우저에 html을 응답해주었다.

<br>

SPA는 싱글페이지로서 뷰랜더링을 브라우저가 담당하고 필요한 데이터만 서버에다가 요청을한다.

빠른속도를 보여주고 UX면에서 사용자가 편리함을 느낀다.

<br>

## React의 SPA 라우팅

<br>

리액트 라우팅 라이브러리는

리액트라우터, 리치 라우처, Next.js등 여러가지가 있다.

<br>

그중 가장 사용을 많이하는것은 리액트 라우터이다.

<br>

### 리액트 라우터 설치

리액트 라우터 라이브러리 설치 명령어

```bash
npm i react-router-dom
```

<br>

### **리액트 라우터를 적용하는 방법**

index.js파일에서 react-router-dom에 내장되어있는 BrowserRouter이라는 컴포넌트로 감싸면된다.

<br>

이컴포넌트는 HTML5의 History API를 사용해서 페이지를 새로고침하지 않고 주소를 변경하고, 현재 주소에 관련된 정보를 props로 쉽게 사용할수있게해준다.

```jsx
import React from "react";
import ReactDOM from "react-dom";
import "./index.css";
import App from "./App";
import reportWebVitals from "./reportWebVitals";
import { BrowserRouter } from "react-router-dom";

ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  document.getElementById("root")
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();
```

<br>

### Route 컴포넌트로 특정주소에 컴포너트 연결시켜주기

<br>

Route 컴포넌트를 사용하여 현재 경로에 따라 다른 컴포넌트를 보여줄수있다.

<br>

사용방식(**V6이전 버전에만 해당 이제는 Error가 발생한다.**)

```jsx
import { Route, Link } from 'react-router-dom';
<Route path='주소규칙' component={보여줄 컴포넌트} />
```

<br>

**사용버전(V6버전 부터, 현재 방법을 사용하면 된다.)**

```jsx
import { Route, Link } from "react-router-dom";
import Alex from "./people";

<Routes>
  <Route path="주소규칙" element={<Alex />} />
</Routes>;
```

<br>

/about 경로로 들어가면 Home과 About 컴포넌트 둘다 보여진다.

때문에 **exact**를 사용하면 정확히 그 주소여야만 그페이지가 보여지게된다.

```jsx
import React, { Component } from "react";
import { Route } from "react-router-dom";
import Home from "./Home";
import About from "./About";

class App extends Component {
  render() {
    return (
      <div>
        <Routes>
          <Route path="/" exact element={<Home />} />
          <Route path="/About" element={<About />} />
        </Routes>
      </div>
    );
  }
}

export default App;
```

<br>

### Link

Link 컴포넌트는 클릭하면 다른 주소로 이동시켜 주는 컴포넌트이다.

일반 웹페이지에서는 a태그로 보여진다.

a태그를 리액트에서 사용하면 그페이지로 새로 서버에다가 요청을 하게 되므로 a태그를 사용하면 안된다.

<br>

Link 컴포넌트를 사용하면 페이지를 새로 불러오지 않고

HTML5 History API를 사용하여 페이지의 주소만 변경해준다.

<br>

**사용방법**

```jsx
import { Route, Link } from "react-router-dom";
<Link to="주소">내용</Link>;
```

<br>

```jsx
import React, { Component } from "react";
import { Route, Link } from "react-router-dom";
import Home from "./Home";
import About from "./About";

class App extends Component {
  render() {
    return (
      <div>
        <ul>
          <li>
            <Link to="/">홈</Link>
          </li>
          <li>
            <Link to="/About">소개</Link>
          </li>
        </ul>
        <Routes>
          <Route path="/" exact element={<Home />} />
          <Route path="/About" element={<About />} />
        </Routes>
      </div>
    );
  }
}

export default App;
```

<br>

### Route 하나에 여러 개의 path 설정

<br>

```jsx
<Routes>
  <Route path="/About" element={<About />} />
  <Route path="/info" element={<About />} />
</Routes>
```

여러개의 path에 같은 컴포넌트를 보여주고 싶을떄는 각각 Route 컴포넌트를 만들어야했다.

<br>

이렇게 사용하는 대신에 path props를 배열로 설정해주면 여러 경로에서 같은 컴포넌트를 보여줄 수 있다.

```jsx
<Routes>
  <Route path={["/about", "/info"]} element={<About />} />
</Routes>
```

<br>

### URL 파라미터와 쿼리

페이지 주소를 정의할때 이동할 페이지를 유동적으로 정해야 할때가 있다.

예를들어 사용자의 아이디, 이름 별로 페이지 내용이 다르게 해야할때

<br>

이럴때 파라미터와 쿼리를 사용하면된다.

<br>

파라미터 : /profile/**alex**

쿼리 : /about?**details=true**

<br>

파라미터와 쿼리를 딱 정해서 쓰는 경우는 없다.

하지만 일반적으로 사용하는 경우가 있다.

<br>

**파라미터를 쓰는경우**

특정 아이디 또는 이름을 사용하여 조회할때

파라미터 : /profile/**alex**

<br>

**쿼리를 쓰는경우**

웹페이지에서 어떠한 키워드를 검색하거나 그 페이지에서 필요한 옵션을 전달할때 사용한다.

쿼리 : /about?**details=true**

<br>

### URL 파라미터

<br>

```jsx
import React from "react";

const data = {
  Alex: {
    name: "어진",
    description: "리액트를 좋아하는 개발자",
  },
  Jaems: {
    name: "어진쓰",
    description: "여행을 좋아하는 일반인",
  },
};

const profiles = ({ match }) => {
  const { username } = match.params;
  const profile = data[username];
  if (!profile) {
    return <div>존재하지 않는 사용자입니다.</div>;
  }
  return (
    <div>
      <h3>
        {username} {profile.name}
      </h3>
      <p>{profile.description}</p>
    </div>
  );
};

export default profiles;
```

<br>

URL파라미터를 사용할때는 라우터로 사용되는 컴포넌트에서 match라는 객체의 params 값을 참조한다.

<br>

![React SPA 라우터](../Images/React%20SPA%20라우터/React%20SPA%20라우터-1.png)

<br>

`/profile/:username` 이라고 path에 넣어주면 match.params.username 값을 통해 현재 username값을 조회할수있다.

```jsx
<Route path="/profiles/:username" element={profiles} />
```

<br>

### URL 쿼리

<br>

쿼리는 location 객체에 들어있는 search값에서 조회할수있다.

location도 라우터 컴포넌트에서 props를 통해 전달된다.

웹 어플리케이션의 현재 주소에 대한 정보를 가지고있다.

```
{
“pathname”: “/about”,
“search”: “?detail=true”,
“hash”: “”
}
```

<br>

위 location 객체는 **http://localhost:3000/about?detail=true** 주소로 들어갔을 때의 값이다.

<br>

쿼리를 읽을때는 search의 값을 확인해야한다.

이값은 문자열로 되어있다.

**?deatail=true&another=1** 과 같이 여러가지 값을 설정해 줄 수 있다.

<br>

쿼리 문자열을 객체로 변환할때 **qs 라는 라이브러리** 를 사용하면된다.

```bash
$ npm i qs
```

<br>

`qs.parse(location.search)` 객체

![React SPA 라우터](../Images/React%20SPA%20라우터/React%20SPA%20라우터-2.png)

<br>

```bash
import React from 'react';
import qs from 'qs';

const About = ({ location }) => {
  const query = qs.parse(location.search, {
    ignoreQueryPrefix: true, // 맨앞의 ?를 생략해준다.
  });
  const showDetail = query.detail === 'true';
  return (
    <div>
      <h1>소개</h1>
      <p>리액트 라우터 기초 실습 페이지</p>
      {showDetail && <p>detail 값을 true로 설정했다!</p>}
    </div>
  );
};

export default About;
```

<br>

### 서브 라우트

서브 라우트는 라우트 내부에 또 라우트를 정의 하는것을 말한다.

컴포넌트 안에 라우트를 더 적어주기만 하면된다.

```jsx
import React from "react";
import { Route, Link } from "react-router-dom";
import Profile from "./profile";

const profiles = () => {
  return (
    <div>
      <h3>사용자 목록:</h3>
      <ul>
        <li>
          <Link to="/profiles/Alex">Alex</Link>
        </li>
        <li>
          <Link to="/profiles/James">James</Link>
        </li>
      </ul>
      <Routes>
        <Route
          path="/profiles"
          exact
          render={() => <div>사용자를 선택해 주세요</div>}
        />
        <Route path="/profiles/:username" element={<Profile />} />
      </Routes>
    </div>
  );
};

export default profiles;
```

<br>

만약 Route컴포넌트에 따로 컴포넌트를 전달하는것이아니라

보여주고싶은 JSX 를 render props로 보여줄수있다.

<br>

## 리액트 라우터 부가기능

<br>

### history

<br>

history 객체는 앞서 설명된 match, location 처럼 컴포넌트에 전달되는 props중 하나이다.

<br>

이 객체를 사용하면 다른페이지로 가는것을 막거나, 뒤로가거나 할수있다.

ex) block → 현재페이지에서 나가는것을 막아준다. 인수로 경고창의 메세지에 넣을내용을 적어준다.

페이지에 나가는것을 사용자에게 확인할수있게해준다.

`this.props.history.block('정말 떠나실 건가요?');`

<br>

```jsx
import React, { Component } from "react";

class HistorySample extends Component {
  handleGoBack = () => {
    this.props.history.goBack();
  };

  handleGoHome = () => {
    this.props.history.push("/");
  };

  componentDidMount() {
    this.unblock = this.props.history.block("정말 떠나실 건가요?");
  }

  componentWillUnmount() {
    if (this.unblock) {
      this.unblock();
    }
  }

  render() {
    return (
      <div>
        <button onClick={this.handleGoBack}>뒤로</button>
        <button onClick={this.handleGoHome}>홈으로</button>
      </div>
    );
  }
}

export default HistorySample;
```

<br>

```jsx
import React, { Component } from "react";
import { Route, Link } from "react-router-dom";
import Home from "./Home";
import About from "./About";
import profiles from "./profiles";
import HistorySample from "./HistorySample";

class App extends Component {
  render() {
    return (
      <div>
        <ul>
          <li>
            <Link to="/">홈</Link>
          </li>
          <li>
            <Link to="/About">소개</Link>
          </li>
          <li>
            <Link to="/profiles">프로필</Link>
          </li>
          <li>
            <Link to="/history">history 예제</Link>
          </li>
        </ul>
        <Routes>
          <Route path="/" exact element={<Home />} />
          <Route path={["/about", "/info"]} element={<About />} />
          <Route path="/profiles" element={<Profiles />} />
          <Route path="/history" element={<HistorySample />} />
        </Routes>
      </div>
    );
  }
}

export default App;
```

<br>
**주의!!**

history를 props를 통해 전달받는다고 했다.

이때 props를 통해 전달 받으려면 해당 컴포넌트는 반드시 App 컴포넌트에서 Router 컴포넌트에 등록되어 있어야 사용 가능하다.

<br>

**Router에 등록된 컴포넌트의 자식 컴포넌트의 자식 컴포넌트의 ... 자식 컴포넌트라고 하는 3~4단계 밑의 컴포넌트는 아예 history props를 전달 받지 못한다.(App 컴포넌트의 Router에 등록되어있지 않았기 때문에)**

그 자식 컴포넌트까지 props를 전달해야 하는데 자식이 많으면 drilling 패턴이 발생한다.

<br>

**해당 컴포넌트가 Router 컴포넌트에 등록되지 않아도 match, location, history 객체를 사용하는 방법이 2가지 있다.**

1. withRouter
2. useHistory, useMatch, useLocation, useRouteMatch

[두번째 Hooks를 사용한 Routing 방법](https://github.com/FE-Lex-Kim/-TIL/blob/master/React/useHistory%2C%20useLocation%2C%20usePrams%2C%20useRouteMatch.md)

<br>

### withRouter

withRouter 함수는 HoC(Hight-order Component) 이다.

라우트가 사용된 컴포넌트가 아니여도 match, location, history 객체에 접근할수있다.

<br>

match객체는 현재 자신을 보여주고있는 라우트 컴포넌트 기준으로 match가 전달된다.

ex) profiles에 withRouter 컴포넌트를 넣으면

profiles기준으로 match객체가 나온다. 그뒤의 username은 나오지않는다.

```jsx
import React from "react";
import { withRouter } from "react-router-dom";
import WithRouterSample from "./WithRouterSample";

const data = {
  Alex: {
    name: "어진",
    description: "리액트를 좋아하는 개발자",
  },
  James: {
    name: "어진쓰",
    description: "여행을 좋아하는 일반인",
  },
};

const profile = ({ match }) => {
  const { username } = match.params;
  const profile = data[username];
  if (!profile) {
    return <div>존재하지 않는 사용자입니다.</div>;
  }
  return (
    <div>
      <h3>
        {username} {profile.name}
      </h3>
      <p>{profile.description}</p>
      <WithRouterSample />
    </div>
  );
};

export default withRouter(profile);
```

<br>

```jsx
import React from "react";
import { withRouter } from "react-router-dom";

const WithRouterSample = ({ location, match, history }) => {
  return (
    <div>
      <h4>location</h4>
      <textarea
        value={JSON.stringify(location, null, 2)}
        rows={7}
        readOnly={true}
      />
      <h4>match</h4>
      <textarea
        value={JSON.stringify(match, null, 2)}
        rows={7}
        readOnly={true}
      />
      <button onClick={() => history.push("/")}>홈으로</button>
    </div>
  );
};

export default withRouter(WithRouterSample);
```

<br>

### Switch(V6에서 사라짐 대신 <Routes/> 로 변경됨)

Switch 컴포넌트는 여러 Route를 감싸서 그중에서 path(경로)가 일치하는 하나의 Route만을 랜더링 시켜준다.

Switch를 사용했을때 아무 Route와 겹치지 않으면 Not Found 페이지를 구현할수있다.

![React SPA 라우터](../Images/React%20SPA%20라우터/React%20SPA%20라우터-3.png)

<br>

`/profild/:id` 는 `/profile` 보다 위에 존재해야한다.

`/profile` 이 위에 존재한다면 먼저 조건에 겹치기 때문에 뒤에 id값이 path에 존재하더라도 항상 `/profile`만 보여질 것이다.

`/profile:id`가 위에 올라가고 싶다면 exact를 해서 정확하게 그 path라고 명시해주어야한다.

또한 `/` 인 첫 경로도 가장 마지막에 exact를 해주어야 한다.

그 이유는 `/` 는 루트 경로 이므로 모든 조건에 걸리기 떄문에 위에 둔다면 항상 루트 페이지만 보일것이다.

그리고 `exact`를 해주어야한다.

<br>

만약 `/` 보다 위에있는 경로들에 걸리지 않고 아예없는 경로라면 항상 루트 경로인 `/` 에 걸려서 루트 페이지가 보일것이다.

따라서 `exact` 해주고 그 하위에 Path를 정해주지않고 NotFound 컴포넌트를 넣어주면 NotFound 페이지를 구현할 수 있다.

<br>

참고

- 리액트를 다루는 기술 책
- 패스트 캠퍼스 프론트엔드 올인원

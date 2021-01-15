# useHistory, useLocation, usePrams, useRouteMatch

<br>

withRouter 을 통해서 컴포넌트에서 직접 history, match, location을 프롭스로 받아 올 수 있다. 

<br>

하지만 withRouter 대신에 react-router-dom 에서 Hooks을 함수형 컴포넌트에서 인스턴스를 만들어 쉽게 사용할 수 있다.

<br>

## useHistory

```jsx
import { useHistory } from "react-router-dom";

function HomeButton() {
  let history = useHistory();

  function handleClick() {
    history.push("/home");
  }

  return (
    <button type="button" onClick={handleClick}>
      Go home
    </button>
  );
}
```

<br>

useHistory() 으로 인스턴스를 만들어 사용한다.

<br>

## useLocation

```jsx
import React from "react";
import ReactDOM from "react-dom";
import {
  BrowserRouter as Router,
  Switch,
  useLocation
} from "react-router-dom";

function usePageViews() {
  let location = useLocation();
  React.useEffect(() => {
    ga.send(["pageview", location.pathname]);
  }, [location]);
}

function App() {
  usePageViews();
  return <Switch>...</Switch>;
}

ReactDOM.render(
  <Router>
    <App />
  </Router>,
  node
);
```

<br>

마찬가지로 useLocation()을 통해 인스턴스를 생성해 사용한다.

<br>

## usePrams

```jsx
import React from "react";
import ReactDOM from "react-dom";
import {
  BrowserRouter as Router,
  Switch,
  Route,
  useParams
} from "react-router-dom";

function BlogPost() {
  let { slug } = useParams();
  return <div>Now showing post {slug}</div>;
}

ReactDOM.render(
  <Router>
    <Switch>
      <Route exact path="/">
        <HomePage />
      </Route>
      <Route path="/blog/:slug">
        <BlogPost />
      </Route>
    </Switch>
  </Router>,
  node
);
```

<br>

useParams() 로 인스턴스를 생성해 쉽게 parms 조회가 가능하다.

<br>

## useRouteMatch

```jsx
import { useRouteMatch } from "react-router-dom";

function BlogPost() {
  let match = useRouteMatch("/blog/:slug");

  // Do whatever you want with the match...
  return <div />;
}
```

<br>

- 참고 :  [https://reactrouter.com/web/api/Hooks/usehistory](https://reactrouter.com/web/api/Hooks/usehistory)
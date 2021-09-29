# Custom Hook

<br>

Custom Hook을 만들면 **컴포넌트 로직을 함수로 만들어 재사용** 할 수 있다.

<br>

React class에서 컴포넌트끼리 중복되어진 코드들은 두 가지 방법을 통해 해결했다.

1. render props
2. HOC(Higher Order Component)

<br>

이 두가지의 방법은 그다지 좋지 않다.

컴포넌트의 계층화를 만들기 때문에 코드 가독성과 코드 추적화가 힘들어진다.

<br>

Custom Hook을 사용해서 계층화 하지않고 중복되어진 코드를 해결할 수 있다.

<br>

custom hook은 예제코드를 보고 공부하는게 더 이해가 잘간다.

예제코드를 보면서 공부해 보자

<br>

### custom hook 예제

사실 custom '**Hook' 이라는 거창한 이름이 있지만, 중복되는 코드를 제거하기 위한 함수이다.** React의 특별한 기능이 있다기 보다 Hook의 디자인 패턴을 따르는 것이다.

<br>

반드시 hook이다 보니 앞에 `use`로 시작해야한다. `use`로 시작해야 Hook의 규칙의 위반여부를 자동으로 체크할 수 있다.

```jsx
import useWindowWidth from "./useWindowWidth";

const css = {
  width: "100px",
  margin: "0 auto",
};

function App() {
  const width = useWindowWidth();
  return (
    <div style={css}>
      <p>{width}</p>
    </div>
  );
}

export default App;
```

<br>

```jsx
import { useEffect, useState } from "react";

function useWindowWidth() {
  const [width, setWidth] = useState(window.innerWidth);

  const resize = () => {
    setWidth(window.innerWidth);
  };

  useEffect(() => {
    window.addEventListener("resize", resize);

    return () => {
      window.removeEventListener("resize", resize);
    };
  }, []);

  return width;
}

export default useWindowWidth;
```

위의 예제 처럼 custom hook 내부에 **state hook** 이 있더라도 외부의 **state hook과 값을 공유하지 않는다.**

custom hook안에 hook이 들어간다고 해도 상관이 없다.

<br>

위의 useWindowWidth라는 custom hook을 통해서 다른 컴포넌트들에서도 재사용되서 사용할 수 있다.

<br>

## HOC vs custom Hook

HOC와 custom hook은 중복된 코드를 재사용하기 위해 사용하는 패턴이다.

<br>

이 두가지가 어떻게 다른지 비교해 보자.

<br>

다음 예제는 초기에 `false` 값을 `consloe`에 보여주고 이후에 `true` 값으로 변경해주는 **HOC**와 **Custom Hook**인 코드이다.

<br>

### HOC

App.js

```jsx
import logo from "./logo.svg";
import useWindowWidth from "./useWindowWidth";
import withHasMounted from "./withHasMounted";
const css = {
  width: "100px",
  margin: "0 auto",
};

function App({ hasMounted }) {
  const width = useWindowWidth();

  console.log(hasMounted);
  return (
    <div style={css}>
      <p>{width}</p>
    </div>
  );
}

export default withHasMounted(App);
```

<br>

withHasMounted.jsx

```jsx
import React from "react";

function withHasMounted(Component) {
  class NewComponent extends React.Component {
    state = {
      hasMounted: false,
    };

    render() {
      const { hasMounted } = this.state;
      return <Component {...this.props} hasMounted={hasMounted} />;
    }

    componentDidMount() {
      this.setState({ hasMounted: true });
    }
  }

  NewComponent.displayName = `withHasMounted(${Component.name})`;

  return NewComponent;
}

export default withHasMounted;
```

<br>

최초에 App 컴포넌트를 렌더링 했다가 이후에 `state`가 변경되어서 처음에 `false` 값이, 이후에 `true` 값이 `console`에 보여진다.

<br>

### custom Hook

useHasMounted.js

```jsx
import { useEffect, useState } from "react";

function useHasMounted() {
  const [hasMounted, setHasMounted] = useState(false);

  useEffect(() => {
    setHasMounted(true);
  }, []);

  return hasMounted;
}

export default useHasMounted;
```

<br>

App.jsx

```jsx
import useHasMounted from "./useHasMounted";
import useWindowWidth from "./useWindowWidth";
import withHasMounted from "./withHasMounted";
const css = {
  width: "100px",
  margin: "0 auto",
};

function App({ hasMounted }) {
  const width = useWindowWidth();
  const hasMounted2 = useHasMounted();

  console.log(hasMounted);
  console.log(hasMounted2);
  return (
    <div style={css}>
      <p>{width}</p>
    </div>
  );
}

export default withHasMounted(App);
```

<br>

처음에 useHasMounted가 호출되고 false 값이 console에 나온다.

이후 render 단계이후에 ComponentDidMount에 발생하는 setHasMounte가 호출되어 state가 변경된다.

다시 컴포넌트가 재호출되고 state이 변경된 값이 반환된다.

<br>

따라서 위의 HOC와 Custom Hook도 동일하게 동작한다.

<br>

## 정리

최근에는 HOC에서 Custom Hook으로 점점 변화하고 있다.

위의 예제들만 봐도 HOC는 계층화가 심해지고 코드의 복잡도, 추적이 어렵다.

<br>

HOC와 custom hook을 비교해서 어떤점이 차이가 있는지 알고 사용하면 더욱 이해가 잘될 것이다.

<br>

참고

- [https://ko.reactjs.org/docs/hooks-custom.html](https://ko.reactjs.org/docs/hooks-custom.html)

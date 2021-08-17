# Error Boundaries

<br>

**자바스립트의 에러가 React의 에러까지 이어지면서 애플리케이션이 정상적으로 동작하지 않게 되었다.**

이렇게 자바스크립트 에러가 전체 애플리케이션을 중단시키면 안되기에

따라서 error Boundaries 라는 개념을 도입했다.

<br>

error Boundaries 는 하위 컴포넌트 어디에서든 자바스크립트 에러를 기록하며 이렇게 에러가난 컴포넌트 대신 폴백 UI를 보여주는 React 컴포넌트 이다.

**즉, 에러가 발생한 컴포넌트 대신 들어가는 컴포넌트**

<br>

error Boundaries는 4가지의 경우 포착하지 않는다.

1. 이벤트 핸들러
2. 비동기적 코드
3. 서버 사이드 렌더링
4. error boudary 자체에서 에러

<br>

## 사용방법

`getDeribedStateFromError()` 와 `ComponentDidCatch()` 중 하나를 정의하면 해당 클래스 컴포넌트 자체가 error Boundaries가 된다.

<br>

### static getDerivedStateFromError()

<br>

하위 자손 컴포넌트에서 오류가 발생했을 때 호출된다.

이 메서드는 매개변수로 에러를 전달받고, 에러가 났는지 확인하는 state값을 반드시 반환해야한다.

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // state를 갱신하여 다음 렌더링에서 대체 UI를 표시합니다.
    return { hasError: true };
  }

  render() {
    if (this.state.hasError) {
      // 별도로 작성한 대체 UI를 렌더링할 수도 있습니다.
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children;
  }
}
```

<br>

### componentDidCatch()

자손 컴포넌트에서 에러가 발생했을때 호출되며, 2개의 매개변수를 전달받는다.

1. error 발생한 오류
2. info 어떤 컴포넌트가 오류를 발생했는지에 대한 정보

<br>

컴포넌트 생명주기중 'commit' 단계에서 호출되므로 DOM 트리가 이미 다 작성된 이후이므로 side Effect를 발생시켜도된다.

에러 로그 기록등을 위해사용하면 된다.

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // state를 갱신하여 다음 렌더링에서 대체 UI를 표시합니다.
    return { hasError: true };
  }

  componentDidCatch(error, info) {
    // Example "componentStack":
    //   in ComponentThatThrows (created by App)
    //   in ErrorBoundary (created by App)
    //   in div (created by App)
    //   in App
    logComponentStackToMyService(info.componentStack);
  }

  render() {
    if (this.state.hasError) {
      // 별도로 작성한 대체 UI를 렌더링할 수도 있습니다.
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children;
  }
}
```

<br>

정리

- `getDerivedStateFromError`는 에러가 났는지에 대한 state값을 관리 하여 UI 렌더링
- `ComponentDidCatch` 는 에러 기록 및 에러가 났는지에 대한 state값을 관리 가능
- 에러 경계는 트리 내에서 하위에 존재하는 컴포넌트의 에러만을 포착.

<br>

react.docs에는 다음과 같은 구문이 있다.

> Use static getDerivedStateFromError() to render a fallback UI after an error has been thrown. Use componentDidCatch() to log error information.

따라서 에러 state를 관리할땐 `getDerivedStateFromError` 사용, error infromation을 기록할땐 `ComponentDidCatch`

<br>

## error Boundaries가 생긴의미

<br>

react에서는 논의를 한 결과로 사용자 경험으로 봤을때, 에러가난 UI를 완전히 제거하는 것보다 그대로 남겨두는 것이 더 좋지않다.

<br>

따라서 사용자 경험관점에서 error boundaries를 추가함으로써 더 나은 사용자 경험을 제공한다.

<br>

예를들어 페이스북 메신저에서 사이드 바, 정보, 대화 기록, 메시지 등등 error boundaries로 감싸 에러를 대체하였다.

<br>

또한 자바스크립트 에러 리포팅 서비스를 활용하게 하여 해당에러를 학습하게 될 수도 있다.

<br>

## try / catch 문과 비교

`try / catch` 문도 좋지만 명령형 코드에서만 동작한다고 한다.

```jsx
try {
  showButton();
} catch (error) {
  // ...
}
```

<br>

그러나 React 컴포넌트는 선언적이 프로그래밍을 하므로 **무엇을 렌더링 할지를 구체화한다.**

```jsx
<Button />
```

<br>

error boundaries는 선언적인 특성을 보존하고 예상대로 동작한다.

<br>

- 선언형 : 무엇을 하는지에 대한지 보여줌
- 명령형 : 하는것에 대한 과정을 모두 보여줌, 상태가 변화하는 것에 대한 상황을 계산해서 보여줌

<br>

## 이벤트 핸들러는?

error boundaries는 이벤트 핸들러 내부를 포착하지 않는다.

이벤트 핸들러는 렌더링 도중에 발생하지 않는다.

이미 렌더트리가 다 완성된후 브라우저에서 동작한다.

<br>

따라서 이벤트 핸들러의 에러를 잡으려면 try / catch 문을 사용해야한다.

```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = { error: null };
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    try {
      // 에러를 던질 수 있는 무언가를 해야합니다.
    } catch (error) {
      this.setState({ error });
    }
  }

  render() {
    if (this.state.error) {
      return <h1>Caught an error.</h1>;
    }
    return <button onClick={this.handleClick}>Click Me</button>;
  }
}
```

<br>

참고

- [https://ko.reactjs.org/docs/react-component.html#static-getderivedstatefromerror](https://ko.reactjs.org/docs/react-component.html#static-getderivedstatefromerror)
- [https://ko.reactjs.org/docs/error-boundaries.html](https://ko.reactjs.org/docs/error-boundaries.html)
- [https://stackoverflow.com/questions/52962851/whats-the-difference-between-getderivedstatefromerror-and-componentdidcatch](https://stackoverflow.com/questions/52962851/whats-the-difference-between-getderivedstatefromerror-and-componentdidcatch)
- [https://stackoverflow.com/questions/1784664/what-is-the-difference-between-declarative-and-imperative-paradigm-in-programmin](https://stackoverflow.com/questions/1784664/what-is-the-difference-between-declarative-and-imperative-paradigm-in-programmin)

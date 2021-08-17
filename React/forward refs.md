# Forwarding Refs

<br>

DOM 요소를 직접 얻어 사용할때 ref를 사용할수 있다.

Forwarding ref를 통해 컴포넌트의 내부의 어떠한 HTML 요소의 DOM을 얻을 수있다.

<br>

방법은 다음과 같다.

```jsx
const FancyButton = React.forwardRef((props, ref) => (
  <button ref={ref} className="FancyButton">
    {" "}
    {props.children}
  </button>
));

// 이제 DOM 버튼으로 ref를 작접 받을 수 있습니다.
const ref = React.createRef();
<FancyButton ref={ref}>Click me!</FancyButton>;
```

<br>

1. React.creatRef를 호출해서 변수에 할당
2. ref를 컴포넌트에 전달
3. forwarRef의 내부에 두번째 인자로 전달한다.
4. 이후 ref를 html 엘리먼트에 `ref={ref}` 로 전달
5. 이제 ref.current로 html 엘리먼트 DOM 을 얻을수있음

<br>

### 고차 컴포넌트에서의 ref 전달

HOC 에서 ref를 전달하는 방법

```jsx
export function withClose(Component) {
  class ClickContainer extends React.Component {
    constructor() {
      super();
      this.handleClose = this.handleClose.bind(this);
    }

    componentDidMount() {
      document.addEventListener("click", this.handleClose);
    }

    componentWillUnmount() {
      document.removeEventListener("click", this.handleClose);
    }

    handleClose(e) {
      const { forwardRef } = this.props;
      console.log(forwardRef);
    }

    render() {
      const { forwardRef, ...rest } = this.props;
      return <Component forwardRef={forwardRef} {...rest} />;
    }
  }

  return React.forwardRef((props, ref) => {
    return <ClickContainer {...props} forwardRef={ref} />;
  });
}
```

<br>

```jsx
const CloseablePopup = withClose(Popup);

class App extends Component {
  popupRef = React.createRef();
  render() {
    return <CloseablePopup ref={popupRef} name="Closable Popup" />;
  }
}
```

<br>

HOC 에서 컴포넌트 내부에 ref를 전달한다.

1. App에서 React.create를 호출한뒤 변수에 저장한다.
2. HOC에 해당 변수를 props로 전달한다.
3. HOC 내부에서 React forwardRef를 호출하여 내부의 콜백함수의 두번째 인자를 HOC의 컴포넌트에게 전달한다.
4. 내부 original 컴포넌트에게 ref를 전달한다.

<br>

ref 전달 정리

App → HOC → forwardRef 내부의 컴포넌트 → original Componet

<br>

참고

- [https://stackoverflow.com/questions/53849369/react-forwardref-hoc-not-giving-reference-to-container-element](https://stackoverflow.com/questions/53849369/react-forwardref-hoc-not-giving-reference-to-container-element)
- [https://ko.reactjs.org/docs/forwarding-refs.html](https://ko.reactjs.org/docs/forwarding-refs.html)

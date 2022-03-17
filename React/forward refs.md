- [Forwarding Refs](#forwarding-refs)
  - [(추가)함수형 컴포넌트에서 forwardRef](#추가함수형-컴포넌트에서-forwardref)
  - [고차 컴포넌트에서의 ref 전달](#고차-컴포넌트에서의-ref-전달)

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

## (추가)함수형 컴포넌트에서 forwardRef

ref는 직접 DOM을 조작할때 사용한다고 했다.

만약 조작하려는 자식 컴포넌트의 DOM을 조작하려고 한다면, ref가 전달되어 사용되지 않는다.

<br>

일반 요소(ref를 등록해서 사용가능하다.)

```jsx
function App() {
  const submitRef = useRef(null);

  return;
  <button ref={submitRef} onKeyDown={submitKeyDown}>
    제출
  </button>;
}
```

<br>

자식 컴포넌트(ref를 props로 전달되지 않는다.)

```jsx
function App() {
  const submitRef = useRef(null);

  <CustomInput ref={submitRef} onKeyDown={submitKeyDown}>
    제출
  </CustomInput>;
}
```

<br>

자식 컴포넌트의 DOM을 조작하려면, React.forwardRef를 사용해야한다.

```jsx
// props 목록 뒤에 ref를 별도로 전달받는 모습입니다.
const CustomInput = React.forwardRef(({ type, onKeyDown, placeholder }, ref) => {
  return (
    <input
      type={type}
      onKeyDown={onKeyDown}
      placeholder={placeholder}
      // 전달받은 ref는 HTML 속성으로 전달됩니다.
      ref={ref}
    ></input>
  );
});

export default CustomInput;
```

<br>

App.jsx(이제 사용 가능하다.)

```jsx
function App() {
  const inputRef = useRef(null);

  return (
    <>
      <CustomInput ref={inputRef} />
    </>
  );
}
```

<br>

## 고차 컴포넌트에서의 ref 전달

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
- [https://merrily-code.tistory.com/121](https://merrily-code.tistory.com/121)
- [https://www.daleseo.com/react-forward-ref/](https://www.daleseo.com/react-forward-ref/)

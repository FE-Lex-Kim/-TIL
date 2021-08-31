# Render Props

<br>

컴포넌트에게 컴포넌트 또는 엘리먼트를 전달하는 방법이 있다.

이것을 'render props' 이라고 한다.

<br>

## 사용 방법

사용 방법은 굉장히 쉽다.

컴포넌트에서 컴포넌트로 데이터를 넘길때는 props를 통해 전달한다.

똑같이 props를 통해 전달 하면된다.

하지만 전달할때 컴포넌트 or 엘리먼트를 그대로 적어서 전달하지 않고 이러한 값들을 반환하는 함수를 전달해준다.

<br>

말은 어렵지만 코드를 보면 이해하기 굉장히 쉽다.

```jsx
<DataProvider render={(data) => <h1>Hello {data.target}</h1>} />
```

<br>

render props를 사용하는 라이브러리들은 React Router, Formik 등등이 있다.

## 언제 사용되어질까?

컴포넌트의 재사용성과 유지보수 및 추상화를 수월하게 해준다.

<br>

컴포넌트 내부에서 항상 정해진 컴포넌트와 엘리먼트가 렌더링이 되는것이 아니라 부모 컴포넌트에서 props를 통해 전달받은 컴포넌트 또는 엘리먼트가 렌더링 되게 해준다.

<br>

즉, 추상화가 가능하고 그에 따라 유지 보수 및 재사용성이 매우 늘어나게 된다.

<br>

예시를 보면 더 쉽게 이해 가능하다.

```jsx
class MouseTracker extends React.Component {
  constructor(props) {
    super(props);
    this.handleMouseMove = this.handleMouseMove.bind(this);
    this.state = { x: 0, y: 0 };
  }

  handleMouseMove(event) {
    this.setState({
      x: event.clientX,
      y: event.clientY,
    });
  }

  render() {
    return (
      <div style={{ height: "100vh" }} onMouseMove={this.handleMouseMove}>
        <h1>Move the mouse around!</h1>
        <p>
          The current mouse position is ({this.state.x}, {this.state.y})
        </p>
      </div>
    );
  }
}
```

위의 예시는 마우스를 움직히면 브라우저에서 현재 마우스 위치에 대한 정보를 텍스트로 보여준다.

<br>

![](../Images/Render%20Props/Render-props-1.png)

<br>

만약 마우스 주의에 고양이 그림을 보여주는 작업을 하려고한다.

이 작업을 수행하려면 기존의 MouseTracker 컴포넌트에서 반환해주는 JSX를 변경직접 변경해주어야한다.

```jsx
class Cat extends React.Component {
  render() {
    const mouse = this.props.mouse;
    return (
      <img
        src="/cat.jpg"
        style={{ position: "absolute", left: mouse.x, top: mouse.y }}
      />
    );
  }
}

class MouseWithCat extends React.Component {
  constructor(props) {
    super(props);
    this.handleMouseMove = this.handleMouseMove.bind(this);
    this.state = { x: 0, y: 0 };
  }

  handleMouseMove(event) {
    this.setState({
      x: event.clientX,
      y: event.clientY,
    });
  }

  render() {
    return (
      <div style={{ height: "100vh" }} onMouseMove={this.handleMouseMove}>
        {/*
          여기서 <p>를 <Cat>으로 바꿀 수 있습니다. ... 그러나 이 경우
          Mouse 컴포넌트를 사용할 때 마다 별도의 <MouseWithSomethingElse>
          컴포넌트를 만들어야 합니다, 그러므로 <MouseWithCat>는
          아직 정말로 재사용이 가능한게 아닙니다.
        */}
        <Cat mouse={this.state} />
      </div>
    );
  }
}

class MouseTracker extends React.Component {
  render() {
    return (
      <div>
        <h1>Move the mouse around!</h1>
        <MouseWithCat />
      </div>
    );
  }
}
```

이렇게 하드코딩을 통해 만든다면 컴포넌트의 재사용성이 전혀 발휘되지 못한다.

만약 다른 이미지 또는 마우스 위치에 따른 다른 기능을 넣는다면 매번 다른 컴포넌트들 만들어주어야한다.

<br>

이때 render prop를 사용하면 Mouse 컴포넌트 안에 Cat 컴포넌트를 부모컴포넌트에서 전달해주어 하드코딩할 필요가 없어진다.

```jsx
class Cat extends React.Component {
  render() {
    const mouse = this.props.mouse;
    return (
      <img
        src="/cat.jpg"
        style={{ position: "absolute", left: mouse.x, top: mouse.y }}
      />
    );
  }
}

class Mouse extends React.Component {
  constructor(props) {
    super(props);
    this.handleMouseMove = this.handleMouseMove.bind(this);
    this.state = { x: 0, y: 0 };
  }

  handleMouseMove(event) {
    this.setState({
      x: event.clientX,
      y: event.clientY,
    });
  }

  render() {
    return (
      <div style={{ height: "100vh" }} onMouseMove={this.handleMouseMove}>
        {/*
          <Mouse>가 무엇을 렌더링하는지에 대해 명확히 코드로 표기하는 대신,
          `render` prop을 사용하여 무엇을 렌더링할지 동적으로 결정할 수 있습니다.
        */}
        {this.props.render(this.state)}
      </div>
    );
  }
}

class MouseTracker extends React.Component {
  render() {
    return (
      <div>
        <h1>Move the mouse around!</h1>
        <Mouse render={(mouse) => <Cat mouse={mouse} />} />
      </div>
    );
  }
}
```

MouseTracker 라는 부모 컴포넌트에서 Mouse 컴포넌트에 render prop으로 Cat 컴포넌트를 반환하는 함수를 전달했다.

이때 함수의 파라미터는 Mouse 컴포넌트 내부에서 함수를 실행시켜 그 인자 값을 전달시켜준다.

<br>

즉, 공통된 state 또는 로직을 Mouse에 적어준다.

이후 JSX로 반환되어질 컴포넌트 또는 엘리먼트를 props로 전달받아 넣어주면 된다.

<br>

### 주의사항

React.PureComponent에서 render porps pattern을 사용했을때 주의 해야한다.

<br>

```jsx
class Mouse extends React.PureComponent {
  // 위와 같은 구현체...
}

class MouseTracker extends React.Component {
  render() {
    return (
      <div>
        <h1>Move the mouse around!</h1>

        {/*
          이것은 좋지 않습니다! `render` prop이 가지고 있는 값은
          각각 다른 컴포넌트를 렌더링 할 것입니다.
        */}
        <Mouse render={(mouse) => <Cat mouse={mouse} />} />
      </div>
    );
  }
}
```

PurComponent는 얕은 비교만 하므로 render-props로 전달한 함수가 이전의 값과 동일하지 않는다고 판단한다.(매번 render()를 호출하므로 함수의 참조값이 달라짐)

<br>

따라서 코드를 다음과 같이 고쳐야한다.

```jsx
class MouseTracker extends React.Component {
  // `this.renderTheCat`를 항상 생성하는 매서드를 정의합니다.
  // 이것은 render를 사용할 때 마다 *같은* 함수를 참조합니다.
  renderTheCat(mouse) {
    return <Cat mouse={mouse} />;
  }

  render() {
    return (
      <div>
        <h1>Move the mouse around!</h1>
        <Mouse render={this.renderTheCat} />
      </div>
    );
  }
}
```

인스턴스는 변경되지 않기에 renderTheCat을 미리 선언해놓아서 참조값이 그대로 이므로 변경되지 않음을 알아차릴 수 있다.

<br>

## HOC vs Render Props

### 공통점

두 가지 패턴모두 중복된 코드를 제거한다는 점이다.

<br>

### 차이점

HOC의 단점

1. 속성의 값을 암묵적으로 전달한다.(가독성 저하)
   - 예를들어 `Redux`에서 `connect HOC`를 사용하면 `dispatch` 사용 가능하다.
2. 동명의 props를 생성하고 이런 props를 사용시 충돌하여 overwrite가 된다.
   - 서로 다른 HOC를 중복적으로 중첩해서 사용했다고 하자.
   - 이때 HOC의 props가 다른 HOC의 props의 네이밍이 동일하다면 충돌이 발생하여 그 하위의 props로 overwrite가 발생하게 된다.

<br>

render-props의 단점

1. 코드의 방식이 지저분 해질수도 있다.(가독성 저하?)
   - render props를 중복적으로 넘겨준다고 할때 코드의 가독성이 굉장히 떨어질수도 있다?
   - 개인적인 생각입니다.

<br>

이러한 차이점으로 인해서 render-props는 HOC가 가지고 있는 단점을 모두 커버해준다.

따라서 개인적으로는 HOC보다 render-props를 사용하는 것이 더 좋다고 생각한다.

<br>

> So go ahead, try out render props in your codebase! Go and find some HOC and turn it into a regular component with a render prop.

그리고 실제로 React 공식문서에서 Render-props의 설명하는 파트의 중간부분에 추천해주는 blog의 내용중에는 HOC를 Render-props로 변경하는것을 권장한다는 내용이 있다.

<br>

참고

- [https://ko.reactjs.org/docs/render-props.html](https://ko.reactjs.org/docs/render-props.html)
- [https://medium.com/@mjackson/use-a-render-prop-50de598f11ce](https://medium.com/@mjackson/use-a-render-prop-50de598f11ce)
- [https://nakta.dev/react-hoc-render-prop](https://nakta.dev/react-hoc-render-prop)
- [https://velog.io/@hwang-eunji/React-고차-컴포넌트-HOC-사용하기](https://velog.io/@hwang-eunji/React-%EA%B3%A0%EC%B0%A8-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-HOC-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)

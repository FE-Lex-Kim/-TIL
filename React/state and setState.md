# State and setState

<br>

prop를 통해 얻는 정보는 상위 컴포넌트에서 전달받으므로 컴포넌트의 자체적인 기능을 유지하지 못하고 항상 상위 컴포넌트에게 의존성을 가지게 된다.

<br>

> State는 props와 유사하지만, 비공개이며 컴포넌트에 의해 완전히 제어됩니다.

<br>

이런점을 해결하기 위해선 컴포넌트 스스로 state을 가지고 있으면 된다.

<br>

> render 메서드는 업데이트가 발생할 때마다 호출되지만, 같은 DOM 노드로 <Clock />을 렌더링하는 경우 Clock 클래스의 단일 인스턴스만 사용됩니다.

<br>

클래스 형 컴포넌트는 해당 클래스 컴포넌트가 업데이트 되었을때, render 메서드가 호출된다.

이때 클래스 자체가 호출되는 것이아니라, 클래스가 생성한 하나의 인스턴스의 render 메서드만 호출된다.

- 함수형 컴포넌트는 렌더링이 되면 컴포넌트 자체, 내부가 모두 호출되어진다.

<br>

따라서 해당 인스턴스의 내부 state와 생명주기 메서드는 영향이 가지 않기에 부가적인 기능을 사용할 수 있다.

<br>

## 클래스에 로컬 state 추가하기

클래스에서 로컬 state를 추가하기 위해 클래스형 컴포넌트의 constructor 내부에서 변수를 선언해야한다.

```jsx
constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }
```

<br>

[메서드를 바인딩하거나 state를 초기화하는 작업이 없다면, 해당 React 컴포넌트에는 constructor를 구현하지 않아도된다.](https://ko.reactjs.org/docs/react-component.html#constructor)

<br>

내부에 state나 다른 구문을 추가하기 앞서서 `super(props)` 를 먼저 호출해야한다.

그렇지 않으면 this.props가 constructor 내에서 정의되지 않아 버그로 이어진다.

<br>

## state를 올바르게 사용하기

<br>

### 직접 State를 수정하지 않는다.

```jsx
this.state.comment = "hello";
```

위와 같이 state를 변경했다 하더라도 컴포넌트는 렌더링이 되지 않았기 때문에, 내부의 UI에 해당 state를 사용한 부분은 변경되지 않는다.

<br>

따라서 setState()를 사용해서 this.state를 변경한뒤, 렌더링되게 하는 방법을 사용해야한다.

```jsx
this.setState({ commnet: "hello" });
```

<br>

### State 업데이트는 비동기적일 수도 있다.

React에서는 성능을 위해서 setState() 호출을 한꺼번에 처리할 수 있다고 했다.

<br>

만약 this.props 와 this.state 값이 따로 업데이트가 되어질 수 있기때문에 state와 props의 값을 연산하여 state를 갱신하려고 할때 설계했던대로 동작하지 않을 수 있다.

<br>

ex)

```jsx
this.setState({
  cunter: this.state.counter + this.props.increament,
});
```

<br>

이런점을 수정하기 위해서 함수를 인자로 받아 setState() 를 호출해야한다.

내부에 콜백함수의 첫번째 인자는 state를 받고, 두번째 인자는 업데이트가 적용된 시점의 props를 받는다.

```jsx
this.setState((state, props) => ({
  counter: state.counter + props.increament,
}));
```

<br>

### state 업데이트는 병합된다.

setState()를 호출하면 내부의 객체를 인수로 전달한 객체 값을 현재 state에 병합한다.

즉, 덮어씌우기가 아니라 병합하여 사용한다.

ex)

```jsx
constructor(props) {
    super(props);
    this.state = {
      posts: [],
      comments: []
    };
  }
```

<br>

```jsx
componentDidMount() {
    fetchPosts().then(response => {
      this.setState({
        posts: response.posts
      });
    });

    fetchComments().then(response => {
      this.setState({
        comments: response.comments
      });
    });
  }
```

<br>

참고

- [https://ko.reactjs.org/docs/state-and-lifecycle.html](https://ko.reactjs.org/docs/state-and-lifecycle.html)
- [https://ko.reactjs.org/docs/react-component.html#constructor](https://ko.reactjs.org/docs/react-component.html#constructor)
- [https://www.zerocho.com/category/React/post/579b5ec26958781500ed9955](https://www.zerocho.com/category/React/post/579b5ec26958781500ed9955)

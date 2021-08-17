# High Order Component 고차 컴포넌트

High Order Component는 컴포넌트 로직을 재사용하기 위한 기술이다.

react의 컴포넌트 구성에서 나오는 패턴이다.

**High Order Component는 컴포넌트를 가져와 새 컴포넌트를 반환하는 함수이다.**

<br>

```jsx
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```

<br>

High Order Component는 Reudx의 connect와 같이 다른 라이브러리에서도 흔하게 찾아볼 수 있다.

<br>

## HOC 사용해보기

예제 코드)

CommentList

```jsx
class CommentList extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {
      // "DataSource" 는 글로벌 데이터 소스입니다.
      comments: DataSource.getComments(),
    };
  }

  componentDidMount() {
    // 변화감지를 위해 리스너를 추가합니다.
    DataSource.addChangeListener(this.handleChange);
  }

  componentWillUnmount() {
    // 리스너를 제거합니다.
    DataSource.removeChangeListener(this.handleChange);
  }

  handleChange() {
    // 데이터 소스가 변경될때 마다 comments를 업데이트합니다.
    this.setState({
      comments: DataSource.getComments(),
    });
  }

  render() {
    return (
      <div>
        {this.state.comments.map((comment) => (
          <Comment comment={comment} key={comment.id} />
        ))}
      </div>
    );
  }
}
```

<br>

BlogPost

```jsx
class BlogPost extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {
      blogPost: DataSource.getBlogPost(props.id),
    };
  }

  componentDidMount() {
    DataSource.addChangeListener(this.handleChange);
  }

  componentWillUnmount() {
    DataSource.removeChangeListener(this.handleChange);
  }

  handleChange() {
    this.setState({
      blogPost: DataSource.getBlogPost(this.props.id),
    });
  }

  render() {
    return <TextBlock text={this.state.blogPost} />;
  }
}
```

<br>

CommentList와 BlogPost는 State에 들어가는 값만 다르다.

그 이외의 부분은 모두 같기때문에 불필요한 코드 낭비가 생기게된다.

<br>

따라서 이런 중복되어지는 코드를 제거하고 HOC에 넣어준다.

그리고 각각 컴포넌트별로 다른 부분을 컴포넌트에서 정의하면된다.

```jsx
// 이 함수는 컴포넌트를 매개변수로 받고..
function withSubscription(WrappedComponent, selectData) {
  // ...다른 컴포넌트를 반환하는데...
  return class extends React.Component {
    constructor(props) {
      super(props);
      this.handleChange = this.handleChange.bind(this);
      this.state = {
        data: selectData(DataSource, props),
      };
    }

    componentDidMount() {
      // ... 구독을 담당하고...
      DataSource.addChangeListener(this.handleChange);
    }

    componentWillUnmount() {
      DataSource.removeChangeListener(this.handleChange);
    }

    handleChange() {
      this.setState({
        data: selectData(DataSource, this.props),
      });
    }

    render() {
      // ... 래핑된 컴포넌트를 새로운 데이터로 랜더링 합니다!
      // 컴포넌트에 추가로 props를 내려주는 것에 주목하세요.
      return <WrappedComponent data={this.state.data} {...this.props} />;
    }
  };
}
```

<br>

```jsx
const CommentListWithSubscription = withSubscription(
  CommentList,
  (DataSource) => DataSource.getComments()
);

const BlogPostWithSubscription = withSubscription(
  BlogPost,
  (DataSource, props) => DataSource.getBlogPost(props.id)
);
```

<br>

## 정리

만약 A 와 B 컴포넌트가 있을때 두 컴포넌트가 비슷한 방법으로 사용하는 로직이라면 통합된 로직을 HOC에만 정의해 놓는다.

<br>

A와 B에서는 다른 부분만을 각각 가지고 있게 해서 코드의 가독성이 좋다.

또한 유지보수를 할때 HOC에서 수정하면 모든 컴포넌트를 한번에 적용할 수 있다는 장점이 있다.

<br>

정리

1. 보이지 않는 메서드를 전달가능
2. 공통코드를 HOC에 저장한다
3. 공통되지 않은 코드를 따로 컴포넌트에 작성

   → 재사용성, 유지보수 높아짐

<br>

HOC는 래핑된 컴포넌트에게 props를 제공하기만 하면 된다.

HOC는 인수로 컴포넌트와 컴포넌트별로 공통되지 않는 부분만을 인수로 받아서 전달함으로써 서로다른 컴포넌트를 반환하게 한다.

<br>

## 컨벤션 : 네이밍

'HOC 이름 + 래핑된 컴포넌트 이름' 으로 HOC컴포넌트를 사용해서 반환된 컴포넌트 이름을 만든다.

<br>

### render 메서드 내부에서 HOC를 사용하지 않기

내부에서 사용하면 성능상 문제가 생기고 다시 마운트가 되면서 컴포넌트의 state와 하위 컴포넌트가 고장날 수 있다.

```jsx
render() {
  // render가 호출될 때마다 새로운 버전의 EnhancedComponent가 생성됩니다.
  // EnhancedComponent1 !== EnhancedComponent2
  const EnhancedComponent = enhance(MyComponent);
  // 때문에 매번 전체 서브트리가 마운트 해제 후 다시 마운트 됩니다!
  return <EnhancedComponent />;
}
```

<br>

따라서 컴포넌트의 정의 바깥에서 HOC를 적용하여 컴포넌트가 한번만 생성되돌고 한다.

<br>

참고

- [https://ko.reactjs.org/docs/higher-order-components.html](https://ko.reactjs.org/docs/higher-order-components.html)
- [https://velopert.com/3537](https://velopert.com/3537)
- [https://liketech.codes/higher-order-component/](https://liketech.codes/higher-order-component/)

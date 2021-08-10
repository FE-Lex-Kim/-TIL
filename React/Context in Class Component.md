# Context

컴포넌트 내부에서는 중첩이 가능하다보니 굉장히 상위에 있는 부모 컴포넌트의 props를 받아올때 그 중간 단계의 컴포넌트들도 모두 해당 props를 받아서 전달해야 했다.

<br>

이러한 일일이 props를 전달하지 않게 하는 방법으로 도입된게 Context이다.

<br>

2가지 방법이 있다.

첫번째로 컴포넌트를 끌어올려 부모 요소의 변수에 해당 컴포넌트들을 JSX로 할당한다. 이후 props로 전달해 원래 위치하고 싶었던 위치에 해당 컴포넌트들을 넣는다.

하지만 복잡한 로직을 상위에 옮기면 이 상위 컴포넌트들은 더 난해해지고 가독성이 나빠져 유지보수에도 힘들어진다.

<br>

따라서 두번째인 Context를 사용해서 하위 컴포넌트에게 값을 전달하는 방법이 있다.

<br>

첫번째 방법부터 보자

## 컴포넌트를 끌어올려서 사용하기

여러 레벨에 걸쳐 props 넘기는 걸 대체하는 데에 context보다 컴포넌 더 간단한 해결책일 수도 있다.

```jsx
<Page user={user} avatarSize={avatarSize} />
// ... 그 아래에 ...
<PageLayout user={user} avatarSize={avatarSize} />
// ... 그 아래에 ...
<NavigationBar user={user} avatarSize={avatarSize} />
// ... 그 아래에 ...
<Link href={user.permalink}>
  <Avatar user={user} size={avatarSize} />
</Link>
```

<br>

Avatar 컴포넌트 자체를 넘겨주면 context를 사용하지 않고 이를 해결할 수 있다.

그러면 중간에 있는 컴포넌트들이 user나 avatarSize 에 대해 전혀 알 필요가 없다.

<br>

```jsx
function Page(props) {
  const user = props.user;
  const userLink = (
    <Link href={user.permalink}>
      <Avatar user={user} size={props.avatarSize} />
    </Link>
  );
  return <PageLayout userLink={userLink} />;
}

// 이제 이렇게 쓸 수 있습니다.
<Page user={user} avatarSize={avatarSize} />
// ... 그 아래에 ...
<PageLayout userLink={...} />
// ... 그 아래에 ...
<NavigationBar userLink={...} />
// ... 그 아래에 ...
{props.userLink}
```

<br>

복잡한 로직을 상위로 옮기면 이 상위 컴포넌트들은 더 난해해지기 마련이고 하위 컴포넌트들은 필요 이상으로 유연해져야한다.

<br>

따라서 두번째인 Context를 사용해서 하위 컴포넌트에게 값을 전달하는 방법이 있다.

<br>

## Context

<br>

Cotext의 메소드는 크게 4가지가 있다.

1. React.creatContext
2. Context.Provider
3. Context.Consumer
4. Class.contextType

<br>

### React.createContext

```jsx
const MyContext = React.createContext(defaultValue);
```

첫번째인 React.creactContext는 context 만드는 메소드이다.

context를 만들어서 초기값을 인수에 전달에 놓을 수 있다.

만약 Provider가 없다면 이 초기값을 가지게 된다.

<br>

### Context.Provider

```jsx
<MyContext.Provider value={/* 어떤 값 */}>
```

두번째인 Context.Provider은 이렇게 만든 Context의 값을 하위 컴포넌트에게 전달할때 사용한다.

따라서 Context 값을 받을 컴포넌트의 상위 컴포넌트에 Context.Provider 엘리먼트를 만들면된다.

<br>

Provioder는 자신의 Consumer로 구독한 자식 컴포넌트중에서 value값을 전달받아 사용할 수 있게된다.

<br>

### Context.Consumer

```jsx
<MyContext.Consumer>
  {value => /* context 값을 이용한 렌더링 */}
</MyContext.Consumer>
```

세번째인 Context.Consumer은 context 값을 받고 싶은 컴포넌트 내부에서 사용하면된다.

Context.Consumer을 JSX위에서 감싸주면된다.

이후 중괄호 안에 함수를 전달해야하고 이 함수의 파라미터 값이 context의 값이 된다.

<br>

### Class.contextType

```jsx
class MyClass extends React.Component {
  componentDidMount() {
    let value = this.context;
    /* MyContext의 값을 이용한 코드 */
  }
  componentDidUpdate() {
    let value = this.context;
    /* ... */
  }
  componentWillUnmount() {
    let value = this.context;
    /* ... */
  }
  render() {
    let value = this.context;
    /* ... */
  }
}
MyClass.contextType = MyContext;
```

네번째인 Class.ContextType은 Context.Consumer이 나온 이후 나왔다.

클래스 컴포넌트 내부에서 Context.Consumer을 사용하기 복잡하고 어렵다는 개발자들의 피드백을 받아 만들었다.

<br>

따라서 사용하려는 컴포넌트 외부에서 단순히 Class.ContextType값에 Context를 할당하면 컴포넌트의 프로퍼티(this.context)에서 context값을 쉽게 사용할 수 있다.

<br>

### ContextType vs Consumer

Class.ContextType은 React v16.6.0에 뒤 늦게 도입되었다.

기존의 Context.Consumer 메소드는 클래스 컴포넌트에서 적용하기에 굉장히 불편하고 어려워서 개발자들의 피드백을 받아 추가했다.

<br>

좀더 편리한 API를 제공해서 클래스 컴포넌트에서 쉽게 context 값을 사용할 수 있게되었다.

<br>

참고

- [https://reactjs.org/blog/2018/10/23/react-v-16-6.html#static-contexttype](https://reactjs.org/blog/2018/10/23/react-v-16-6.html#static-contexttype)
- [https://ko.reactjs.org/docs/context.html](https://ko.reactjs.org/docs/context.html)
- [https://stackoverflow.com/questions/54283509/react-context-context-consumer-vs-class-contexttype](https://stackoverflow.com/questions/54283509/react-context-context-consumer-vs-class-contexttype)

# Components and Props

<br>

개념적으로 컴포넌트는 JavaScript 함수와 유사하다. 

“props”라고 하는 임의의 입력을 받은 후, 화면에 어떻게 표시되는지를 기술하는 React 엘리먼트를 반환한다.

<br>

## 함수 컴포넌트와 클래스 컴포넌트

<br>

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

<br>

이 함수는 데이터를 가진 하나의 “props” (props는 속성을 나타내는 데이터이다.) 객체 인자를 받은 후 React 엘리먼트를 반환하므로 유효한 React 컴포넌트이다. 

이러한 컴포넌트는 JavaScript 함수이기 때문에 말 그대로 **“함수 컴포넌트”** 라고 호칭한다.

<br>

또한 **ES6 class** 를 사용하여 컴포넌트를 정의할 수 있습니다.

```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```
<br>


## 컴포넌트 렌더링

<br>

이전까지는 React 엘리먼트를 DOM 태그로 나타냈다.

```jsx
const element = <div />;
```

<br>

React 엘리먼트는 사용자 정의 컴포넌트로도 나타낼 수 있다.

```jsx
const element = <Welcome name="Sara" />;
```

<br>

React가 사용자 정의 컴포넌트로 작성한 엘리먼트를 발견하면 JSX 어트리뷰트와 자식을 해당 컴포넌트에 단일 객체로 전달한다. 

이 객체를 **“props**” 라고 한다.

<br>

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

<br>

### **위 예시 동작원리)**

1. `<Welcome name="Sara" />` 엘리먼트로 `ReactDOM.render()`를 호출한다.
2. React는 `{name: 'Sara'}`를 props로 하여 `Welcome` 컴포넌트를 호출한다.
3. `Welcome` 컴포넌트는 결과적으로 `<h1>Hello, Sara</h1>` 엘리먼트를 반환한다.
4. React DOM은 `<h1>Hello, Sara</h1>` 엘리먼트와 일치하도록 DOM을 효율적으로 업데이트한다.

<br>

컴포넌트의 이름은 항상 대문자로 시작한다.

React는 소문자로 시작하는 컴포넌트를 DOM 태그로 처리한다. 

예를 들어 `<div />`는 HTML `div` 태그를 나타내지만, `<Welcome />`은 컴포넌트를 나타내며 범위 안에 `Welcome`이 있어야 합니다.

<br>

## 컴포넌트 합성

<br>

컴포넌트는 자신의 출력에 다른 컴포넌트를 참조할 수 있다.

<br>

예를 들어 Welcome을 여러 번 렌더링하는 App 컴포넌트를 만들 수 있다.

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

<br>

일반적으로 새 React 앱은 최상위에 단일 App 컴포넌트를 가지고 있다. 

하지만 기존 앱에 React를 통합하는 경우에는 Button과 같은 작은 컴포넌트부터 시작해서 뷰 계층의 상단으로 올라가면서 점진적으로 작업해야 할 수 있다.

<br>

## 컴포넌트 추출

```jsx
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}

const comment = {
  date: new Date(),
  text: 'I hope you enjoy learning React!',
  author: {
    name: 'Hello Kitty',
    avatarUrl: 'https://placekitten.com/g/64/64',
  },
};
ReactDOM.render(
  <Comment
    date={comment.date}
    text={comment.text}
    author={comment.author}
  />,
  document.getElementById('root')
);
```

<br>

Comment 컴포넌트는 comment의 객체를 참조하여 props로 받아 함수 컴포넌트를 구성한다.

<br>

이 컴포넌트는 구성요소들이 모두 중첩 구조로 이루어져 있어서 변경하기 어려울 수 있으며, 각 구성요소를 개별적으로 재사용하기도 힘들다. 

<br>

```jsx
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}
```

<br>

`Avatar` 는 자신이 `Comment` 내에서 렌더링 된다는 것을 알 필요가 없다.

따라서 props의 이름을 `author`에서 더욱 일반화된 `user`로 변경한다.

<br>

props의 이름은 사용될 context가 아닌 컴포넌트 자체의 관점에서 짓는 것을 권장한다.

```jsx
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <Avatar user={props.author} />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

<br>

```jsx
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}
```

<br>

```jsx
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

<br>

재사용 가능한 컴포넌트를 만들어 놓는 것은 더 큰 앱에서 작업할 때 두각을 나타낸다. 

UI 일부가 여러 번 사용되거나 UI 일부가 자체적으로 복잡한 (Comment) 경우에는 별도의 컴포넌트로 만드는 게 좋다.

<br>

## props는 읽기 전용이다.

<br>

함수 컴포넌트나 클래스 컴포넌트 모두 컴포넌트의 자체 props를 수정해서는 안 된다.

```jsx
function sum(a, b) {
  return a + b;
}
```

<br>

이런 함수들은 순수 함수라고 호칭한다. 

입력값을 바꾸려 하지 않고 항상 동일한 입력값에 대해 동일한 결과를 반환하기 때문이다.

<br>

다음 함수는 자신의 입력값을 변경하기 때문에 비순수 함수이다.

```jsx
function withdraw(account, amount) {
  account.total -= amount;
}
```

<br>

**React는 매우 유연하지만 한 가지 엄격한 규칙이 있다.**

<br>

**모든 React 컴포넌트는 자신의 props를 다룰 때 반드시 순수 함수처럼 동작해야 한다.**

<br>

출처: 리액트 공식문서[[https://ko.reactjs.org/docs/components-and-props.html](https://ko.reactjs.org/docs/components-and-props.html)]
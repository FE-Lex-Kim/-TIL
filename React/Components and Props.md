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

또한 **ES6 class** 를 사용하여 **클래스 컴포넌트** 를 정의할 수 있습니다.

```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

<br>

### **클래스형 컴포넌트 vs 함수형 컴포넌트**

<br>

**함수형 컴포넌트 장점**

1 . 컴포넌트 선언 편리

2 . 메모리 사용 적음

<br>

**함수형 컴포넌트 단점**

1 . state 사용 불가능

2 . 라이플사이클 API 사용 불가능(Hooks 기능 추가로 해결)

<br>

**클래스형 컴포넌트 장점**

1 . state 기능

2 .  라이프사이클 기능 사용

3 . 임의 메서드 정의 가능

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

# Props

<br>

props는 properties를 줄인 표현이다.

컴포넌트 속성을 설정할때 사용하는 요소이다.

<br>

props의 값은 사용하는 컴포넌트의 부모 컴포넌트에서 속성으로 설정 할 수 있다.

<br>

## 1. props는 읽기 전용이다.

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

## 2. props 기본값 설정

<br>

<컴포넌트이름>.defaultProps 에서 객체로서 기본값을 설정해주면된다.

```jsx
import React from 'react';

const MyComponent = (props) => <div>안녕 내이름은 {props.name} 이야!</div>;

MyComponent.defaultProps = {
  name: 'Jaems',
};

export default MyComponent;
```

<br>

## 3. children

<br>

```jsx
import React from 'react';
import MyComponent from './MyComponent';

const App = () => <MyComponent name="Alex">리액트</MyComponent>;
export default App;
```

<br>

```jsx
import React from 'react';

const MyComponent = (props) => (
  <div>
    안녕 내이름은 {props.name} 이야! children 값은 {props.children} 이야!
  </div>
);

MyComponent.defaultProps = {
  name: 'Jaems',
};

export default MyComponent;
```

<br>

위의 APP 코드에서 MyComponent태그 사이의 텍스트 값을 가져올 수 있다.

<br>

디스트럭쳐링 할당으로 코드를 만들면 간편한 코드를  만들 수 있다.

```jsx
import React from 'react';

const MyComponent = ({ name, children }) => (
  <div>
    안녕 내이름은 {name} 이야! children 값은 {children} 이야!
  </div>
);

MyComponent.defaultProps = {
  name: 'Jaems',
};

export default MyComponent;
```

<br>

## 4. propTypes

<br>

props의 타입을 지정해 줄 수 있다.

```bash
npm i -S prop-type
```

<br>

import를 해주어야한다.

```jsx
import propTypes from 'prop-types';
```

<br>

다음과 같이 propTypes 객체로 지정해 줄 수 있다.

```jsx
import React from 'react';
import propTypes from 'prop-types';

const MyComponent = ({ name, children }) => (
  <div>
    안녕 내이름은 {name} 이야! children 값은 {children} 이야! 내 생일인 달은{' '}
    {month}월 이야!
  </div>
);

MyComponent.defaultProps = {
  name: 'Jaems',
};

MyComponent.propTypes = {
  name: propTypes.string,
};

export default MyComponent;
```

<br>

타입이 동일하지 않으면 브라우저의 콘솔창에서 오류가 나온다.

<br>

propTypes를 지정하지 않았을때 경고 메세지를 보낼수있다.

isRequired을 붙여주면된다.

```jsx
MyComponent.propTypes = {
  name: propTypes.string,
  month: propTypes.number.isRequired,
};
```

<br>

**지정할 수 있는 타입종류**

1. array : 배열
2. arrayOf: 특정타입만 가지고있는 배열
3. bool: 불린값
4. number: 숫자
5. func: 함수
6. string: 문자열
7. ...등등

<br>

[더자세한 정보](https://ko.reactjs.org/docs/typechecking-with-proptypes.html)

<br>

## 5. 클래스형 컴포넌트의 props

<br>

render() 함수안에서 this.props로 참조할수있다.

defaultProps와 propTypes도 똑같은 방식이다.

<br>

defaultProps와 propTypes을 클래스 바깥에 선언 해주어야한다.

```jsx
import React, { Component } from 'react';
import PropTypes from 'prop-types';

class MyComponent extends Component {
  render() {
    // eslint-disable-next-line react/prop-types
    const { name, children, month } = this.props;
    return (
      <div>
        안녕 내이름은 {name} 이야! children 값은 {children} 이야! 내 생일인 달은{' '}
        {month}월 이야!
      </div>
    );
  }
}

MyComponent.defaultProps = {
  name: '아무것도 아니다',
};

MyComponent.propTypes = {
  name: PropTypes.string,
  month: PropTypes.number.isRequired,
};

export default MyComponent;
```

참고: 리액트 공식문서[[https://ko.reactjs.org/docs/components-and-props.html](https://ko.reactjs.org/docs/components-and-props.html)]
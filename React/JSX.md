# JSX

<br>

## JSX란?

<br>

```jsx
const element = <h1>Hello, world!</h1>;
```

위의 문법은 문자열도, HTML도 아니다.

<br>

JSX라 하며 JavaScript를 확장한 문법이다. 

JSX라고 하면 템플릿 언어가 떠오를 수도 있지만, JavaScript의 모든 기능이 포함되어 있다.

JSX는 React **엘리먼트(element)** 를 생성한다.

<br>

## JSX에 표현식 포함하기

<br>

```html
<div id="root"></div>
```

```jsx
const nameObj = {
  oldName : 'james',
  newName : 'Alex'
};

const newPerson = ({oldName, newName}) => oldName + ' and ' +newName;

const element = (
  <h1>
    Hello,{newPerson(nameObj)}
  </h1>);

ReactDOM.render(element, document.getElementById('root'));
```

<br>

JSX의 중괄호 안에는 유효한 모든 JavaScript 표현식을 넣을 수 있다. 

예를 들어 `2 + 2, newPerson(nameObj)` 등은 모두 유효한 JavaScript 표현식이다.

<br>

가독성을 좋게 하기 위해 JSX를 여러 줄로 나눌때는 필수는 아니지만, 

자동 세미콜론 삽입을 피하고자 괄호로 묶는 것을 권장한다.

```jsx
const element = (
  <h1>
    Hello,{newPerson(nameObj)}
  </h1>);

const element = <h1>
    Hello,{newPerson(nameObj)}
  </h1>;

```

<br>

## JSX도 표현식이다.

<br>

표현식은 값으로 표현가능하다.

<br>

때문에 컴파일이 끝나면, 

JSX 표현식이 JavaScript 함수 호출이 되고 **JavaScript 객체로 인식된다.**

<br>

즉, JSX를 `if` 구문 및 `for loop` 안에 사용하고, 변수에 할당하고, 인자로서 받아들이고, 함수로부터 반환할 수 있다.

<br>

```jsx
const nameObj = {
  oldName : 'james',
  newName : 'Alex'
};

const newPerson = ({oldName, newName}) => oldName + ' and ' +newName;

// const element = (
//   <h1>
//     Hello,{newPerson(nameObj)}
//   </h1>);

function getGreeting(nameObj) {
  if (nameObj) {
    return <h1>Hello, {newPerson(nameObj)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}

ReactDOM.render(getGreeting(nameObj), document.getElementById('root'));
```

<br>

## JSX 속성 정의

<br>

속성에 따옴표를 이용해 문자열 리터럴을 정의할 수 있다.

```jsx
const element = <div tabIndex="0"></div>;
```

<br>

중괄호를 사용하여 어트리뷰트에 JavaScript 표현식을 삽입할 수도 있다.

```jsx
const element = <img src={user.avatarUrl}></img>;
```

<br>

어트리뷰트에 JavaScript 표현식을 삽입할 때 중괄호 주변에 따옴표를 입력하지 말아야 한다.

<br>

따옴표(문자열 값에 사용) 또는 중괄호(표현식에 사용) 중 하나만 사용하고, 

동일한 어트리뷰트에 두 가지를 동시에 사용하면 안된다.

<br>

JSX는 HTML보다는 JavaScript에 가깝기 때문에, React DOM은 HTML 어트리뷰트 이름 대신 `camelCase` 프로퍼티 명명 규칙을 사용한다.

예를 들어, JSX에서 `class`는 `[className](https://developer.mozilla.org/ko/docs/Web/API/Element/className)`가 되고 tabindex는 `[tabIndex](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/tabIndex)`가 된다.

<br>

속성안에는 스타일 객체가 들어간다.

```jsx
const nameObj = {
  oldName : 'james',
  newName : 'Alex'
};

const newPerson = ({oldName, newName}) => oldName + ' and ' +newName;

const element = (
  <h1>
    Hello,{newPerson(nameObj)}
  </h1>);

function getGreeting(nameObj) {
  if (nameObj) {
		// style="color : red"
    return <h1 style={{color : "red"}}>Hello, {newPerson(nameObj)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}

ReactDOM.render(getGreeting(nameObj), document.getElementById('root'));
```

<br>

## JSX로 자식 정의

태그가 비어있다면 XML처럼 /> 를 이용해 닫아주어야한다.

<br>

```jsx
const element = <img src={user.avatarUrl} />;
```

JSX 태그는 자식을 포함할 수 있다.

```jsx
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

<br>

```jsx
const newPerson = ({oldName, newName}) => oldName + ' and ' +newName;

const element = (
  <div>
    <h1>
      Hello,{newPerson(nameObj)}
    </h1>
    <p>I'm pretty okay!</p>
  </div>
);

ReactDOM.render(element, document.getElementById('root'));
```

<br>

## JSX는 객체를 표현한다.

Babel은 JSX를 React.createElement() 호출로 컴파일한다.

<br>

이 두가지 예제는 똑같다.

하지만 Babel이 JSX 컴파일하면 두번째 예시처럼 만든다.

```jsx
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

```jsx
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

<br>

`React.createElement()`는 버그가 없는 코드를 작성하는 데 도움이 되도록 몇 가지 검사를 수행하며, 기본적으로 다음과 같은 객체를 생성한다.

<br>

```jsx
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```

<br>

이러한 객체를 **“React 엘리먼트”** 라고 하며, 이를 화면에 표시하려는 항목에 대한 설명이라고 생각할 수 있다. 

React는 이러한 객체를 읽은 후 DOM을 구성하고 최신으로 유지하는 데 이러한 객체를 사용한다.
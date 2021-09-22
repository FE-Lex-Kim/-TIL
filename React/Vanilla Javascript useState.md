# Vanilla Javascript useState

<br>

React Hook에서 사용되는 useState를 Vanilla로 구현해보자.

useState 자체가 어떤 프로세스로 동작하는지 이해하게 된다면 useState를 사용하는 로직을 만들때 더 수월하고 이해도 높게 사용할 것이다.

<br>

useState를 vanilla js로 만들기전 기본적으로 클로저를 알아야한다.

클로저를 통해 useState가 동작하기 때문이다.

<br>

간단하게 클로저에 대해 공부해보자

### 클로저란

자바스크립트는 렉시컬 스코프이다.

**렉시컬 스코프는 함수가 호출 되었을때 상위스코프를 기억하는 것이 아니라 정의 되었을때 상위 스코프를 기억한다는 의미이다.**

<br>

따라서 외부함수안에 내부함수가 반환된다면 외부함수의 생명주기는 종료되고 내부함수는 외부 함수보다 더 긴 생명주기를 가진다.

<br>

이때 내부함수가 외부함수의 스코프에 있는 변수를 참조한다면, 외부함수의 변수는 그대로 보존되어 있다.

그 변수 이름을 **"자유 변수"** 라고 부른다.

<br>

이렇게 내부함수가 외부함수보다 생명주기가 더 길고(1), 자유변수를 가지고 있다면(2) 이러한 내부함수를 **클로저** 라고 부른다.

<br>

자, 이제 vanilla js로 useState를 만들 준비가 완료 되었다.

<br>

## useState 기초

기초적인 방법으로 만들어보자

```jsx
function useState(init) {
  let _val = init;
  const state = _val;
  const setState = (newVal) => {
    _val = newVal;
  };

  return [state, setState];
}

const [count, setCount] = useState(1);
console.log("count: ", count); // 1
setCount(2);
console.log("count: ", count); // 1
```

위와 같이 초기값은 할당되지만 `setCount` 를 한다고 해도 원하는 값이 나오지 않는다.

<br>

당연히 `useState` 내부에서 `state`에 할당하지 않았기 때문이다.

<br>

## count값 갱신

그렇다면 count값이 실시간으로 갱신되게 만들자.

```jsx
function useState(init) {
  let _val = init;
  const state = () => _val;
  const setState = (newVal) => {
    _val = newVal;
  };

  return [state, setState];
}

const [count, setCount] = useState(1);
console.log("count: ", count()); // 1
setCount(2);
console.log("count: ", count()); // 2
```

`useState` 내부에서 state를 함수로 만들어서 호출시켜주면 변경된`_val` 값이 반환되게 만들어서 정상적으로 보이게되었다.

<br>

## React 모듈생성

최대한 실제와 비슷하게 React 모듈안에 넣어서 만들어보자.

```jsx
const React = (() => {
  function useState(init) {
    let _val = init;
    const state = () => _val;
    const setState = (newVal) => {
      _val = newVal;
    };

    return [state, setState];
  }

  return { useState };
})();

const [count, setCount] = React.useState(1);
console.log("count: ", count()); // 1
setCount(2);
console.log("count: ", count()); // 2
```

이제 React.useState로 호출가능하다.

<br>

## 예제를 위한 component 생성

**함수형 컴포넌트**에서 **click 이벤트**가 발생했을때 `setCount(count + 1)` 이 실행되어서 값이 1씩 증가한다고 해보자.

```jsx
const React = (() => {
  let _val;
  function useState(init) {
    // 만약 _val값이 없으면 init을 넣어줌
    // 가장 초기화 할때는 _val 값이 없으니 init을 넣어준다.
    const state = _val || init;
    const setState = (newVal) => {
      _val = newVal;
    };

    return [state, setState];
  }

  function render(Component) {
    const C = Component();

    // 단순하게 state 값이 디버그 콘솔에 보이게 한다.
    C.render();

    return C;
  }

  return { useState, render };
})();

function Component() {
  const [count, setCount] = React.useState(1);
  return {
    render: () => console.log(count),
    click: () => setCount(count + 1),
  };
}

var App = React.render(Component);
App.click();
var App = React.render(Component);
```

<br>

`Component` 는 반환하는 값이 `render`와 `click` 메서드를 담고 있는 객체이다.

<br>

반환되어진 객체의 메서드인

render는 현재 `count`값을 알려준다.

click은 `setCount`가 발생해 `count + 1` 해준다.

<br>

React 함수 내부에 `render` 함수를 만들어서 인수에 `Component`를 받는다.

<br>

**함수형 컴포넌트**이다 보니 `Component`가 `click` 또는 `type` 이벤트가 발생하면 다시 호출된다.

<br>

따라서 Componet가 다시 호출되므로 내부의 **useState가 다시 호출되고 초기값이 다시 반환**되면 안된다.

이전에 저장된 값이 있다면(setState로 추가된 값) 가장 마지막에 들어간 값을 사용해야한다.

<br>

`React.render(Component)`를 실행하면 현재 해당 `Component.render` 를 호출해 state 값을 디버그 콘솔에 보여준다.(`React.render`가 HOC 처럼 컴포넌트를 인수로 받아 내부에서 작성된 코드를 사용하다 보니 어떻게 로직이 흘러가는지 모른다는 점에 가독성과 코드 추적이 어려워서 개인적으로 너무 싫다.)

<br>

`React.render(Component)`가 반환하는 값은 인수로 들어간 `Component`가 된다.

<br>

이제 `useState`를 사용해서 나온 `state`값과 `setState`값을 편하게 그리고 실제 컴포넌트에서 보여지듯이 구현했다.

<br>

## useState를 여러번 호출

Component 내부에서 state를 여러개 관리하기 위해서 여러번 호출한다면 어떻게 될까.

<br>

결과 부터 말하면 정상동작하지 않는다.

```jsx
const React = (() => {
  let _val;
  function useState(init) {
    // 만약 _val값이 없으면 init을 넣어줌
    // 가장 초기화 할때는 _val 값이 없으니 init을 넣어준다.
    // Component를 두 번째 호출할떄는 _val에 값이 있으므로 init 값이 들어간다.
    const state = _val || init;
    const setState = (newVal) => {
      _val = newVal;
    };

    return [state, setState];
  }

  function render(Component) {
    const C = Component();

    // 단순하게 state 값이 디버그 콘솔에 보이게 한다.
    C.render();

    return C;
  }

  return { useState, render };
})();

function Component() {
  const [count, setCount] = React.useState(1);
  const [text, setText] = React.useState("Apple");
  return {
    render: () => console.log({ count, text }),
    click: () => setCount(count + 1),
    type: (nextText) => setText(nextText),
  };
}

var App = React.render(Component); // {count : 1, text: 'Apple'}
App.click();
var App = React.render(Component); // {count : 2, text: 2}
App.type("Orange");
var App = React.render(Component); // {count : Orange, text: 'Orange'}
```

<br>

어째서 setState를 하면 state값이 의도와 다르게 나올까?

차근차근 프로세스를 생각해보자.

<br>

### 예제코드 동작 프로세스

1. Component가 `React.render`에 의해 호출된다.
2. Component 내부의 useState가 호출되어서 초기값인 1이 count에 들어간다.
3. useState가 두번째로 호출되어 초기값인 'Apple'이 text에 들어간다.
4. `App.click()` 호출되어지고 `setCount(count + 1)` 이 호출된다.
   1. setCount 로직을 보면 React 스코프의 `_val` 변수에 `count + 1` 값을 할당한다.
5. Component가 `React.render`에 의해 호출된다.
   1. 이때 내부 동작을 보면 첫번째 useState가 호출되고 초기값 1이 들어가 있다.
   2. useState의 내부 로직에 `const state = _val || init;` 의미는 `_val`에 값이 없다면 `init`인 초기값이 들어간다는 로직이다.
   3. 하지만 \_val 값은 이미 들어가 있다. 언제 들어가 있었을까.
   4. 4.a 동작에 의해 이미 `_val` 값에는 `count + 1` 즉, 2값이 이미 할당 되어있다.
   5. 따라서 state값은 2가 된다.
   6. 두번째 `useState` 도 마찬가지로 인수에 `"Orange"` 가 들어 가 있지만 마찬가지 로직에 의해 text는 2가 들어간다.
6. 두번째 `App.click()` 도 위와 같은 로직에 의해 동작하게 된다.

<br>

## 배열에 state 저장

<br>

`_var` 에 저장했던게 문제 였기 때문에 이전에 정상동작 하지 않았다.

그래서 state들을 배열안에 저장한다면 어떨까?

```jsx
const React = (() => {
  let hooks = []; // <- 이부분 수정됨
  let idx = 0; // <- 이부분 수정됨
  function useState(init) {
    // 만약 _val값이 없으면 init을 넣어줌
    // 가장 초기화 할때는 _val 값이 없으니 init을 넣어준다.
    let state = hooks[idx] || init;
    const setState = (newVal) => {
      hooks[idx] = newVal; // <- 이부분 수정됨
    };
    idx++; // <- 이부분 수정됨

    return [state, setState];
  }

  function render(Component) {
    const C = Component();

    // 단순하게 state 값이 디버그 콘솔에 보이게 한다.
    C.render();

    return C;
  }

  return { useState, render };
})();

function Component() {
  const [count, setCount] = React.useState(1);
  const [text, setText] = React.useState("Apple");
  return {
    render: () => console.log({ count, text }),
    click: () => setCount(count + 1),
    type: (nextText) => setText(nextText),
  };
}

var App = React.render(Component); // {count : 1, text: 'Apple'}
App.click();
var App = React.render(Component); // {count : 2, text: 'Apple'}
App.type("Orange");
var App = React.render(Component); // {count : 'Orange', text: 'Apple'}
```

<br>

위의 로직을 하나하나 로직을 차례대로 살펴보면서 잘못된 값을 가지는지 살펴보자.

<br>

### 먼저 알고 있으면 로직 이해할때 쉬운 사항을 먼저 짚어 보자

1. `useState`가 호출되면 `idx` 값이 `1`씩 증가한다.
2. `idx`의 목적은 `setState`를 호출하면 `idx` 인덱스 위치에 새로운 값을 할당한다.
3. `useState`가 호출되면 가장 마지막에 `setState`한 값이 있으면 그 값을 사용한다.`(let state = hooks[idx] || init;)`
4. 이후 `idx` 값을 `1` 증가 시켜서 `hooks`의 새로운 인덱스 위치에 넣을 준비를 한다.

<br>

### 예제코드 동작 프로세스

1. `Componet` 최초 호출
   1. 내부의 `useState(1)` 호출
   2. `state` 값은 `hooks[idx]` 값이 `undefiend` 이므로 `init` 값이 들어가 `state` 는 `1`이 된다.
   3. `idx++`로 기존의 `idx===0`값에서 `1`로 증가한다.
   4. 두 번째 `useState('Apple')` 가 호출된다.
   5. `hooks[idx](idx === 1)` 값이 `undefiend` 이므로 init 값인 `'Apple'` 이 들어간다.
   6. 이후 `idx` 는 `2`로 증가한다.
2. `App.click` 호출
   1. `setCount(count + 1)` 이 호출된다.
   2. 현재 `idx===2` 이므로 `hooks[2] = 2` 값이 된다.
3. `Componet` 두 번째 호출
   1. click 이벤트 핸들러로 인해 함수형 컴포넌트인 `Component`는 두 번째 호출이 된다.
   2. `Component`의 로직이 다시 실행되서 `useState(1)` 이 호출 된다.
   3. `useState`호출 로직 내부에 `let state = hooks[idx] || init;` 가 있는데 현재 `idx===2` 이므로 `hooks[2]` 값은 `2`이다. 따라서 `state`값은 `2`가 된다.
   4. 이후 `idx++` 이므로 `idx` 값은 `3`으로 증가한다.
   5. 두번째 `useState('Apple')` 호출 된다.
   6. 현재 `idx===3` 이므로 `hooks[3]` 값은 `undefiend` 이다. 따라서 `state` 값은 `init`인 `'Apple'`가 들어간다.
   7. 이후 `idx++` 로 인해 `idx` 값은 `4`로 증가한다.
   8. 현재까지 `state = { count : 2, text : Apple}`
4. `App.type` 호출
   1. `setText('Orange')`가 호출된다.
   2. 현재 `idx===4` 이므로 `hooks[4]`는 `'Orange'`가 들어가게 된다.
5. `Componet` 세 번째 호출
   1. `type` 이벤트 핸들러로 인해 함수형 컴포넌트인 `Component`는 세 번째 호출이 된다.
   2. `Component`의 로직이 다시 실행되서 `useState(1)` 이 호출 된다.
   3. `useState`호출 로직 내부에 `let state = hooks[idx] || init;` 가 있는데 현재 `idx===4` 이므로 `hooks[4]` 값은 `Orange`이다. 따라서 `state`값은 `Orange`가 된다.
   4. 이후 `idx++` 이므로 `idx` 값은 `5`으로 증가한다.
   5. 두번째 `useState('Apple')` 호출 된다.
   6. 현재 `idx===5` 이므로 `hooks[5]` 값은 `undefiend` 이다. 따라서 `state` 값은 `init`인 `'Apple'`가 들어간다.
   7. 이후 `idx++` 로 인해 `idx` 값은 `6`로 증가한다.
   8. 현재까지 `state = { count : 'Orange', text : Apple}`

<br>

지금까지 로직을 하나하나 살펴보면서 알아 챘을 수 있다.

`useState`가 여러개일때, 컴포넌트의 첫번째로 호출되는 `useState`종류와 호출된 `setState`의 종류가 일치하지 않으면 값이 `setState` 값으로 덮어진다는 점이다.

<br>

그렇다고 항상 `Component`의 첫번째 `useState(예제에서는 count state)`인 `setState`만 호출할 수도 없다.

다른 `useState`의 `setState`도 호출하는 이벤트도 있기 때문이다.(당연한 말이지만 다시 짚음)

<br>

## 각 useState 마다 state를 저장할 index를 지정

매번 hooks 배열의 다른 index에 state를 저장하면서 마지막 index에 위치한 state값을 참조한 방법은 단점이 컸었다.

<br>

따라서 각각 useState마다 배열에 저장할 state의 고유의 index를 지정하면 된다.

```jsx
const React = (() => {
  let hooks = [];
  let idx = 0;
  function useState(init) {
    // 만약 _val값이 없으면 init을 넣어줌
    // 최초 초기화 할때는 _val 값이 없으니 init을 넣어준다.
    let state = hooks[idx] || init;
    let _idx = idx;
    const setState = (newVal) => {
      hooks[_idx] = newVal;
    };
    idx++;

    return [state, setState];
  }

  function render(Component) {
    // render가 호출되면 idx값을 0으로 초기화함
    idx = 0;
    const C = Component();

    // 단순하게 state 값이 디버그 콘솔에 보이게 한다.
    C.render();

    return C;
  }

  return { useState, render };
})();

function Component() {
  const [count, setCount] = React.useState(1);
  const [text, setText] = React.useState("Apple");
  return {
    render: () => console.log({ count, text }),
    click: () => setCount(count + 1),
    type: (nextText) => setText(nextText),
  };
}

var App = React.render(Component);
App.click();
var App = React.render(Component);
App.type("Orange");
var App = React.render(Component);
```

<br>

### 예제코드 동작 프로세스

1. 최초 Component 호출
   1. `useState(1)`을 호출한다.
   2. `idx`가 `0`이고 `hooks[idx]` 값은 비어있으므로 `state`는 `init`값인 `1`이된다.
   3. `_idx`변수에 현재 `idx`를 저장한다.
   4. `idx`의 값을 `1`증가 시킨다.
   5. `useState('Apple')` 을 호출한다.
   6. `idx`가 `1`이고 `hooks[idx]` 값은 비어있으므로 `state`는 `init` 값인 `'Appel'`이 된다.
   7. `_idx`변수에 현재 `idx`를 저장한다.
2. App.click 호출
   1. `setCount(count + 1)`이 호출된다.
   2. `setCount`함수가 정의 되었을 시점에 상위 스코프의 `idx`의 값은 `0`이 였다. 따라서 `_idx`도 `0`이 였고 `setCount`가 클로저로 반환되고 이때 `_idx`값은 자유변수로 해당 시점에 호출된 `useState`의 `_idx` 값을 가지고 있다. 따라서 `hooks[0]` 값에 `2`를 할당한다.
3. 두 번째 Component 호출
   1. `React.render` 가 호출되면 `idx` 값을 `0`으로 초기화 한다. 그이유는 `useState`의 `state`가 저장되는 위치를 순서대로 하기 위해서 이다.(useState의 순서라고 생각하면 쉽다.)
   2. `useState(1)`을 호출한다.
   3. `idx`가 `0`이고 `hooks[0]` 값은 2였으므로 `state`는 `2`가된다.
   4. `_idx`변수에 현재 `idx`를 저장한다.
   5. `idx`의 값을 `1`증가 시킨다.
   6. `useState('Apple')` 을 호출한다.
   7. `idx`가 `1`이고 `hooks[1]` 값은 `'Apple'` 이였으므로 `state`는`'Apple'`이 된다.
   8. `_idx`변수에 현재 `idx`를 저장한다.
4. App.type 호출
   1. `setText('Orange')`가 호출된다.
   2. `setText`함수가 정의 되었을 시점에 상위 스코프의 `idx`의 값은 `1`이 였다. 따라서 `_idx`도 `1`이 였고 `setText`가 클로저로 반환되고 이때 `_idx`값은 자유변수로 해당 시점에 호출된 `useState`의 `_idx` 값을 가지고 있다. 따라서 `hooks[1]` 값에 `'Orange'`를 할당한다.
5. 세 번째 Component 호출
   1. `React.render` 가 호출되면 `idx` 값을 `0`으로 초기화 한다. 그이유는 위의 두 번째 `Component`가 호출됬을 때랑 같다.
   2. `useState(1)`을 호출한다.
   3. `idx`가 `0`이고 `hooks[0]` 값은 2였으므로 `state`는 `2`가된다.
   4. `_idx`변수에 현재 `idx`를 저장한다.
   5. `idx`의 값을 `1`증가 시킨다.
   6. `useState('Apple')` 을 호출한다.
   7. `idx`가 `1`이고 `hooks[1]` 값은 `'Orange'` 이였으므로 `state`는`'Orange'`이 된다.
   8. `_idx`변수에 현재 `idx`를 저장한다.

<br>

현재 useState가 호출된 순서에 따라서 index 위치가 정해지고 있다.

따라서 useState가 조건문 또는 반복문에 따라서 호출되고 안되고를 결정하면 state가 저장된 배열(React 함수의 hooks 변수)의 index의 값을 불러오거나 저장할 수 없을 것이다.

실제로 [React Hook 규칙 공식 홈페이지](https://ko.reactjs.org/docs/hooks-rules.html) 를 보면 다음과 같은 내용을 가지고 있다.

![vanill javascript useState](../Images/Vanilla%20Javascript%20useState/Vanilla%20Javascript%20useState-1.png)

<br>

### 정리

- 각각 useState 별로 state를 저장할 고유의 index 위치를 만든다.
- 최초의 index는 Component에서 정의된 useState에서 시작해서 다른 useState가 호출될 때마다 증가한다.

<br>

useState를 만들기 위해서 이런저런 과정을 거치고 이런 로직일땐 안되고 저런 로직일땐 안되고를 만들어보고 알게되었다.

마지막에 완성형으로 만든 useState를 더 잘 이해할 수 있게 되었다.

<br>

실제 useState는 React에 따르면 최적화를 위해서 가장 마지막에 비동기로 실행된다고 한다.

위의 예제와 다른 코드로 동작하지만 간단하게 useState를 만들어 보았다.

<br>

참고

- [https://www.youtube.com/watch?v=KJP1E-Y-xyo](https://www.youtube.com/watch?v=KJP1E-Y-xyo)
- [https://hewonjeong.github.io/deep-dive-how-do-react-hooks-really-work-ko/](https://hewonjeong.github.io/deep-dive-how-do-react-hooks-really-work-ko/)

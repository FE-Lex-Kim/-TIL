# Hook 사용하는 이유

<br>

React는 Hook을 사용하기 이전에 지금까지 컴포넌트를 만들면서 여러가지 많은 문제들이 있었다.

이러한 문제들을 해결하기위해서 React는 컴포넌트를 만드는 새로운 개념인 Hook을 도입했다.

<br>

그렇다면 Hook을 사용하기 이전에 어떠한 문제가 있었는지 살펴보자.

<br>

## 1. 컴포넌트 사이에서 상태 로직들의 재사용이 힘들다.

컴포넌트 사이에서 중복된 코드들이 있을때 render props 또는 HOC를 사용해서 코드의 중복성을 막고 재사용할 수 있게 해왔었다.

<br>

하지만 render props와 HOC를 사용하는 패턴은 컴포넌트를 재구성해야만 했다.

이렇게 컴포넌트를 재구성을 하게되면 컴포넌트의 코드 추적이 굉장히 힘들어졌다.

<br>

예를들어 Provider, Consumer, HOC, render Props 등등을 사용하게되면 추상화에 따른 **'래퍼 지옥'** 가능성이 굉장히 높아졌었다.

따라서 계층적 구조가 계속 깊어지고 유지보수 및 가독성이 매우 떨어지게 되었다. \*\*\*\*

<br>

이러한 점을 Hook으로 개선했다.

<br>

또한 Hook은 계층과 상관 없고 로직 재사용, 독립적 테스트이 가능했다.

Hook은 상태관리하는 `useState`와 `useEffect` 가 독립적으로 상태를 가지고 있다.

다른 컴포넌트에서 해당 재사용 함수를 가져와 호출하여도 현재 컴포넌트의 상태 로직이 영향을 받지 않는다.

따라서 컴포넌트를 class 처럼 래핑하지 않아 계층화 하지않는다.

<br>

정리

- class에서 코드재사용은 컴포넌트 계층화가 일어난다.
- Hook은 단순 함수를 호출하여 재사용을 하기에 계층화의 부작용을 해결했다.

<br>

## 2. lifecycle에 의한 복잡한 코드

`componentDidMount, componentDidUpdate` 에서 AJAX를 통한 데이터를 불러오거나, 이벤트 핸들러를 등록하는 작업을 이곳에서 했다.

<br>

`componentDidMount` 에서 데이터 통신 및 이벤트 핸들러를 하는 여러가지 로직들이 많이 있다면, 해당 로직들을 잘게 쪼개어 분리할 수 없었다.

예를들어 `componentDidMount`에서 만약 "데이터 통신1" "데이터 통신2" "이벤트 핸들러 등록1", "이벤트 핸들러 등록2" 가 있다고 하면 각각 데이터 통신 1과 2와 이벤트 핸들러등록 1과2의 로직들을 분리하지 못하고 항상 뭉쳐있어서 코드의 가독성과 유지보수에서 불편함이 많았다.

<br>

또한 `componentDidMount` 에서 등록한 이벤트 핸들러가 `componentWillUnmount` 에서 이벤트핸들러를 제거하려면 그 사이의 제거와 등록하는 로직이 코드상 멀리 떨어져있어서 가독성이 매우 떨어진다.

예를 들면

```jsx
class example extends PureComponent {
  componentDidMount() {
			... 데이터 통신1
			... 데이터 통신2
			... 이벤트 핸들러 등록1
			... 이벤트 핸들러 등록2
	}

  componentDidUpdate() {
			... 기타 등등 로직
	}

  componentWillUnmount() {
			// componentDidMount와 떨어져 있다보니 이벤트 핸들러 등록1과 제거1이 멀어져 유지보수와 가독성이 떨어진다.
			... 이벤트 핸들러 제거1
			... 이벤트 핸들러 제거2
	}

  render() {
    return <div></div>;
  }
}
```

componentDidMount와 떨어져 있다보니 **이벤트 핸들러 등록1**과 **제거1**이 멀어져 유지보수와 가독성이 떨어진다.

<br>

다시 정리하자면 두가지 단점

1. lifecycle 내부에서 여러가지 기능들의 로직 뭉쳐있어 가독성 하락
2. 각각 lifecycle의 메서드가 분리되어 있어 가독성 하락

<br>

이 두가지 단점을 Hook을 통해 해결했다.

`ComponentDidMount` 에서 이전 예제에서 "데이터 통신1" "데이터 통신2" "이벤트 핸들러 등록1", "이벤트 핸들러 등록2"의 로직들이 있었다.

이것을 Hook을 통해서는 각각 useEffect를 4번 나누어서 호출하면 된다.

```jsx
import React from "react";

function example(props) {
  useEffect(); // 데이터 통신1을 위한 호출
  useEffect(); // 데이터 통신2을 위한 호출
  useEffect(); // 이벤트 핸들러 등록1을 위한 호출
  useEffect(); // 이벤트 핸들러 등록2을 위한 호출
  return <div></div>;
}
```

<br>

이렇게 나누어서 호출할 수 있다어서 단점 1을 해결할 수 있다.

<br>

또한 `useEffect` 는 내부에서 이벤트를 등록을 했다면 return문의 함수에서 이벤트를 제거할 수 있는 패턴이다.

따라서 단점 2 또한 해결할 수 있다.

```jsx
function example(props) {
  useEffect(() => {
    // 이벤트 핸들러 1 등록
    return () => {
      // 이벤트 핸들러 1 제거
    };
  });
  return <div></div>;
}
```

이벤트 핸들러1 등록과 제거 로직이 가까이 있어서 가독성과 유지보수이 좋다.

<br>

## 3. javascript class의 부족함

React를 사용하기 위해서는 javascript this 개념을 완벽하게 파악해야 한다는 점이 있었다.

<br>

1. javascript의 this는 다른 언어와 다르게 동작한다는 점에서 어려움이 있다.
2. this는 코드의 혼란과 재사용, 로직 구성을 어렵가 만든다는 점이 있다.
3. 이벤트 핸들러 등록방법을 알아야 사용할 수 있다.

   - this를 사용한 이벤트 핸들러 등록방법은 코드의 로직이 길어진다는 단점이 있다.

     → 이벤트 핸들러가 호출되었을때 이벤트 핸들러 내부의 this 값이 브라우저가 되어 this의 값은 `undefiend` 가 된다.

     → 따라서 해당 이벤트 핸들러를 `bind(this)`를 해주어야 한다는 점이 있다.

     → 또 단점으로 이러한 this를 bind 해주는 방법이 여러가지가 있어서 능숙한 React 개발자들도 각자 다르게 bind 한다.

     ```jsx
     class Toggle extends React.Component {
       constructor(props) {
         super(props);
         this.state = { isToggleOn: true };

         // This binding is necessary to make `this` work in the callback
         this.handleClick = this.handleClick.bind(this);
       }

       handleClick() {
         this.setState((prevState) => ({
           isToggleOn: !prevState.isToggleOn,
         }));
       }

       render() {
         return (
           <button onClick={this.handleClick}>
             {this.state.isToggleOn ? "ON" : "OFF"}
           </button>
         );
       }
     }
     ```

<br>

참고

- [https://ko.reactjs.org/docs/hooks-intro.html](https://ko.reactjs.org/docs/hooks-intro.html)
- [https://ko.reactjs.org/docs/hooks-custom.html](https://ko.reactjs.org/docs/hooks-custom.html)
- [https://gseok.gitbooks.io/react/content/bd80-bd84-bd80-bd84-c9c0-c2dd-b4e4/react-c5d0-c11c-event-handling-d558-b294-bc95-c815-b9ac.html](https://gseok.gitbooks.io/react/content/bd80-bd84-bd80-bd84-c9c0-c2dd-b4e4/react-c5d0-c11c-event-handling-d558-b294-bc95-c815-b9ac.html)

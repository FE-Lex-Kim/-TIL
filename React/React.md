# React

<br>

## React는 무엇일까?

<br>

### 선언형

React는 상호작용이 많은 UI를 만들 때 생기는 어려움을 줄여준다.

React는 데이터가 변경됨에 따라 적절한 컴포넌트만 효율적으로 갱신하고 렌더링한다.

<br>

### 컴포넌트 기반

스스로 상태를 관리하는 캡슐화된 컴포넌트를 만든다.

컴포넌트 로직은 템플릿이 아닌 JavaScript로 작성된다. 

따라서 다양한 형식의 데이터를 앱 안에서 손쉽게 전달할 수 있고, DOM과는 별개로 상태를 관리할 수 있다.

<br>

React 컴포넌트는 render()라는 메서드를 구현하는데, 이것은 데이터를 입력받아 화면에 표시할 내용을 반환하는 역할을 한다. 

이 예제에서는 XML과 유사한 문법인 JSX(Javascript + XML)를 사용한다. 

컴포넌트로 전달된 데이터는 render() 안에서 **this.props**를 통해 접근할 수 있다.

<br>

```bash
class HelloMessage extends React.Component {
  render() {
    return (
      <div>
        Hello {this.props.name}
      </div>
    );
  }
}

ReactDOM.render(
  <HelloMessage name="Taylor" />,
  document.getElementById('hello-example')
);
```

<br>

JSX를 컴파일하려면 Babel을 이용해야한다.

### React는 기술 스택의 나머지 부분에는 관여하지 않는다.

기술 스택의 나머지 부분에는 관여하지 않기 때문에

기존 코드를 다시 작성하지 않고도 React의 새로운 기능을 이용해 개발할 수 있다.

React는 라이브러리로 굉장히 자유롭게 사용할 수 있다.

<br>

**그렇다면 XML은 무엇일까?**

eXtensible Markup Language의 약자

XML은 데이터 저장과 전송을 목적으로 만들어진 마크업 언어이다.

<br>

### Virtual DOM 사용

<br>

### Why? Virtual DOM 사용?

브라우저에서 서버로부터 HTML을 전달받으면

브라우저의 렌더 엔진이 이를 파싱하고 DOM 노드(Node) 로 이뤄진 트리를 만든다.

<br>

CSS로 만든 CSSOM과 DOM Tree가 합쳐

브라우저에 화면에 랜더링 되는 노드들만 나타내는 Render Tree를 생성한다.

<br>

Render Tree에서 DOM 트리에 새로운 노드가 추가되면 그과정에서 

랜더링이 다시 되는 Reflow, Repaint가 일어난다.

<br>

**이때**

100개의 노드를 하나하나 수정했을때 레이아웃 계산 및 리랜더링을 100번 다시한다.

<br>

때문에 100번이라는 리패인트,리패인팅 발생으로 인한 비용이 많으므로

<br>

만약 DOM에 다른점이 있다면 가상돔을 사용해 변화 되었던 노드들을 가상돔에서 바뀐점을 비교하며 교체를 하고

<br>

모든것이 바뀐뒤 실제 DOM에서 단한번 랜더링을 하게되 기존의 수많은 레이아웃 계산과 리랜더링을 단 한번으로 막아줄 수 있다.
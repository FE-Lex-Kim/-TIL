- [Typescript(9) - 타입스크립트 컴파일, React 타입스크립트](#typescript9---타입스크립트-컴파일-react-타입스크립트)
  - [타입스크립트 컴파일](#타입스크립트-컴파일)
  - [React 타입스크립트](#react-타입스크립트)
    - [Children Props 타입 지정](#children-props-타입-지정)
    - [리액트 요소 타입](#리액트-요소-타입)
      - [2. React.ReactNode](#2-reactreactnode)
      - [3. JSX.Element](#3-jsxelement)
    - [HTML 요소 타입](#html-요소-타입)

# Typescript(9) - 타입스크립트 컴파일, React 타입스크립트

## 타입스크립트 컴파일

컴파일러는 소스코드를 기계어 코드로 변환시켜 프로그램을 실행시킨다.

타입스크립트는 자바스크립트로 변환시키는 컴파일러다.

또다른 말로는 고수준 언어인 타입스크립트에서 고수준 언어인 자바스크립트로 변환되는 것이기 때문에 트랜스파일이라고 부르기도 한다.

<br>

타입스크트립 실행과정

1. .ts 확장자가 붙은 파일 찾음
2. 타입스크립트 소스코드를 타입스크립트 AST로 만듬
3. 타입 검사기가 AST를 확인해서 타입 확인(여기서 에러)
4. 탕입스크립트 AST를 자바스크립트 소스로 변환
5. 이후 자바스크립트 과정 실행됨

즉, 타입스크립트는 타입을 확인하는데만 쓰이며, 최종적으로 쓰이는 자바스크립트 코드에는 영향을 주지 않는다.

<br>

컴파일러 주요 역활은 크게 두가지가 있다.

1. 컴파일러는 코드에 타입 검사로 오류가 없는지 확인한다.
   - 컴파일 단계에서 타입을 확인함으로 런타임 전에 에러를 사전에 알려준다.
2. 타입검사후 구버전의 자바스크립트로 트랜스파일을 한다.
   - 바벨과 다른점은 바벨은 구버전의 자바스크립트로 컴파일해주기만 하고 타입 검사는 하지 않는다.

<br>

타입스크립트 컴파일러 동작과정

1. 스캐너 → .ts 파일을 토큰 단위로 분리.
2. 파서 → 토큰으로 AST 생성(트리구조)
3. 바인더 → 각 AST 노드에 대응하는 심볼 생성. 심볼은 선언된 타입의 노드 정보 포함하고 있음.
4. 체커 → AST를 탐색하며 심볼 정보를 활용해 타입 검사
5. 이미터 → 에러가 없으면 자바스크립트 소스 파일로 변환.

<br>

## React 타입스크립트

함수형 리액트 컴포넌트 타입은 React.FC가 있다.

```tsx
interface Props {
  name: string;
}

const Alex: React.FC<Props> = () => {};
```

React.FC를 사용할때 Children Props는 어떻게 타입 지정하는지 알아보자.

<br>

### Children Props 타입 지정

가장 보편적인 방법은 `ReactNode | undefined` 이다.

```tsx
interface Props {
  name: string;
  children?: ReactNode | undefined;
}
```

ReactNode를 하면 ReactElement 이외에도 boolean, number.. 등등이 있어서 구체적으로 타이핑하는 용도로는 좋지않다.

만약 구체적으로 타이핑을 하고싶다면, 직접 children 타이핑을 해주면된다.

예를 들면

```tsx
interface Props {
  children: "Alex" | "James" | "Andrew";
}
interface Props {
  children: string;
}
interface Props {
  children: ReactElement;
}
```

<br>

### 리액트 요소 타입

리액트 요소 타입으로 3가지가 있다.

1. React.ReactElement
2. React.ReactNode
3. JSX.Element

<br>

**#### 1. ReactElement**

`React.createElement` 메서드를 호출한 후 반환하는 타입이 ReactElement 타입이다.

`ReacatElement<P>` 같이 제네릭으로 사용하면, 해당 컴포넌트 props의 타입을 P 매개변수 타입에 지정해서 타이핑 할 수 있다.

예를들면

```tsx
const App = () => {
  return <Alex hobby={<Soccer player={"messi"} />}></Alex>;
};

interface HobbyProps {
  player: string;
}

interface Props {
  hobby: ReactElement<HobbyProps>;
}

const Alex = ({ hobby }: Props) => {
  return <li>{hobby}</li>;
};
```

Soccer(hobby)의 Props를 타이핑을 할 수 있게 됐다.

<br>

#### 2. React.ReactNode

ReactNode는 다음과 같이 구성되어있다.

```tsx
type ReactText = string | number;
type ReactChild = ReactElement | ReactText;
type ReactNode =
  | ReactChild
  | ReactFragment
  | ReactPortal
  | boolean
  | null
  | undefined;
```

즉 ReactNode는 render 함수가 반환 할 수 있는 모든 형태를 담고 있다.

<br>

```tsx
const MyComponent: React.FC<Props> = ({ children }) => {
  return <div>{children}</div>;
};
```

**ReactNode는 다양한 종류의 값을 나타내므로, 컴포넌트의 자식 요소로 다양한 형태의 값을 받을 때 사용된다.**

<br>

#### 3. JSX.Element

JSX 문법을 사용하여 생성된 요소를 나타내는 타입이다.

주로 렌더링할 컴포넌트의 요소를 표현하는 데 사용된다.

```tsx
const element = <MyComponent prop={value} />;
```

<br>

```tsx
interface Props {
  person: JSX.Element;
}

const Alex = ({ person }: Props) => {
  return <h1>hello</h1>;
};
export default Alex;

const App = () => {
  return <Alex person={<James />}></Alex>;
};
```

**JSX.Element 타입으로 선언함으로써 `person={<James />}` 과 같이 JSX 형태의 props만 전달할 수 있다.**

<br>

### HTML 요소 타입

HTML 태그의 속성 타입으로는 대표적으로 2가지가 있다.

1. DetailedHTMLProps
2. ComponentWithoutRef

<br>

1. DetailedHTMLProps

```tsx
type NativeButtonProps = React.DetailedHTMLProps<
  React.ButtonHTMLAttributes<HTMLButtonElement>,
  HTMLButtonElement
>;

type ButtonProps = {
  onClick?: NativeButtonProps["onClick"];
};
```

<br>

2. ComponentPropsWithoutRef

```tsx
type NativeButtonProps = React.ComponentPropsWithoutRef<"button">;

type ButtonProps = {
  onClick?: NativeButtonProps["onClick"];
};
```

<br>

이 두가지 공통점은 HTML 요소에 대해 타이핑 할 수 있다는점이다.

이 두가지 차이점은 함수형 컴포넌트에서 ref를 사용할때 나타난다.

- DetailedHTMLProps은 ref를 포함하지 않는 요소의 타입이다.
- ComponentPropsWithoutRef는 ref를 포함하지 않는 요소의 타입이다.

DetailedHTMLProps은 실제로 동작하지 않는 ref를 받도록 타입이 지정되어 예기치않은 에러가 발생할 수 있다.

따라서 HTML 속성의 타입을 할때는 ComponentPropsWithoutRef 타입을 사용해서 ref가 실제로 forwardRef와 함께 사용할때만 타입을 정의하는 것이 안전하다.

<br>

그렇다면 forwardRef의 타입은 어떻게 해야할까

```tsx
type Props = {
  name: string;
};

const Button = forwardRef<HTMLButtonElement, Props>((props, ref) => {
  return (
    <button ref={ref} {...props}>
      hello
    </button>
  );
});

const Alex = () => {
  const buttonRef = useRef();

  return <Button ref={buttonRef} />;
};
```

forwardRef의 제네릭 인자가 2가지가 있다.

`forwardRef<A, B>`

- A는 ref에 대한 타입
- B는 props에 대한 타입이다.

<br>

참고

- 우아한 타입스크립트 책을 정리한 글입니다.

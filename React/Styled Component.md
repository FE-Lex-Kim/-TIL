- [Styled Components](#styled-components)
  - [Styled Components를 사용하는 이유](#styled-components를-사용하는-이유)
  - [기본 사용방법](#기본-사용방법)
  - [Props](#props)
  - [Extending Style](#extending-style)
  - [Overriding .attrs](#overriding-attrs)
  - [Animation](#animation)
  - [Theme](#theme)
  - [Global Style](#global-style)

<br>

# Styled Components

**자바스크립트 파일안에 스타일을 선언할수있는 방식이 있다.**

이방식을 CSS-in-JS 라고 부른다.

**자바스크립트 파일하나에 스타일을 작성가능하여 css파일을 따로 만들지 않아도된다.**

Styled Components는 React Styling 라이브러리중에서 가장 많이 사용하고 있는 라이브러리이다.

![Styled Components](../Images/Styled%20Component/Styled%20Component-1.png)

<br>

## Styled Components를 사용하는 이유

- styled-component는 컴포넌트가 어떤 페이지에서 렌더되었는지 지속적으로 추적하고 그에 해당하는 스타일을 적용시켜주는 것이 자동화 되어있다. **따라서 코드를 분할하고 결합해서, 최소한의 코드만을 로드하게 할 수 있게 한다.**
- styled-component는 스타일의 class 이름이 중복되지 않게 class 이름을 유니크하게 생성한다. 따라서 **개발자는 중복, 겹침을 전혀 걱정할 일이 없어진다.**
- 컴포넌트에 영향을 주는 스타일링을 각각 다른 파일을 걸쳐서 찾지 않아도 된다. 그래서 **유지보수에서 굉장히 뛰어나다.**

<br>

설치

```bash
npm install --save styled-components
```

<br>

## 기본 사용방법

**`styled.`뒤에 태그이름을 넣어주면된다.**

**그리고 `styled.tag` 뒤에 tagged 템플릿 리터럴 문법을 써서 css를 정의하면된다.**

<br>

글로 이해하기 어렵지만 코드를 보면 굉장히 심플하다.

```jsx
import React from "react";
import styled from "styled-components";

const Wrapper = styled.section`
  padding: 4em;
  background: indigo;
`;

const Title = styled.h1`
  font-size: 1.5em;
  text-align: center;
  color: tomato;
`;

const StyledComponent = () => {
  return (
    <Wrapper>
      <Title>Hello world</Title>
    </Wrapper>
  );
};

export default StyledComponent;
```

<br>

![Styled Components](../Images/Styled%20Component/Styled%20Component-2.png)

Styled-component의 방식은 styles 과 component의 간격을 줄여준다.

**기존에는 css 파일을 만들어서 component와 Style의 거리가 멀었지만, styled-component는 단순히 일반적인 React Component를 만들어서 Style을 적용시켜주면된다.**

<br>

## Props

**Styled Components의 중요한 장점은 props의 값을 참조해서 css에 넣어줄수도있다.**

<br>

```jsx
const Wrapper = styled.section`
  padding: 4em;
  background: ${(props) => props.color}; ;
`;

...

const StyeldComponent = () => {
  return (
    <Wrapper color="yellow">
      <Title>Hello world</Title>
      <PinkTitle as="button">Hello world</PinkTitle>
    </Wrapper>
  );
};
```

<br>

## Extending Style

**자주 사용되는 Component가 있지만, 약간의 다른 Style을 적용하고 싶을 수 있다.**

Style을 그대로 상속받아서 쉽게 사용하는 방법이 있다.

<br>

**자주 사용하는 컴포넌트를 Styled() 생성자 함수로 감싸면 된다.**

```jsx
const Title = styled.h1`
  font-size: 1.5em;
  text-align: center;
  color: tomato;
`;

// 이미 Styled-componet로 사용된 컴포넌트를 상속받아 Style을 적용할 수 있다.
const PinkTitle = styled(Title)`
  color: pink;
`;

const StyeldComponent = () => {
  return (
    <Wrapper blue>
      <Title>Hello world</Title>
      <PinkTitle>Hello world</PinkTitle>
    </Wrapper>
  );
};
```

<br>

이렇게 Title 컴포넌트가 자주 사용되는 Style여서 상속받아 적용할 수 있지만,

**만약 다른 h1 태그가 아닌 다른 태그를 사용하려면 어떻게 할까?**

**Component prop에 as를 전달해 다른 Tag로 변경할 수 있다.**

```jsx
const Title = styled.h1`
  font-size: 1.5em;
  text-align: center;
  color: tomato;
`;

const PinkTitle = styled(Title)`
  display: block;
  margin: 0 auto;
  color: pink;
`;

const StyeldComponent = () => {
  return (
    <Wrapper blue>
      <Title>Hello world</Title>
      // as prop에 button을 전달해 h1태그에서 button 태그로 변경되었다.
      <PinkTitle as="button">Hello world</PinkTitle>
    </Wrapper>
  );
};
```

<br>

## Overriding .attrs

**`.attrs`를 사용하면, 기존에 있던 Component의 Attribute를 덮어씌어서 atttr을 설정 할 수 있다.**

<br>

`styled.tag.attrs(객체 || 함수)` 로 사용한다.

메서드의 인수로는

- **어트리뷰트로 사용할 객체를 넣어주거나**
- props를 인자로 받고 **어트리뷰트로 사용할 객체를 반환시켜주는 함수를 넣어주면된다.**

```jsx
const Input = styled.input.attrs((props) => ({
  type: "text",
  size: props.size || "1em",
}))`
  margin: ${(props) => props.size};
  padding: ${(props) => props.size};
`;

const PasswordInput = styled(Input).attrs({
  type: "password",
})`
  border: 2px solid tomato;
`;

const StyeldComponent = () => {
  return (
    <>
      <Input size="2em" />
      <PasswordInput />
    </>
  );
};
```

<br>

## Animation

스타일드 컴포넌트에 애니메이션을 적용하는 방법은 다음과 같다.

1. styled-component 모듈에서 `keyframes import` 해온다.
2. 원하는 keyframes의 css를 만든후 변수에 할당시켜준다.
3. 컴포넌트의 `animation` 의 name 값으로 keyframes 변수를 할당한다.

<br>

코드 예제)

```jsx
import styled, { keyframes } from 'styled-components';

const boxFade = keyframes`
    11.1% {
      transform: none;
    }
    ...
    100% {
      transform: none;
    }
`;

const AnimateContainer = styled.div`
  animation: ${boxFade} 2s infinite;
  transform-origin: center;
`;

const Main = () => (
    <>
        <AnimateContainer>
          <h1>Diary</h1>
        </AnimateContainer>
		<>
)
```

![Styled Components](../Images/styled_component_animation/ezgif.com-gif-maker.gif)

<br>

## Theme

**자주 사용하려는 색상, 폰트 사이즈, 크기.. 등등을 미리 변수에 지정해 Component에서 전역 변수로 사용할 수 있는 객체를 Theme이라고 한다.**

<br>

따로 theme.js 파일을 만들어서 사용하는게 가독성에 좋다.

<br>

**theme.js**

```jsx
const margins = {
  sm: "1rem",
  base: "2rem",
  lg: "3rem",
  xl: "4rem",
};

const paddings = {
  sm: "1rem",
  base: "2rem",
  lg: "3rem",
  xl: "4rem",
};

const color = {
  awesomeColor: "#f928",
};

export const defalutTheme = {
  color,
  margins,
  paddings,
};
```

<br>

**App.js**

```jsx
import StyeldComponent from "./styledComponent/StyeldComponent";
import { ThemeProvider } from "styled-components";
import { defalutTheme } from "./styledComponent/theme";

function App() {
  return (
    <>
      <ThemeProvider theme={theme}>
        <StyeldComponent />
      </ThemeProvider>
    </>
  );
}

export default App;
```

<br>

**StyledComponent.js**

```jsx
const ThemeDiv = styled.div`
  color: ${(props) => props.theme.color.awesomeColor};
`;

const StyeldComponent = () => {
  return <ThemeDiv>Hellow world</ThemeDiv>;
};
```

<br>

**props.theme을 통해 미리 필요한 CSS 정보 저장한뒤, 불러올 수 있다.**

<br>

## Global Style

**전역 Style을 적용하는 방법도 있다.**

**props를 전달받아서 조건에 따라 다르게 적용도 가능하다.**

<br>

```jsx
import StyeldComponent from "./styledComponent/StyeldComponent";
import { createGlobalStyle, ThemeProvider } from "styled-components";
import { defalutTheme } from "./styledComponent/theme.js";
import { css } from "styled-components";

function App() {
  const GlobalStyle = createGlobalStyle`
    div {
      background: indigo;
    }

    ${(props) => css`
      section {
        padding: ${props.theme.paddings.sm};
      }
    `}
  `;

  return (
    <>
      <ThemeProvider theme={defalutTheme}>
        <GlobalStyle />
        <StyeldComponent />
      </ThemeProvider>
    </>
  );
}

export default App;
```

<br>

참고

- [https://styled-components.com/docs/basics](https://styled-components.com/docs/basics)
- [https://www.youtube.com/watch?v=FSCSdAlLsYM&list=PLC3y8-rFHvwgu-G08-7ovbN9EyhF_cltM&index=1](https://www.youtube.com/watch?v=FSCSdAlLsYM&list=PLC3y8-rFHvwgu-G08-7ovbN9EyhF_cltM&index=1)

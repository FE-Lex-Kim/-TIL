# styled component Keyframe animation

<br>

스타일드 컴포넌트에 애니메이션을 적용하는 방법은 다음과 같다.

<br>

1. styled-component 모듈에서 keyframes import 해온다.
2. 원하는 keyframes의 css를 만든후 변수에 할당시켜준다.
3. 컴포넌트의 animation 의 name 값으로 keyframes 변수를 할당한다.

<br>

코드 예제)

<br>

```jsx
import styled, { keyframes } from 'styled-components';

const boxFade = keyframes`
    11.1% {
      transform: none;
    }
    22.2% {
      transform: skewX(-12.5deg) skewY(-12.5deg);
    }
    33.3% {
      transform: skewX(6.25deg) skewY(6.25deg);
    }
    44.4% {
      transform: skewX(-3.125deg) skewY(-3.125deg);
    }
    55.5% {
      transform: skewX(1.5625deg) skewY(1.5625deg);
    }
    66.6% {
      transform: skewX(-0.78125deg) skewY(-0.78125deg);
    }
    77.7% {
      transform: skewX(0.390625deg) skewY(0.390625deg);
    }
    88.8% {
      transform: skewX(-0.1953125deg) skewY(-0.1953125deg);
    }
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

<br>

![styled_component_animation](../Images/styled_component_animation/ezgif.com-gif-maker.gif)
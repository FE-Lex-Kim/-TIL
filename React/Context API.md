# Context API

<br>

### Context Consumer

리액트 컴포넌트에서 전역적으로 사용할 상태를 관리할때

가장 최상위 컴포넌트APP에 state를 넣어서 관리한다.

<br>

그리고 그 자식 컴포넌트에 props로 전달하여 컴포넌트 전역에서 전달받은 props로 상태를 관리한다.

<br>

하지만 컴포넌트가 굉장히 복잡하고 깊은경우 유지보수성이 낮아질 가능성이 있다.

<br>

Context API를 사용하면 전역상태 관리 작업을 할때 따로 상태를 보관해 필요한 컴포넌트에 직접 사용할 수 있도록 하여 쉽게 처리할 수 있다.

<br>

예제

```jsx
import { creactContext } from 'react';

const ColorContext = creactContext({ color: 'black' });

export default ColorContext;
```

<br>

creatContext를 react에서 불러와서 함수안에 Context의 초기값으로 넣어주면된다.

<br>

본격적으로 다른 컴포넌트에서 Context를 사용하는 예제를 공부해보자

예제

```jsx
import React from 'react';
import ColorContext from '../Context/color';

const ColorBox = () => {
  return (
    <ColorContext.Consumer>
      {({ color }) => (
        <div
          style={{
            width: '64px',
            height: '64px',
            background: color,
          }}
        />
      )}
    </ColorContext.Consumer>
  );
};

export default ColorBox;
```

<br>

Context을 직접 불러와서 JSX로 감싸주어서 사용하면된다.

<br>

**하지만 여기서 주의점!**

import한 Context그자체를 사용하면안된다.

Consumer이라는 컴포넌트를 통해 Context의 초기값을 접근할 수 있다.

<br>

따라서 위에 ColorContext.Consumer로 컴포넌트를 만들어 그안에

원래 상태를 받으려는 컴포넌트를 작성해주면된다.

<br>

또 다시 여기서 주의점!**

그냥 컴포넌트를 작성해주면안된다.

Context.Consumer 컴포넌트안에 중괄호를 넣어주어

그안에서 함수로써 호출해주어야한다.

그 함수에서 매개변수의 값은 Context의 초기값이 오게된다.

<br>

**또 다시 여기서 주의점!**

중괄호안에 함수를 넣고 화살표 함수이므로 소괄호로 리턴해주어야한다.

<br>

### Context Provider

<br>

Provider를 사용하면 Context의 초기값을 변경해줄 수 있다.

변경해줄때는 Cotnext를 사용하였던 컴포넌트를 JSX로 사용한 부모컴포넌트의 위에 사용해주어야한다.

<br>

예제

```jsx
import React from 'react';
import ColorBox from './component/ColorBox';
import ColorContext from './Context/color';

function App() {
  return (
    <ColorContext.Provider value={{ color: 'black' }}>
      <ColorBox />
    </ColorContext.Provider>
  );
}

export default App;
```

<br>

Provider을 사용한뒤 Props에 value로 Context를 갱신하려는 값을 넣어주면된다.

<br>

**여기서 주의!**

만약 value를 넣어주지 않으면 오류가난다.

Context에 creatContext함수로 초기화한 값은 Provider가 사용되지 않을때 값이 사용된다.

<br>

**반드시 Provider를 사용하면 value 값 명시해주기!**

<br>

### Context 값 업데이트하기

<br>

위의 Consumer과 Provider를 사용하면 정적일때만 적용이 가능하다.

무슨말이냐면 이벤트로 Context의 값을 바꾸고 싶을때는 못바꾼다.

<br>

따라서 동적인 Context를 만드는 방법을 공부해보자.

```jsx
import { createContext, useState } from 'react';

const ColorContext = createContext({
  state: { color: 'black', subColor: 'red' },
  actions: { setColor: () => {}, setSubColor: () => {} },
});

const ColorProvider = ({ children }) => {
  const [color, setColor] = useState('black');
  const [subColor, setSubColor] = useState('red');

  const value = {
    state: { color, subColor },
    actions: { setColor, setSubColor },
  };

  return (
    <ColorContext.Provider value={value}>{children}</ColorContext.Provider>
  );
};

const { Consumer: ColorConsumer } = ColorContext;

export { ColorConsumer, ColorProvider };

export default ColorContext;
```

<br>

APP 컴포넌트에 반영하기

```jsx
import React from 'react';
import ColorBox from './component/ColorBox';
import ColorContext, { ColorProvider } from './Context/color';

function App() {
  return (
    <ColorProvider value={{ color: 'black' }}>
      <div>
        <ColorBox />
      </div>
    </ColorProvider>
  );
}

export default App;
```

<br>

기존에 Context.Provider에서 Provider로 더 줄여서 표현했다.

<br>

또한 애초에 Provider 컴포넌트를 만들때 JSX에 value props를 미리 만들어놔서 APP에서는 안적어 놔도 된다.

<br>

ColorBox 파일 변경

예제)

```jsx
import React from 'react';
import ColorContext, { ColorConsumer } from '../Context/color';

const ColorBox = () => {
  return (
    <ColorConsumer>
      {({ state }) => (
        <>
          <div
            style={{
              width: '64px',
              height: '64px',
              background: state.color,
            }}
          />
          <div
            style={{
              width: '32px',
              height: '32px',
              background: state.subColor,
            }}
          />
        </>
      )}
    </ColorConsumer>
  );
};

export default ColorBox;
```

<br>

기존에 Context.consumer 에서 Consumer로 간단하게 표기했다.

또한 background 컬러를 바꾸어주었다.

<br>

### 클릭이벤트시 Cotnext state 변경하기

<br>

```jsx
import { ColorConsumer } from '../Context/color';

const colors = ['red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'violet'];

const SelectColors = () => {
  return (
    <div>
      <h2>색상을 선택하세요.</h2>
      <ColorConsumer>
        {({ actions }) => (
          <div style={{ display: 'flex' }}>
            {colors.map((color) => (
              <div
                key={color}
                style={{
                  width: '24px',
                  height: '24px',
                  cursor: 'pointer',
                  background: color,
                }}
                onClick={() => {
                  actions.setColor(color);
                }}
                onContextMenu={(e) => {
                  e.preventDefault();
                  actions.setSubColor(color);
                }}
              />
            ))}
          </div>
        )}
      </ColorConsumer>
      <hr />
    </div>
  );
};

export default SelectColors;
```

<br>

ColorConsumer에서 원래 Consumer하듯이 중괄호 안에 함수로써 작성을 해준다.

Context의 기본값은 APP에서 Provider에의해 value로 인해 바뀌었다.

<br>

따라서 Context의 actions의 setColor와 setSubColor가 state의 값을 바꿀수 있게 되었다.

그대로 actions를 디스트럭처링 할당으로 받아온뒤 클릭이벤트와 우클릭이벤트로 

state값을 바꾸어준다.

<br>

그렇게 되었을때 ColorBox의 큰 박스는 state의 Color값이므로 클릭시 바뀌게 되고

ColorBox의 작은 박스는 state의 subColor값이므로 우클릭시 바뀌게된다.

<br>

**참고!**

onContextMenu는 우클릭 이벤트이다.

<br>

### Consumer 대신 Hook 사용

<br>

지금까지 React의 createContext API를 통해 사용했다.

굉장히 매력적인 Hook를 사용해 간단하게 만들수있다.

```jsx
import React, { useContext } from 'react';
import ColorContext from '../Context/color';

const ColorBox = () => {
  const { state } = useContext(ColorContext);
  return (
    <>
      <div
        style={{
          width: '64px',
          height: '64px',
          background: state.color,
        }}
      />
      <div
        style={{
          width: '32px',
          height: '32px',
          background: state.subColor,
        }}
      />
    </>
  );
};

export default ColorBox;
```

<br>

useContext함수의 매개변수 값에 Context를 넣어준다.

변수에 할당해주면 Context의 값이 할당이 된다.

<br>

위의 예제는 디스트럭처링 할당으로 초기값에서 state만 가져왔다.

<br>

Render Props패턴이 힘드니

간단하게 useContext Hook를 사용하면 편하게 Context값을 조회가 가능하다.

당연하게 Hook이니 함수형 컴포넌트에만 사용이 가능하다.

<br>

참고: 리액트를 다루는 기술
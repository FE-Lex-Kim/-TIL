- [Storybook(2) - Story within Story, Args, Decorators, Addons](#storybook2---story-within-story-args-decorators-addons)
  - [Story within Story](#story-within-story)
  - [Args](#args)
  - [Decorators](#decorators)
  - [Addons](#addons)
    - [Backgrounds Addon](#backgrounds-addon)
    - [Controls Addon](#controls-addon)
    - [Actions Addon](#actions-addon)
    - [Docs Addon](#docs-addon)
    - [Viewport Addon](#viewport-addon)
    - [Outline Addon](#outline-addon)
    - [Measure Addon](#measure-addon)

<br>

# Storybook(2) - Story within Story, Args, Decorators, Addons

<br>

## Story within Story

**Story에 다른 Story를 불러와 붙여서 사용할 수 있다.**

Stroy가 변경되면, **변경된 Story를 사용한 Story 또한 그대로 변경된다.**

**따라서 코드의 재사용이 가능하고 관리하기가 좋다.**

<br>

```jsx
import React from "react";
import { Orange } from "./Button.stories.js";
import { Tomato } from "./Input.stories.js";

export default {
  title: "form/DoubleButtons",
};

export const OrnagToamtoButton = () => (
  <>
    <Orange />
    <Tomato />
  </>
);
```

<br>

## Args

지금까지 Story를 JSX 통해 컴포넌트를 만들어왔다.

```jsx
import React from "react";
import Button from "./Button";

export default {
  title: "Form/Button",
  component: Button,
};

export const Blue = () => <Button color="blue">Blue</Button>;
export const Gray = () => <Button color="gray">gray</Button>;
```

<br>

**다른 방법으로 Story를 만들 수 있는 방법이 있다.**

각각의 JavaScript object을 정의하는 메커니즘으로 Story를 만든다.

```jsx
export default {
  title: "form/button",
  component: Button,
};

// export const Blue = () => <Button color="blue">Blue</Button>;
// export const Gray = () => <Button color="gray">gray</Button>;

const Template = (args) => <Button {...args} />;

export const Blue = Template.bind({});
Blue.args = {
  color: "blue",
  children: "Blue",
};

export const Gray = Template.bind({});
Gray.args = {
  color: "gray",
  children: "Gray",
};
```

<br>

확실히 보이기에는 JSX를 통한 Story 생성이 더 쉽고 가독성이 좋아 보인다.

<br>

하지만 **Args를 사용하는 이유는**

- **더 복잡한 로직**을 props로 사용할 수 있다는 점이다.
- 다른 Story에 **args을 전달해 재사용이** 가능하다는 점이다.

  ```jsx
  export const Gray = Template.bind({});
  Gray.args = {
    color: "gray",
    children: "Gray",
  };

  export const LongGray = Template.bind({});
  LongGray.args = {
    ...Gray.args,
    children: "LongGray",
  };
  ```

- **유니크한 args**을 적용 가능하다는 점이다.

<br>

또한 **모든 Story 컴포넌트의 args에 공통적으로 적용시킬 수 있다.**

```jsx
export default {
  title: "form/button",
  component: Button,
  args: {
    children: "Button",
  },
};

const Template = (args) => <Button {...args} />;

export const Blue = Template.bind({});
Blue.args = {
  color: "blue",
  // children: "Blue",
};

export const Gray = Template.bind({});
Gray.args = {
  color: "gray",
  // children: "Gray",
};
```

- 모든 Story 컴포넌트들의 args.chidren을 ‘Button”으로 공통적으로 적용시켰다.
- 만약 주석된 부분을 다시 푼다면, **각각 Story에 적용된 args를 우선시한다.**

<br>

## Decorators

Storybook을 보면 모든 컴포넌트들은 좌상단에 고정되어 있다.

![Storybook](<../Images/Storybook(2)/Storybook(2)-1.png>)

<br>

**Storybook에서만 UI가 보기 좋게 모든 Story 컴포넌트에 공통적으로 CSS 적용 시키는 방법에 대해 알아보자.**

<br>

모든 컴포넌트들을 Storybook에서 보기좋게 가운대 정렬시켜보자.

<br>

Center.js

```jsx
// Center.css
.center {
  display: flex;
  justify-content: center;
}

import React from "react";
import "./Center.css";

const Center = (props) => {
  return <div className="center">{props.children}</div>;
};

export default Center;
```

<br>

이제 모든 Stroy 컴포넌트에 Center 컴포넌트로 감싸주면 가운데 정렬이 될것이다.

```jsx
export default {
  title: "form/button",
  component: Button,
};

export const Blue = () => (
  <Center>
    <Button color="blue">Blue</Button>
  </Center>
);

export const Gray = () => (
  <Center>
    <Button color="gray">Gray</Button>
  </Center>
);
```

<br>

**하지만 일일이 Story 컴포넌트 마다 감싸주는것은 번거롭다.**

이럴때 Storybook이 제공하는 좋은 방법이 있다.

```jsx
export default {
  title: "form/button",
  component: Button,
  decorators: [(story) => <Center>{story()}</Center>],
};

export const Blue = () => <Button color="blue">Blue</Button>;
export const Gray = () => <Button color="gray">gray</Button>;
```

**이제 Story 컴포넌트들은 Center이 감싸주게 된다.**

하지만 form/button에 있는 Story 컴포넌트 들만 Center이 감싸주게 된다.

<br>

모든 Story들을 Global 하게 감싸주는 방법을 알아보자.

**preview.js 파일에 decorators을 export해서 설정할 수 있다.**

<br>

.storybook/preview.js

```jsx
import React from "react";
import Center from "../src/storybookComponent/Center";

export const parameters = {
		....
};

export const decorators = [(story) => <Center>{story()}</Center>];
```

<br>

## Addons

**Storybook에 내장되지 않은 효율적인 작업을 위한 기능들을 추가할 수 있다.**

이러한 기능들을 **애드온**이라고 한다.

<br>

대부분의 **Storybook 기능은 애드온을 통해 구현된다.**

Storybook은 수백개의 애드온들이 NPM에 있다.

유용하고 다양한 애드온을 알아보는 방법은 [**공식홈페이지**](https://storybook.js.org/addons)를 추천한다.

<br>

[**필수적인 애드온**](https://storybook.js.org/addons/tag/essentials/)에 대해 알아보자.

![Storybook](<../Images/Storybook(2)/Storybook(2)-2.png>)

<br>

### Backgrounds Addon

**Light, Dark 모드**를 디자인 할때 쉽게 바꿀 수 있게해 유용하다.

![Storybook](<../Images/Storybook(2)/Storybook(2)-3.png>)

<br>

### Controls Addon

**상호작용을 통해 컴포넌트의 args을 동적으로 변경가능하게 해주는 애드온이다.**

Controls 애드온은 **args의 값을 조정하는 UI을 제공**해 시각적으로 변경하는 것을 쉽게 볼 수 있어 굉장히 유용하다.

<br>

**Controls 애드온을 사용하기 위해서는 args을 사용**해서 Story 컴포넌트를 만들어야한다.

```jsx
export default {
  title: "form/button",
  component: Button,
  args: {
    children: "Button",
  },
  argTypes: {
    color: {
      control: {
        options: ["blue", "orange", "gray"],
        type: "radio",
      },
    },
  },
};

export const Template = (args) => <Button {...args} />;

export const Orange = Template.bind({});
Orange.args = {
  color: "orange",
  children: "Orange",
};

export const Tomato = Template.bind({});
Tomato.args = {
  color: "tomato",
  children: "Tomato",
};
```

arg에 type을 설정해서 UI로 설정이 가능하다.

다른 시나리오 테스트에 매우 유용하다.

<br>

![Storybook](<../Images/Storybook(2)/Storybook(2)-4.png>)

<br>

**Controls 애드온에서 사용할 수 있는 argType이다.**

![Storybook](<../Images/Storybook(2)/Storybook(2)-5.png>)

<br>

[**Control documentation**](https://storybook.js.org/docs/react/essentials/controls)

<br>

### Actions Addon

**이벤트가 발생했을때, 이벤트 정보를 보여준다.**

**이벤트가 발생했는지 확실히 알수있게 해준다.** 또한 여러가지 이벤트에 대한 정보 알 수 있다.

```jsx
export default {
  title: "form/button",
  component: Button,
  argTypes: {
    onClick: { action: "이곳에 로그에 나올 이벤트핸들러 이름을 설정" },
  },
};
```

<br>

Button.js

```jsx
import React from "react";
import "./Button.css";

const Button = ({ color, children, onClick }) => {
  return (
    <button className={`button ${color}`} onClick={onClick}>
      {children}
    </button>
  );
};

export default Button;
```

<br>

[**Actions documentation**](https://storybook.js.org/docs/react/essentials/actions)

<br>

### Docs Addon

### Viewport Addon

### Outline Addon

### Measure Addon

<br>

참고

- [https://storybook.js.org/tutorials/intro-to-storybook](https://storybook.js.org/tutorials/intro-to-storybook)
- [https://storybook.js.org/addons](https://storybook.js.org/addons)
- [https://storybook.js.org/addons/tag/essentials/](https://storybook.js.org/addons/tag/essentials/)
- [https://carpediem9911.tistory.com/45#3-actions-설정](https://carpediem9911.tistory.com/45#3-actions-%EC%84%A4%EC%A0%95)
- [https://www.youtube.com/watch?v=BySFuXgG-ow&list=PLC3y8-rFHvwhC-j3x3t9la8-GQJGViDQk&index=1](https://www.youtube.com/watch?v=BySFuXgG-ow&list=PLC3y8-rFHvwhC-j3x3t9la8-GQJGViDQk&index=1)

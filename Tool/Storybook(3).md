# Storybook Typescript

## Storybook 환경설정 with TypeScript

타입스크립트로 스토리북을 타입을 지정해 스토리북 자동완성과 타입체킹을 하게 해준다.

```tsx
// Replace your-framework with the framework you are using (e.g., react-webpack5, vue3-vite)
import type { StorybookConfig } from "@storybook/your-framework";

const config: StorybookConfig = {
  // Required
  framework: "@storybook/your-framework",
  stories: ["../src/**/*.mdx", "../src/**/*.stories.@(js|jsx|mjs|ts|tsx)"],
  // Optional
  addons: ["@storybook/addon-essentials"],
  docs: {
    autodocs: "tag",
  },
  staticDirs: ["../public"],
};

export default config;
```

<br>

## Story 작성 with TypeScript

스토리북은 타입스크립트 환경설정을 딱히 해줄필요가 없다.(알아서 해준다)

타입을 지정을 통해 타입 안정성과 코드완성을 향상시킬수 있다.

타입지정하는 스토리도 딱히 어렵지 않다.

```tsx
// Replace your-framework with the name of your framework
import type { Meta, StoryObj } from "@storybook/your-framework";

import { Button } from "./Button";

const meta: Meta<typeof Button> = {
  component: Button,
};

export default meta;
type Story = StoryObj<typeof Button>;

//👇 Throws a type error it the args don't match the component props
export const Primary: Story = {
  args: {
    primary: true,
  },
};
```

Meta, StoryObj 제네릭 타입을 사용했다.

나머지 코드는 고정적으로 두고 나머지 Button(컴포넌트) 부분만 바꿔서 넣어주면된다.

<br>

## 4.9 타입스크립트 버전

4.9 타입스크립트에서 satisfies를 사용할 수 있다.

약간 달라진 부분이 있지만 크게 당황하지 않아도 된다.

단지 컴포넌트 네이밍 부분만 바꿔주면 된다.

```tsx
// Replace your-framework with the name of your framework
import type { Meta, StoryObj } from "@storybook/your-framework";

import { Button } from "./Button"; // <-- 여기 수정

const meta = {
  component: Button, // <-- 여기 수정
} satisfies Meta<typeof Button>; // <-- 여기 수정

export default meta;
type Story = StoryObj<typeof meta>;

export const Example = {
  args: {
    primary: true,
    label: "Button",
  },
} satisfies Story;
```

<br>

참고

- https://storybook.js.org/docs/configure/typescript

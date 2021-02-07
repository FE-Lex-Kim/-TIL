# tailwindcss Complex variants

<br>

tailwindcss를 사용하다가 단순한 선택자를 수정하는것이 아니라 좀더 복잡한 css 선택자를 이용한 작업을 해야하는 경우에는 `'container'` 인스턴스를 이용해야한다.

<br>

컨테이너 인스턴스를 사용하여 사용하면 모듈에있는 규칙과 @variants 에 있는 모든 규칙들을 보다 우선권을 가지고 정의하여 사용할 수 있게 된다.

<br>

예시) focus가 되었을때 그다음 인접 요소가 클래스를 focused-sibling을 가진다면 css을 적용시켜준다.

```jsx
// tailwind.config.js

const plugin = require("tailwindcss/plugin");

const focusedSiblingPlugin = plugin(function ({ addVariant, e }) {
  addVariant("focused-sibling", ({ container }) => {
    container.walkRules((rule) => {
      rule.selector = `:focus + .focused-sibling\\:${rule.selector.slice(1)}`;
    });
  });
});

module.exports = {
  // ...
  plugins: [focusedSiblingPlugin],
  variants: {
    extend: {
      backgroundColor: ["focused-sibling"],
    },
  },
};
```

<br>

클래스로 적용시킬땐 `foucsed-sibling:bg-gray-200` 과 같이하면 이전에 focus된 요소가 선택될시 css가 적용된다.

<br>

참고

- [https://stackoverflow.com/questions/63334626/tailwind-css-is-there-a-way-to-target-next-sibling](https://stackoverflow.com/questions/63334626/tailwind-css-is-there-a-way-to-target-next-sibling)
- [https://tailwindcss.com/docs/plugins#complex-variants](https://tailwindcss.com/docs/plugins#complex-variants)

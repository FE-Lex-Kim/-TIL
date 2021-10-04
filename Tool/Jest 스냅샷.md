# Jest 스냅샷

스냅샷 테스트는 UI가 예기치 않게 변경되지 않도록 하려는 경우 매우 유용한 도구이다.

복잡한 디자인, 로직에 대한 테스트 코드를 한줄한줄 작성을 하는 것은 비효율 적이다.

<br>

스냅샷을 통해서 예상된 결과물에 대한 컴포넌트를 찍어 두고 직접 테스트 코딩을 하지 않아도 된다는 편리함이 있다.

<br>

하지만 무분별하게 사용하면 스냅샷 자체의 유지보수가 어려워진다.

따라서 테스트의 예상 결과를 개발자가 직접 작성하는데 시간과 노력이 많이 들어가는 경우에만 제한적으로 사용하는 것을 권장한다.

<br>

## 스냅샷 사용

`toMatchSnapshot()` 을 사용하면 스냅샷 파일을 만든다.

해당 스냅샷을 찍은 컴포넌트를 기준으로 다음 테스팅 했을때의 컴포넌트와 비교한다.

<br>

```jsx
import React from "react";
import Count from "./Count";
const { render, fireEvent } = require("@testing-library/react");

describe("Count 컴포넌트 테스트", () => {
  it("snapshot: name 있음", () => {
    const el = render(<Count name="Alex" />);
    expect(el).toMatchSnapshot();
  });

  it("snapshot: name 없음", () => {
    const el = render(<Count />);
    expect(el).toMatchSnapshot();
  });
});
```

<br>

Count.test.js.snap

```jsx
// Jest Snapshot v1, https://goo.gl/fbAQLP

exports[`Count 컴포넌트 테스트 snapshot: name 없음 1`] = `
Object {
  "asFragment": [Function],
  "baseElement": <body>
    <div>
      <div>
        0
      </div>
    </div>
  </body>,
  "container": <div>
    <div>
      0
    </div>
  </div>,
  ...
}
`;

exports[`Count 컴포넌트 테스트 snapshot: name 있음 1`] = `
Object {
  "asFragment": [Function],
  "baseElement": <body>
    <div>
      <div>
        Alex 0
      </div>
    </div>
  </body>,
  "container": <div>
    <div>
      Alex 0
    </div>
  </div>,
  ...
}
`;
```

<br>

## 스냅샷 업데이트

스냅샷을 찍어두고 이후 테스트와 비교한다.

처음에 찍어두었던 내용을 바탕으로 테스트를 하기 때문에, 이후에 스냅샷을 변경한 로직으로 업데이트한후 테스트를 하고 싶을때 사용할 수 있다.

<br>

![Jest 스냅샷](/Images/Jest%20스냅샷/Jest%20스냅샷-1.png)

<br>

**u 을 누르면 해당 실패한 스냅샷을 업데이트한다.**

<br>

참고

- [https://www.daleseo.com/jest-snapshot/](https://www.daleseo.com/jest-snapshot/)
- [https://jestjs.io/docs/snapshot-testing](https://jestjs.io/docs/snapshot-testing)
- [https://www.youtube.com/watch?v=g4rMWtPNOr8&list=PLZKTXPmaJk8L1xCg_1cRjL5huINlP2JKt&index=6](https://www.youtube.com/watch?v=g4rMWtPNOr8&list=PLZKTXPmaJk8L1xCg_1cRjL5huINlP2JKt&index=6)

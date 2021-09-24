# Vanilla Javascript useEffect

[Vanilla Javascript useState](https://github.com/FE-Lex-Kim/-TIL/blob/master/React/Vanilla%20Javascript%20useState.md) 의 내용을 그대로 이어서 useEffect에 대해 공부해보자.

<br>

useEffect가 어떻게 의존성 배열에 따라 callBack 함수가 호출되는지 알아보자.

```jsx
const React = (() => {
  let hooks = [];
  let idx = 0;
  const useEffect = (cb, depArr) => {
    const oldDeps = hooks[idx];
    let hasChanged = true;
    if (oldDeps) {
      hasChanged = depArr.some((dep, i) => !Object.is(dep, oldDeps[i]));
    }

    if (hasChanged) cb();
    hooks[idx] = depArr;
    idx++;
  };

  const useState = (init) => {
    let state = hooks[idx] || init;
    let _idx = idx;
    const setState = (nextState) => {
      hooks[_idx] = nextState;
    };
    idx++;

    return [state, setState];
  };
  const render = (Component) => {
    idx = 0;
    let C = Component();

    C.render();

    return C;
  };

  return { render, useState, useEffect };
})();

let Component = () => {
  let [count, setCount] = React.useState(1);
  let [type, setType] = React.useState("Apple");
  React.useEffect(() => {
    console.log("Alex!!");
  }, [count]);
  return {
    render: () => {
      console.log({ count, type });
    },
    click: () => {
      setCount(count + 1);
    },
    type: () => {
      setType("Orange");
    },
  };
};

var App = React.render(Component);
App.click();
var App = React.render(Component);
App.type("Orange");
var App = React.render(Component);
```

hooks를 사용하게되면 React의 `hooks` 배열에 하나씩 해당 값을 할당하고 `idx` 가 1씩 증가한다.

해당값이란 useState일때는 **state**가 hooks 배열에 들어간다.

useEffect 일땐 **의존성 배열**이 들어간다.

<br>

useEffect가 호출되었을때 로직을 한줄한줄 살펴보자.

3가지 경우가 있다.

첫번째 의존성 배열에 state가 들어가 있을때,

두번째 의존성 배열이 빈 배열일때,

세번째 두번째 인수에 아무값도 넣지 않을떄,

<br>

### 첫번째 의존성 배열에 state가 들어가 있을때(ex count)

1. 첫번째 `React.render(Component)` 가 호출된다.
2. Component 내부 로직중 `useEffect`가 호출이 된다. 이때 의존성 배열값은 `[1]` 이다.
3. `oldDeps`의 값은 `hooks[idx]` 값인데, 현재 `idx`값은 2이다.
   1. 이전 `useState`가 두번 호출 되어서 1씩 증가하므로 총 2가 된다.
   2. 따라서 `hooks[2]`값은 이전에 hooks 배열에 할당한 값이 없어서 `undefined`가 되므로 `oldDeps` 값은 `undefined`가 된다.
   3. `oldDeps`가 falsy 값이므로 조건문이 실행되지 않는다.
   4. `hasCanged` 값은 그대로 `true`이므로 `cb`이 호출된다.
   5. 이후 `hooks[3]` 값에 현재 `depArr` 값이 바인딩 된다. 현재 depArr 값은 count인 값인 1이 들어가 있다.(`[1]`)
   6. 이후 다음 hooks가 사용할 index 위치를 위해서 `idx`를 1증가 시켜준다.
4. 두번째 `App.click()`이 호출된다.
   1. count state값이 변경되어진다.
   2. 이후 useEffect의 의존성 배열의 값이 변경되어 `2`가 들어간다.
5. 두 번째 `React.render(Component)` 가 호출된다.
6. Component 내부 로직중 `useEffect`가 호출이 된다. 이때 의존성 배열의 값은 `[2]` 이다.
7. `oldDeps`의 값은 `hooks[idx]` 값인데, 현재 `idx`값은 `2`이다.
   1. 이전 `useState`가 두번 호출 되어서 1씩 증가하므로 총 `2`가 된다.
   2. `hooks[2]`값은 이전에 hooks 배열에 할당한 값인 `1` 이다. 따라서 `oldDeps`의 값은 `1`이 들어간다.
   3. `oldDeps`가 `trusy` 값이므로 조건문이 실행된다.
      1. 이전 의존성 배열의 상태값과 현재 들어온 의존성 배열을 비교한다.`depArr.some((dep, i) => !Object.is(dep, oldDeps[i]));`
      2. 이때 `Object.is`를 사용하는 이유는 `NaN`값과 `NaN`값을 비교하기 위해서이다.
      3. 만약 이전 의존성 배열과 현재 의존성 배열의 요소를 각각 비교하는데 만약 **서로 다른 값**이 있다면 `hasChanged`에 `true`를 반환한다.
      4. 만약 서로 모두 같은 값이라면 `hasChanged`에 `false`를 반환한다.
   4. `hasCanged` 값은 `true`이므로 `cb`이 호출된다.
   5. 이후 `hooks[3]` 값에 현재 `depArr` 값이 바인딩 된다. 현재 depArr 값은 count인 값인 2이 들어가 있다.(`[2]`)
   6. 이후 다음 hooks가 사용할 index 위치를 위해서 `idx`를 1증가 시켜준다.

<br>

위의 로직 과정이 반복되어진다.

<br>

### 두 번째인 빈 배열이 들어갔을때

1. 첫번째 `React.render(Component)` 가 호출된다.
2. Component 내부 로직중 `useEffect`가 호출이 된다. 이때 의존성 배열값은 `[]` 이다.
3. `oldDeps`의 값은 `hooks[idx]` 값인데, 현재 `idx`값은 2이다.
   1. 따라서 `hooks[2]`값은 이전에 hooks 배열에 할당한 값이 없어서 `undefined`가 되므로 `oldDeps` 값은 `undefined`가 된다.
   2. `oldDeps`가 falsy 값이므로 조건문이 실행되지 않는다.
   3. `hasCanged` 값은 그대로 `true`이므로 `cb`이 호출된다.
   4. 이후 `hooks[3]` 값에 현재 `depArr` 값이 바인딩 된다. 현재 depArr 값은 빈배열이다.(`[]`)
   5. 이후 다음 hooks가 사용할 index 위치를 위해서 `idx`를 1증가 시켜준다.
4. 이후 상태값이 변한다고 해도 빈배열 값이 `oldDeps`에 들어간다.
5. 빈배열은 `trusy`한 값이다. 따라서 조건문이 실행된다.
6. 하지만 `Array.some`메서드는 빈배열에 호출하면 무조건 `false` 값으 반환한다. 따라서 현재 `depArr`는 빈배열이므로 `hasChanged`는 `false`값이 된다.
7. 따라서 `cb`은 호출되지 않는다.

<br>

### 세번째인 값을 할당하지 않을때

값을 할당하지 않으면 `depArr`값은 `undefined`값이 들어간다.

<br>

1. 첫번째 `React.render(Component)` 가 호출된다.
2. Component 내부 로직중 `useEffect`가 호출이 된다. 이때 의존성 배열값은 `undefined` 이다.
3. `oldDeps`의 값은 `hooks[idx]` 값인데, 현재 `idx`값은 2이다.
   1. 따라서 `hooks[2]`값은 이전에 hooks 배열에 할당한 값이 없어서 `undefined`가 되므로 `oldDeps` 값은 `undefined`가 된다.
   2. `oldDeps`가 `falsy` 값이므로 조건문이 실행되지 않는다.
   3. `hasCanged` 값은 그대로 `true`이므로 `cb`이 호출된다.
   4. 이후 `hooks[3]` 값에 현재 `depArr` 값이 바인딩 된다. 현재 `depArr` 값은 `undefined` 이다.
   5. 이후 다음 hooks가 사용할 index 위치를 위해서 `idx`를 1증가 시켜준다.
4. 이후 상태값이 변한다고 해도 `undefined` 값이 `oldDeps`에 들어간다.
5. `undefined` 값이므로. 따라서 조건문이 실행되지 않는다.
6. 따라서 이후 어떠한 상태가 변경된다고 해도 `hasChanged` 값은 항상 `true` 값이 되고 `cb`는 매번 컴포넌트가 호출될때마다 실행된다.

<br>

참고

- [https://www.youtube.com/watch?v=KJP1E-Y-xyo](https://www.youtube.com/watch?v=KJP1E-Y-xyo)

# **Reselect**

**Reselect는 useSelector의 주의사항중, 여러개의 state을 불러올때 리렌더링이 강제로 되어질 수 있다는 것을 효율적이고 성능적으로 향상 시킨채 해결할 수 있는 방법중 하나이다.**

<br>

설치

```bash
npm i reselect
```

<br>

## **Reselect을 왜 사용할까?**

### 1. state을 여러개 불러올때, 해당 컴포넌트가 리렌더링 되지 않게 하고 쉬운 로직으로 불러옴

<br>

리덕스 스토어를 공유하고 있는 다른 컴포넌트가 리렌더링 되었을때, **각각 스토어를 공유하고 있었던 컴포넌트들의 useSelector의 리턴 값을 체킹한다.**

**useSelector의 현재 리턴 값들을 확인해서, 리렌더링 이전에 리턴했던 값과 다른지 비교한다.**

실제 state값이 변하지 않았음에도 불구하고,

객체 값이 이전 참조값과 참조값이 달라 다르다고 판단해서 **강제 리렌더링을 수행한다.**

※ [Redux 공식홈페이지에 써있는 useSelector의 주의사항](https://react-redux.js.org/api/hooks#equality-comparisons-and-updates)

<br>

예제 코드)

```jsx
const CounterContainer = () => {
  const {numberOne, numberTwo, numberThree} = useSelector((state) => ({
				state.counter.numberOne,
				state.counter.numberTwo,
				state.counter.numberThree,
	}));

	...
  };
```

위의 예제는 useSelector가 여러 state을 포함한 객체를 반환한다.

1. 같은 store을 구독하고 있는 **다른 컴포넌트가 리렌더링 되어지면**
2. CounterContainer의 useSeletor의 **이전 반환값인 객체와 현재 useSelecotor을 호출한뒤 반환한 객체 값을 비교한다.**
3. 실제 state가 변하지 않았음에도 불구하고 **참조값이 달라서 CounterContainer는 다시 강제 리렌더링 되게 된다.**

<br>

[**이러한 단점을 해결하기 위해 여러가지 방법이 있지만**](https://github.com/FE-Lex-Kim/-TIL/blob/master/React/react-redux.md#useselector-%EC%A3%BC%EC%9D%98%EC%82%AC%ED%95%AD),

<br>

Reselect를 통해 쉽게 구현가능하다.

<br>

예제코드)

```jsx
import { createSelector } from "reselect";

const selectA = (state) => state.a;
const selectB = (state) => state.b;

const structuredSelector = createSelector(selectA, selectB, (a, b) => a + b);
```

<br>

**selectA, selectB의 state값이 그대로 createSelector 마지막 인수인 콜백함수의 인자값으로 들어가게된다.**

**그래서 selectA, selectB의 state값이 a,b인 값으로 들어가게된다.**

<br>

createSelector 마지막 콜백함수에 받은 **state의 값을 원하는 값으로 가공하고** **그에대한 값이 createSelector의 반환값이 된다.**(createSelector의 반환값이 structuredSelector 변수에 바인딩된다.)

<br>

예제코드의 structuredSelector 값은 a + b 값이된다.(selectA + selectB)

**이렇게 state 값들을 여러개 불러와도 강제 리렌더링이 되지 않는다.**

<br>

배열안에 넣는 방법으로도 가능하다.) → 이게 더 가독성이 높아 보인다.

```jsx
import { createSelector } from "reselect";

const selectA = (state) => state.a;
const selectB = (state) => state.b;

const structuredSelector = createSelector([selectA, selectB], (a, b) => a + b);
```

<br>

### 2. state을 불러온뒤, 원하는 값으로 가공하는 로직을 매번 리렌더링때 계산되지 않고, 메모리제이션하여 성능을 향상시킴.

<br>

**Reselect는 메모이제이션 기법을 사용해서 이전의 사용했던 결과값을 저장하고 있는다.**

리렌더링 되어도 **state 값이 변경되지 않았다면 메모이제이션 값을 가져와서 재사용한다.**

**다시 로직을 실행하지 않아 성능 향상이 된다.**

<br>

사용전 예제코드)

```jsx
const CounterContainer = () => {
  const numberOne = useSelector((state) => state.counter.numberOne);
  const numberTwo = useSelector((state) => state.counter.numberTwo);
  const numberThree = useSelector((state) => state.counter.numberThree);

  const total = numberOne + numberTwo + numberThree;
};
```

**CounterContainer이 리렌더링 된다면 total 값도 매번 다시 계산되어져서 바인딩된다.**

지금은 간단한 값이지만 복잡한 로직이면 성능에 악영향이 갈 수 도있다.

<br>

Reselct 사용후)

```jsx
import { createSelector } from 'reselect'

const numberOne = state => state.counter.numberOne;
const numberTwo = state => state.counter.numberTwo;
const numberThree = state => state.counter.numberThree;

const CounterContainer = () => {
	const total = createSelector(numberOne, numberTwo, numberThree, (a, b) => (a + b))
	...
}

```

**CounterContainer이 다시 리렌더링 되어도 numberOne, numberTwo, numberThree 값이 변경되지 않으면, `(a, b) => (a + b)` 로직을 다시 호출해서 계산하지 않고, 이전에 계산한 값을 메모이제이션 했기 때문에 그대로 가져와서 재사용한다.**

<br>

따라서 다시 로직을 실행해 결과값을 만들지 않아 성능도 크게 향상된다.

<br>

### 3. state을 불러오는 input 콜백함수를 재사용할 수 있음

<br>

```jsx
import { createSelector } from "reselect";

const numberOne = (state) => state.counter.numberOne;
const numberTwo = (state) => state.counter.numberTwo;
const numberThree = (state) => state.counter.numberThree;

export default createSelector(
  numberOne,
  numberTwo,
  numberThree,
  (a, b) => a + b
);
```

<br>

```jsx
import countTotal import './countTotal'

const CounterContainer = () => {
	const total = useSelector(countTotal)
	...
}

```

<br>

이렇게 countToal을 import해서 사용할 수 있기 때문에, 여러 컴포넌트에서 재사용도 가능하다.

<br>

참고

- 인프런 프론트엔드 최적화 강의
- [https://github.com/reduxjs/reselect](https://github.com/reduxjs/reselect)
- [https://www.npmjs.com/package/reselect](https://www.npmjs.com/package/reselect)
- [https://react-redux.js.org/api/hooks#equality-comparisons-and-updates](https://react-redux.js.org/api/hooks#equality-comparisons-and-updates)
- [https://stackoverflow.com/questions/63493433/confusion-about-useselector-and-createselector-with-redux-toolkit](https://stackoverflow.com/questions/63493433/confusion-about-useselector-and-createselector-with-redux-toolkit)

# Immer

<br>

리액트에서 불변성을 유지하며 상태를 업데이트 하는것이 중요하다.

불변성을 치겨야 객체 내부의 값이 새로 변한것을 비교하여 바뀐지 알아 최적화를 할 수 있다.

<br>

이러한 불변성을 유지하기위해서 스프레드 문법과 배열의 내장함수로 배열, 객체를 복사하고 새로운 값을 만든다. 

<br>

하지만 이러한 객체의 구조가 매우 깊어지면 불변성을 유지하기에 유지보수가 힘들다.

<br>

immer 라이브러리를 사용하면 구조가 복잡한 객체도 매우 쉽고 깔끔하게 불변성을 유지하며 업데이트 해줄 수 있다.

<br>

**설치방법**

```bash
npm i immer
```

<br>

## 사용법

<br>

불변성을 지키기위해 변경시키려고 하는 값을 현재값을 대신하는 draftState에 모두 적용시켜둔다.

draftState에 변경하려고 하는 값을 모두 적용시킨후에 nextState를 만든다.

![Immer](../Images/Immer/Immer-1.png)

<br>

**예제**

```jsx
import produce from "immer"

const baseState = [
    {
        todo: "Learn typescript",
        done: true
    },
    {
        todo: "Try immer",
        done: false
    }
]

const nextState = produce(baseState, draftState => {
    draftState.push({todo: "Tweet about it"})
    draftState[1].done = true
})
```

<br>

기존의 state는 건들지 않고 draftState를 새로 만든다.

이 draftState는 baseState의 값과 같다.

이때 draftState에 값을 변경시키는 등의 모든 작업이 완료되면 nextState를 반환한다.

<br>

**장점**

1 . 기존의 까다롭게 불변성을 위해 지켜야하는 스프레드 문법같은 작업을 하지않아도 되고 기존의 값을 변경시키는 작업을 해도된다. ex) arr.push(1)....
2 . 만들어진 nextState 객체를 변경시킬 수 없게 만들어 놓았다.
3 . 깊은 업데이트가 쉽다.
4 . Small: 3KB gzipped

## Curried producers

<br>

produce의 첫번째 파라미터값으로 함수를 넣으면 

그 파라미터의 함수가 상태를 변경시키는 함수를 반환한다.

<br>

```jsx
// mapper will be of signature (state, index) => state
const mapper = produce((draft, index) => {
    draft.index = index
})

// example usage
console.dir([{}, {}, {}].map(mapper))
//[{index: 0}, {index: 1}, {index: 2}])
```

produce에 함수값을 파라미터로 넣었을때 그 함수가 상태를 변경시키는 업데이트 함수가 된다.

<br>

함수의 첫번째 파라미터인 draft는 새로 만든 draft 객체가 된다.

<br>

### React setState 예제

<br>

```jsx
onBirthDayClick1 = () => {
    this.setState(prevState => ({
        user: {
            ...prevState.user,
            age: prevState.user.age + 1
        }
    }))
}

/**
 * ...But, since setState accepts functions,
 * we can just create a curried producer and further simplify!
 */
onBirthDayClick2 = () => {
    this.setState(
        produce(draft => {
            draft.user.age += 1
        })
    )
}
```

<br>

위와 같이 setState와 같이 썼을때 

첫번째 예제는 setState의 첫번째 파라미터에 함수가 와서 상태를 파라미터값으로 받아 변경시켰다.

<br>

두번쨰는 Immer을 사용했을때 

setState의 첫번째 파라미터값으로 proudce가 오게된다.

<br>

produce에 첫번째 파라미터가 함수로 들어오게 되면 그 함수가 업데이트 함수로 반환되므로

setState에 produce가 들어가게 되었을때 반환되는 업데이트 함수로 setState를 처리한다.

<br>

덕분에 setState의 인수에 produce를 넣어 업데이트 함수를 만들어주기만 하면된다.

<br>

참고 :  [Immer 공식 문서](https://immerjs.github.io/immer/docs/introduction)
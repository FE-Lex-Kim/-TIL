# Jest React UI 테스팅

react UI를 테스팅 할때 여러가지 쿼리를 사용해서 UI를 테스팅 할 수 있다.

<br>

테스트를 진행할때는 테스트 코드를 먼저 생성한다.

이후 테스트 코드를 통과할 가장 단순한 코드를 만든다.

<br>

이렇게 2가지 과정을 계속 반복하면서 점점 더 높은 기능을 수행하는 테스트 코드를 만든다.

<br>

이러한 내용을 배울때는 글로 배운는 것보다 훨씬 와닿는 예제코드를 보면서 공부하면 훨씬 쉽다.

[Testing Libarary](https://testing-library.com/docs/queries/about)에 다양한 쿼리들을 공부할 수 있다.

<br>

테스팅 프로세스

1. 검사할 엘리먼트 구하기
2. 이벤트 발생시키기
   - `fireEvent`, `use-event`
3. expect(테스트 하기)
   - 해당 엘리먼트가 있는지 확인
   - `not.toBeNull`, `toBeInstanceOf`

<br>

※ 테스트코드는 로직을 구성하기 위해서 중복된 코드여도 상관 없다. 직관적으로 흐르는 길을 봐야하기 때문이다.

<br>

## render

`document.body` 에 추가할 컴포넌트 또는 엘리먼트를 테스팅 할 수 있다.

인수에 전달하면된다.

```jsx
describe("Button Compoent", () => {
  it("컴포넌트 생성", () => {
    render(<Buttons />);
  });
}
```

<br>

사실 render 함수에 옵션을 추가적으로 줄 수 있다.

하지만 자주 사용되지 않기 때문에 추가적인 설명이나 공부는 [공식문서](https://testing-library.com/docs/react-testing-library/api/#render-options)에서 하길 바란다.

<br>

## render Result

사실 가장 중요한 사실은 render 함수를 실행해서 그 컴포넌트 또는 엘리먼트가 유효한 것을 테스팅하는 것이 아니다.

<br>

해당 컴포넌트 또는 엘리먼트를 더 자세히 테스팅할 도구들을 객체의 반환한 값에 들어가 있다.

<br>

더 자세한 정보들은 공식문서 [Render Result](https://testing-library.com/docs/react-testing-library/api/#render-result)에서 공부하고 이곳 글에서는 간단한 예제들을 사용하면서 공부하도록 하겠다.

<br>

## 엘리먼트 하나 선택

### getBy...

### queryBy...

### findBy...

<br>

## 쿼리 조합

### ByRole

### ByLabelText

### ByPlaceholderText

### ByText

### ByDisplayValue

### ByAltText

### ByTitle

### ByTestId

<br>

## 이벤트

### fireEvent

- 이벤트 테스트를 도와주는 객체
- 객체값이고 이곳안에 여러가지 이벤트를 실행할 수 있는 메서드들이 있다.
  - ex) click, scroll, keydown, change....
  - `fireEnvet.click`

### user-event

```jsx
userEvent.type(Element, "Hello world");
```

→ Element에 'Hello world' 라고 입력했다. 라고 테스팅

<br>

fireEvent는 직접실행 시키고 테스팅을 하지 않지만,

user-event는 '실행한 결과가 ~ 이다' 라고 테스트 하는 도구이다.

<br>

### act

fireEvent로 state의 변화가 있는 경우, act 안에 넣어 주어야한다

- ex) setTimeOut, state 변경 같은 모든것
- AAA패턴

<br>

### jest.useFakingTimers

setTimeOut, setInter.. 과 같은 네이비트 함수들이 Jest 타이머로 교체된다.

<br>

```jsx
// Fake timers using Jest
beforeEach(() => {
  jest.useFakeTimers();
});
afterEach(() => {
  jest.clearAllTimers();
});
```

<br>

### jest.advanceTimerByTime

- 인자에 시간을 넣어 시간만큼 기다린다.
- setTimeOut과 비슷하다.
- `jest.advanceTimerByTime()`

<br>

### jest.clearAllTimer

테스트가 종료된후 모든 타이머가 종료되게 정리한다.

<br>

참고

- [https://haeguri.github.io/2020/01/12/jest-mock-timer/](https://haeguri.github.io/2020/01/12/jest-mock-timer/)
- [https://www.youtube.com/watch?v=3e1GHCA3GP0](https://www.youtube.com/watch?v=3e1GHCA3GP0)
- [https://testing-library.com/docs/queries/about](https://testing-library.com/docs/queries/about)
- [https://github.com/testing-library/jest-dom](https://github.com/testing-library/jest-dom)
- 한 번에 끝내는 프론트엔드 개발 초격차 패키지 Online 패스트 캠퍼스 강의

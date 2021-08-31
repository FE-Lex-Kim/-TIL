# Controlled components

<br>

HTML에서 폼 엘리먼트는 자체적으로 사용자로부터 입력데이터를 받아 내부에서 상태값을 가지고 있다.

따라서 기존 React 엘리먼트와 다르게 동작한다.

<br>

React에서는 Javascript 함수로 폼 제출을 처리한다.

또한 **사용자가 폼에 입력한 데이터에 접근 가능하도록 state에서 관리하여 다른 UI 엘리먼트에 input의 값을 전달하거나 추가적인 로직을 만들 수있는 편리함이 있다.**

<br>

폼 엘리먼트를 이렇게 동작하게 하는것을 '제어 컴포넌트' 라는 기술로 부르고 있다.

<br>

## 제어 컴포넌트

`**<input>, <textarea>, <select>` 와 같은 폼 엘리먼트\*\*는 일반적으로 사용자의 입력을 기반으로 state을 관리하고 업데이트 한다.

<br>

폼을 렌더링 하는 React 컴포넌트는 폼에 발생한 사용자 입력값을 제어한다.

이러한 방식으로 React에 의해 값이 제어되는 입력, 폼 엘리먼트를 '제어 컴포넌트' 라고 부른다.

```jsx
function App(props) {
  let [textValue, setTextValue] = useState("");

  function onChange(e) {
    setTextValue(e.target.value);
  }

  return (
    <>
      <label>
        <input type="text" value={textValue} onChange={onChange} />
      </label>
      <p>{textValue}</p>
    </>
  );
}
```

value 어트리뷰트는 state에 의해 관리되는 폼 엘리먼트에 설정되므로 state 값이 value 값에 들어간다.

<br>

React state를 업데이트하기 위해 모든 키입력에 onChange가 동작하기 때문에 사용자가 입력할때 보여지는 값이 업데이트 된다.

<br>

※ javascript 에서 Change 이벤트와 Input 이벤트가 구별되어있다.

Change 이벤트는 input에서 포커스가 떠날때 발생한다.

input 이벤트는 입력할때마다 이벤트가 발생한다. 하지만 React에서는 Javascript의 input 이벤트를 React에서 onChage로 등록을 해놓았다. 아마도 이슈가 있기때문에 이렇게 해놓았다고 하지만 많은 개발자들이 이러한 부분에 납득을 하지 못한다고 한다. [출처](https://stackoverflow.com/questions/38256332/in-react-whats-the-difference-between-onchange-and-oninput)

<br>

## textarea 태그

```jsx
<textarea>Hello there, this is some text in a text area</textarea>
```

HTML에서 `textarea` 엘리먼트는 텍스트를 자식으로 가져 화면에 보여준다.

<br>

React에서는 `textarea`는 `value` 어트리뷰트를 통해 화면에 보여준다.

초기값을 만들때는 기존 state에 초기화값을 넣어주어야한다.

```jsx
function App(props) {
  let [textValue, setTextValue] = useState(
    "Please write an essay about your favorite DOM element.'"
  );

  function onChange(e) {
    setTextValue(e.target.value);
  }

  return (
    <>
      <label>
        <textarea value={textValue} onChange={onChange} />
      </label>
      <p>{textValue}</p>
    </>
  );
}
```

<br>

## select 태그

HTML에서 `select`는 드롭다운 목록을 만든다.

```jsx
<select>
  <option value="grapefruit">Grapefruit</option>
  <option value="lime">Lime</option>
  <option selected value="coconut">
    Coconut
  </option>
  <option value="mango">Mango</option>
</select>
```

selected 옵션이 있으면 Coconut 부분에 처음에 select 되어질 것이다.

<br>

React 에서는 마찬가지로 최상단의 `select` 태그에 `value` 어트리뷰트를 사용해서 초기값을 결정하고 select 될 엘리먼트를 결정한다.

이러한 장점으로 `selected` 옵션이 `option` 엘리먼트의 각각 붙게 되지만 최상단 `select` 태그에 value 값으로 한곳에서 업데이트되어 사용하기 더 편하다.

```jsx
function App(props) {
  let [selectedValue, setSelectedValue] = useState({ value: "coconut" });

  function onChange(e) {
    setSelectedValue({ value: e.target.value });
  }

  return (
    <>
      <select value={selectedValue.value} onChange={onChange}>
        <option value="grapefruit">Grapefruit</option>
        <option value="lime">Lime</option>
        <option value="coconut">Coconut</option>
        <option value="mango">Mango</option>
      </select>
    </>
  );
}
```

`<input type="text">, <textarea> 및 <select>` 모두 `value` 어트리뷰트를 통해 구현한다.

<br>

## 다중 입력 제어하기

여러 `input` 엘리먼트를 제어하려고 할 때, 각 엘리먼트의 name 어티리뷰트의 값을 state의 키로 저장하고 값으로 초기값을 저장한다.

```jsx
let [state, setState] = useState({
  isGoing: true,
  numberOfGuests: 1,
});
```

<br>

하나의 이벤트 핸들러를 통해 제어할때 input의 type에 따라서 값을 조건문에 따라 결정해서 setState을 해준다.

```jsx
function onChange(e) {
  let type = e.target.type;
  let name = e.target.name;
  let value = type === "checkbox" ? e.target.checked : e.target.value;
  setState({
    ...state,
    [name]: value,
  });
}
```

<br>

```jsx
function App(props) {
  let [state, setState] = useState({
    isGoing: true,
    numberOfGuests: 1,
  });

  function onChange(e) {
    let type = e.target.type;
    let name = e.target.name;
    let value = type === "checkbox" ? e.target.checked : e.target.value;
    setState({
      ...state,
      [name]: value,
    });
  }

  return (
    <>
      <label>
        Is going:
        <input
          name="isGoing"
          type="checkbox"
          checked={state.isGoing}
          onChange={onChange}
        />
      </label>
      <br />
      <label>
        Number of guests:
        <input
          name="numberOfGuests"
          type="number"
          value={state.numberOfGuests}
          onChange={onChange}
        />
      </label>
    </>
  );
}
```

<br>

참고

- [https://ko.reactjs.org/docs/forms.html](https://ko.reactjs.org/docs/forms.html)
- [https://stackoverflow.com/questions/38256332/in-react-whats-the-difference-between-onchange-and-oninput](https://stackoverflow.com/questions/38256332/in-react-whats-the-difference-between-onchange-and-oninput)
- [https://developer.mozilla.org/ko/docs/Learn/Forms](https://developer.mozilla.org/ko/docs/Learn/Forms)
- [https://ko.javascript.info/events-change-input](https://ko.javascript.info/events-change-input)

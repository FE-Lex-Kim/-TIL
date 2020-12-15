# 이벤트

<br>

사용자가 웹브라우저에서 동적으로 상호작용 하는것을 이벤트라고한다.

자바스크립트에서 인라인으로 이벤트를 처리할때랑 비슷하다.

```jsx
<button onClick='alert('hello')'>확인</button>
```

<br>

1. 리액트에서 이벤트 이름은 카멜케이스로 작성해야한다 ex) `onClick`
2. 이벤트안에 함수를 전달한다. 또는 외부에서 만든 함수를 전달해도된다.
3. DOM요소에서만 이벤트를 설정할수있다. 직접만든 컴포는트에는 이벤트를 설정할수없다.

<br>

**이벤트 종류**

[React 공식문서 이벤트](https://ko.reactjs.org/docs/events.html)

<br>

## 클래스형 컴포넌트 이벤트 처리

<br>

```jsx
import React, { Component } from 'react';

class EventPractice extends Component {
  render() {
    return (
      <div>
        <input
          type="text"
          placeholder="입력해보세요!"
          onChange={(e) => {
            console.log(e.target.value);
          }}
        />
      </div>
    );
  }
}

export default EventPractice;
```

<br>

### 이벤트 발생으로 state 값 변경하기

```jsx
import React, { Component } from 'react';

class EventPractice extends Component {
  state = {
    message: '',
  };
  render() {
    return (
      <div>
        <input
          type="text"
          placeholder="입력해보세요!"
          onChange={(e) => {
            console.log(e.target.value);
            this.setState({ message: e.target.value });
          }}
        />
        <button
          onClick={(e) => {
            alert(this.state.message);
          }}
        >
          확인
        </button>
      </div>
    );
  }
}

export default EventPractice;
```

<br>

### 이벤트에 외부에서 정의한 함수를 전달

<br>

this는 호출한 대상에 따라 결정된다.

이때 클래스에서 메서드로써 함수를 정의했다.

<br>

하지만 HTML요소의 이벤트로 등록되었을때 함수를 호출한 대상이 

<br>

브라우저에서 이벤트로 호출되어 

클래스의 인스턴스가 아니라 브라우저이므로 Undefiend가 된다.

<br>

때문에 직접적으로 bind를 하여 클래스의 인스턴스를 직접적으로 가리키게 바인딩해주어야한다.

```jsx
import React, { Component } from 'react';

class EventPractice extends Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.handleClick = this.handleClick.bind(this);
  }
  state = {
    message: '',
  };
  handleChange(e) {
    console.log(e.target.value);
    this.setState({ message: e.target.value });
  }
  handleClick(e) {
    alert(this.state.message);
  }
  render() {
    return (
      <div>
        <input
          type="text"
          placeholder="입력해보세요!"
          onChange={this.handleChange}
        />
        <button onClick={this.handleClick}>확인</button>
      </div>
    );
  }
}

export default EventPractice;
```

<br>

이렇게 바인딩을 하는작업에서 consturctor도 수정해야하고 작업하는데 있어서 불편하다.

<br>

bind함수로 this를 바인딩안하고 

사용할수있게 화살표함수로 메서드를 정의하면된다.

바벨이 transform-class-properties를 사용해서 정의해준다.

```jsx
import React, { Component } from 'react';

class EventPractice extends Component {
  state = {
    message: '',
  };
  handleChange = (e) => {
    console.log(e.target.value);
    this.setState({ message: e.target.value });
  };
  handleClick = (e) => {
    alert(this.state.message);
  };
  render() {
    return (
      <div>
        <input
          type="text"
          placeholder="입력해보세요!"
          onChange={this.handleChange}
        />
        <button onClick={this.handleClick}>확인</button>
      </div>
    );
  }
}

export default EventPractice;
```

<br>

### 여러개의 중복된 함수를 이벤트에 적용할때

<br>

여러개의 함수를 재활용하여 이벤트에 적용하려고 한다.

<br>

이때 e.target.name을 활용하여 이벤트에 등록할 함수에서 setState를 다르게해주면된다.

name과 state의 키의 값을 동일하게 하여 등록하는 state를 HTML의 대상별로 다르게 적용된다.

```jsx
import React, { Component } from 'react';

class EventPractice extends Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.handleClick = this.handleClick.bind(this);
  }
  state = {
    message: '',
    username: '',
  };
  handleChange(e) {
    this.setState({ [e.target.name]: e.target.value });
  }
  handleClick() {
    alert(this.state.message + ' ' + this.state.username);
  }
  render() {
    return (
      <div>
        <input
          type="text"
          name="message"
          placeholder="입력해보세요!"
          onChange={this.handleChange}
        />
        <input
          type="text"
          name="username"
          placeholder="입력해보세요!"
          onChange={this.handleChange}
        />
        <button onClick={this.handleClick}>확인</button>
      </div>
    );
  }
}

export default EventPractice;
```

<br>

## 함수형 컴포넌트 이벤트 처리

<br>

useState() 함수의 인수값으로 객체가 들어가도된다.

state의 값을 수정할떄는

객체 스프레드 문법을 사용하면된다.

```jsx
import React, { useState } from 'react';

const EventPractice = () => {
  const [form, setForm] = useState({
    username: '',
    message: '',
  });

  const handleChange = (e) => {
    setForm({
      ...form,
      [e.target.name]: e.target.value,
    });
  };

  const emtyState = (e) => {
    alert(`${form.username} ${form.message}`);
    setForm({ username: '', message: '' });
  };

  const pressKEY = (e) => {
    if (e.key !== 'Enter') return;
    emtyState();
  };

  return (
    <div>
      <h1>이벤트 연습</h1>
      <input
        type="text"
        name="username"
        placeholder="이름을 적어주세요"
        value={form.username}
        onChange={handleChange}
      />
      <input
        type="text"
        name="message"
        placeholder="메세지를 적어주세요"
        value={form.message}
        onChange={handleChange}
        onKeyPress={pressKEY}
      />
      <button onClick={emtyState}>확인</button>
    </div>
  );
};

export default EventPractice;
```
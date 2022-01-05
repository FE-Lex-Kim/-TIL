- [Cypress - 사용하기](#cypress---사용하기)
  - [Writing tests](#writing-tests)
    - [예제 파일 설치](#예제-파일-설치)
    - [Test 파일 생성](#test-파일-생성)
    - [Test 코드 작성](#test-코드-작성)
    - [visit()](#visit)
    - [type()](#type)
  - [다양한 Test 메서드](#다양한-test-메서드)
    - [Test 코드 데이터 요청 방법](#test-코드-데이터-요청-방법)
  - [예제) TodoList 테스트 코드](#예제-todolist-테스트-코드)
  - [테스트 코드 작성 팁](#테스트-코드-작성-팁)
    - [테스트 코드](#테스트-코드)
    - [Element 선택](#element-선택)
    - [잘못된 DOM 탐색](#잘못된-dom-탐색)
    - [올바른 DOM 탐색](#올바른-dom-탐색)

<br>

# Cypress - 사용하기

설치

```bash
npm i cypress -D
```

<br>

## Writing tests

### 예제 파일 설치

cypress를 설치한 후, `**./node_modules/.bin` 폴더에 cypress가 설치된다.\*\*

아래와 같은 커맨드를 실행하면, **예제 테스트와 예제 폴더**들이 설치된다.

```bash
node_modules/.bin/cypress open
```

또는

```bash
npx cypress open
```

<br>

Cypress가 열리게 된다.

**이후 우상단에 브라우저 변경을 해서 테스트 할 수 있다.**

![Cypress(1)](<../Images/Cypress(1)/Cypress(1)-1.png>)

<br>

매번 Cypress를 실행시키기 위해 긴 path를 입력하는 것은 매우 불편하다.

`**package.json` script를 통해 쉽게 실행시키는 방법이 있다.\*\*

<br>

package.json

```json
{
  "scripts": {
    "cypress": "cypress open"
  }
}
```

<br>

위의 방법은 **cypress 예제 파일을 설치한 다음**에 실행하는 방법이다.

이번에는 직접 Test 코드를 작성하는 방법에 대해 알아보자.

<br>

### Test 파일 생성

1. 먼저 **cypress/integration 폴더를 root에서 생성한다.**
2. 이후 **Test 코드 파일(sample.js)을 만들면 된다.**
3. sample.js 파일의 **코드 최상단에 주석으로 아래의 코드**를 꼭 써주어야한다.

<br>

**sample.js**

```jsx
/// <reference types="cypress" />
```

<br>

파일을 생성한후, Cypress Test Runner은 sample.js파일이 **변경이 있는지 감시하고 자동으로 그 변경에 대해 보여준다.**

![Cypress(1)](<../Images/Cypress(1)/Cypress(1)-2.png>)

<br>
### Test 코드 작성

Cypress는 Mochca가 빌트인 되어있다.

Mocha 문법을 바탕으로 테스트 코드를 작성하면 된다.

- **하지만 도입부의 문법은 `cy`로 시작한다.**
- `cy` 객체안에 Cypress의 test 할 메서드들이 모두 포함되어있다.

Cypress에 있는 **[문법은 공식 홈페이지](https://docs.cypress.io/api/table-of-contents) 또는 [예제를 포함한 공식 홈페이지](https://example.cypress.io/)** 에서 보는것을 추천한다.

<br>

**테스트 코드를 작성하고 파일을 저장하면, Cypress 브라우저에서는 자동으로 실시간으로 적용되어진다.**

### visit()

sample.js

```jsx
describe("My First Test", () => {
  it("Visits the google", () => {
    cy.visit("https://google.com");
  });
});
```

`cy.visit(url)`은 **테스트 하려는 페이지를 정하고 방문하는 메서드이다.**

<br>

`**cy.visit(url)` 은 baseUrl 이 정의가 되어있지 않으면, 자동으로 Test 파일을 Test URL로 실행한다.\*\*

- baseUrl 은 `cypress.json` 파일에서 정의할 수 있다.

```jsx
{
  "baseUrl": "http://localhost:3000"
}
// 이제 '/' 을 기준으로 api 주소 작성이 가능하다.
```

<br>

아래의 코드에서

1. 첫 번째 테스트 코드는 `/` 인 **baseUrl을 방문하고,**
2. 두 번째 테스트 코드는 **baseUrl을 기준으로 경로를 판단해 방문한다.**

```jsx
it("visits base url", () => {
  cy.visit("/");
  cy.contains("h1", "Kitchen Sink");
});
it("visits file", () => {
  cy.visit("app/index.html");
  cy.contains("local file");
});
```

<br>

Cypress에서 표적지 모양을 클릭하면, 마우스 커서로 **어떤 엘리멘트를 얻을 것인지 알 수 있다.**

![Cypress](<../Images/Cypress(1)/Cypress(1)-3.png>)

<br>

### type()

구글 Input이 Element class가 `**.gLFyf` 이라는 것을 알 수 있다.\*\*

```jsx
/// <reference types="cypress" />

describe("My First Test", () => {
  it("Visits the google", () => {
    cy.visit("https://google.com");
    cy.get(".gLFyf").type("Lex wants to be best Front-end developer."{enter});
		// get 메서드로 Element를 얻는다.
		// type 메서드로 Element에 기입한다.
  });
});
```

- `type` 메서드 인수에 특수한 `{}` 문법으로 **이벤트를 실행시킬 수 있다.**
- 예를들어 `{enter}`, `{backspace}`, `{downarrow}` .. 등등
- 더 많은 정보는 **[type 챕터 공식홈페이지](https://docs.cypress.io/api/commands/type#Arguments)**를 보면 알 수 있다.

<br>

## 다양한 Test 메서드

- `cy.contains(’hello’)` : `‘hello’` 라는 text를 가진 Element를 선택한다.
- `cy.get(’abc’).find(’li’)` : `‘abc’`의 자식 요소들중 `li` 을 찾아 선택한다.
- `click()` : Element를 클릭한다.
- `cy.wait(5000)` : 5000ms 동안 잠시 멈춘다.
- `cy.get('abc').should(’contain’, 'Lex Kim')` : `abc` Element가 `Lex Kim` 이라는 **Text를 반드시 가지고 있는지 확인한다.**
- `cy.get(’abc’).should(’have.class’ , 'dfg')` : `abc` Elemen가 `dfg` 라는 class를 **가지고 있는지 확인한다.**
- `cy.get(’abc’).should(’contain’ , 'Lex Kim').and(’have.class’, ‘dfg)` : Lex Kim Text와 ‘dfg’ 클래스를 가지고 있는지 확인한다.

<br>

`expect`를 사용해서 익숙한 방식으로 테스트할 수 있다.

```jsx
cy.get("tbody tr:first").should(($tr) => {
  expect($tr).to.have.class("active");
  expect($tr).to.have.attr("href", "/users");
});
```

<br>

### Test 코드 데이터 요청 방법

```jsx
cy.request("POST", "/test/seed/post", {
  title: "First Post",
  authorId: 1,
  body: "...",
});
```

<br>

## 예제) TodoList 테스트 코드

TodoList.js

```jsx
const TodoList = () => {
  const [list, setList] = useState([
    "Cypress 공부하기",
    "React Query 공부하기",
    "Webpack 공부하기",
  ]);
  const [value, setValue] = useState("");

  function onChange(e) {
    setValue(e.target.value);
  }

  function onEnterAdd(e) {
    if (value === "") return;
    if (e.key === "Enter") {
      setList([...list, value]);
      setValue("");
    }
  }
  function onClickAdd() {
    if (value === "") return;
    setList([...list, value]);
    setValue("");
  }

  function onDelete(e) {
    const id = e.target.parentNode.dataset.id;
    const last = list.filter((todo, i) => i !== +id);
    setList(last);
  }

  return (
    <Container>
      <H1>Todo List</H1>
      <InputContainer>
        <Input
          data-cy="input"
          value={value}
          onChange={onChange}
          onKeyDown={onEnterAdd}
        />
        <Button onClick={onClickAdd}>추가</Button>
      </InputContainer>
      <CheckBoxContainer data-cy="list">
        {list.map((todo, i) => (
          <div key={i} data-id={i}>
            <Checkbox type="checkbox" />
            <List>{todo}</List>
            <Delete onClick={onDelete}>X</Delete>
          </div>
        ))}
      </CheckBoxContainer>
    </Container>
  );
};

export default TodoList;
```

<br>

todo.spec.js

```jsx
/* eslint-disable no-undef */
/// <reference types="cypress" />

describe("Testing TodoList", () => {
  beforeEach(() => {
    cy.visit("/");
  });
  it("add a Todo", () => {
    // 초기값
    function dataCy(data) {
      return cy.get(`[data-cy=${data}]`); // data-cy 함수 생성
    }

    const input = dataCy("input");

    // 테스트
    // 1. input value 입력 테스트
    input
      .type("Cypress 기본개념 공부")
      .should("have.value", "Cypress 기본개념 공부"); // input value를 확인

    // 2. TodoList 생성 테스트
    input.type("{enter}"); // enter 입력

    const list = dataCy("list");
    list
      .children()
      .last() // 마지막 요소 찾기
      .find("p") // 자식 요소중 p 태그인 요소 찾기
      .should("have.text", "Cypress 기본개념 공부"); // 요소가 text을 가지고 있는지 확인
  });
});
```

![Cypress](<../Images/Cypress(1)/Cypress(1)-4.png>)

## 테스트 코드 작성 팁

### 테스트 코드

**테스트 코드를 작성할때, 어떤식으로 작성할지 정하면 좋다.**

```jsx
test('should $1', () => {
  // Given
  const data = $4

  // When
  const result = $3

  // Then
  expect(result).toEqual($2)
})

참고 : https://jbee.io/react/testing-2-react-testing/
```

1. 테스트 코드에 대한 **목적(test)**을 작성한다.
2. 최종적으로 **예상하는 값(result)**을 작성한다.
3. 예상하는 값을 어떠한 **조건(expect)**으로 검증할지 작성한다.
4. expect를 하기위한 그 이전의 가정을 작성한다.

<br>

- **data는 playload, mocking state** 등등 이 될 수 있다.
- **result**는 data를 기반으로 **어떠한 행동을 취해서 검증해야 하는 값이 온다.**
- 대부분 **`toEqual`로 작성하지만 `tobe`, `tobeFalsy`.. 등등이 될 수 있다.**

<br>

### Element 선택

<br>

### 잘못된 DOM 탐색

**시각적 테스트를 위한 Class와 CSS를 위한 Class를 구분해야한다.**

→ **DOM 탐색을 할때,** Class 이름이 **시각적 표현을 위한 클래스 명을 사용하면 안된다.**

→ **시각적인 요소를 변경하기 위해 수정했을때, 테스트 코드도 오류가 생긴다.**

![Cypress](https://github.com/FE-Lex-Kim/-TIL/raw/master/Images/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C%20TDD/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C%20TDD-2.png)

<br>

### 올바른 DOM 탐색

→ **기능만을 위한 의미있는 클래스명을 사용해야한다.**

→ 테스트를 위한 **별도의 속성값 사용한다.**

![Cypress](https://github.com/FE-Lex-Kim/-TIL/raw/master/Images/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C%20TDD/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C%20TDD-3.png)

→ `getByTestID`를 사용해야한다.

<br>

![https://github.com/FE-Lex-Kim/-TIL/raw/master/Images/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C%20TDD/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C%20TDD-4.png](https://github.com/FE-Lex-Kim/-TIL/raw/master/Images/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C%20TDD/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C%20TDD-4.png)

시각적 요소와 상관없이 **테스트 요소를 항상 변함없이 선택가능하다.**

<br>

`**data-cy` 형식으로 attribute를 정해서 Element를 선택하는게 좋다.\*\*

```jsx
<button class="btn btn-large" data-cy="submit">
  Submit
</button>
```

<br>

```jsx
function dataCy(data) {
  return cy.get(`[data-cy=${data}]`);
}

const input = dataCy("input");
const list = dataCy("list");
```

<br>

배포시 data-cy를 제거하는게 성능에 좋을 수도 있다.

아래 플러그인을 사용하면된다.

```bash
npm install --save-dev babel-plugin-react-remove-properties
```

```json
{
  "env": {
    "production": {
      "plugins": [
        [
          "react-remove-properties",
          {
            "properties": ["data-cy"]
          }
        ]
      ]
    }
  }
}
```

<br>

참고

- [https://docs.cypress.io/guides/overview/why-cypress](https://docs.cypress.io/guides/overview/why-cypress)
- [https://jbee.io/react/testing-2-react-testing/](https://jbee.io/react/testing-2-react-testing/)
- [https://velog.io/@averycode/Cypress](https://velog.io/@averycode/Cypress)
- [https://soobing.github.io/dev/cypress-with-react/](https://soobing.github.io/dev/cypress-with-react/)
- [https://www.js2uix.com/frontend/cypress-study-step1/](https://www.js2uix.com/frontend/cypress-study-step1/)
- [https://www.youtube.com/watch?v=LcGHiFnBh3Y&t=1872s](https://www.youtube.com/watch?v=LcGHiFnBh3Y&t=1872s)
- [https://www.youtube.com/watch?v=CYcdT-tOvB0&list=PLhW3qG5bs-L9LTfxZ5LEBiM1WFfvX3dJo](https://www.youtube.com/watch?v=CYcdT-tOvB0&list=PLhW3qG5bs-L9LTfxZ5LEBiM1WFfvX3dJo)

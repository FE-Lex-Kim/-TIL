- [Cypress(1) - Cypress 시작하기](#cypress1---cypress-시작하기)
  - [Writing tests](#writing-tests)
    - [예제 파일 설치](#예제-파일-설치)
    - [Test 파일 생성](#test-파일-생성)

<br>

# Cypress(1) - Cypress 시작하기

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

참고

- [https://docs.cypress.io/guides/overview/why-cypress](https://docs.cypress.io/guides/overview/why-cypress)
- [https://jbee.io/react/testing-2-react-testing/](https://jbee.io/react/testing-2-react-testing/)
- [https://velog.io/@averycode/Cypress](https://velog.io/@averycode/Cypress)
- [https://soobing.github.io/dev/cypress-with-react/](https://soobing.github.io/dev/cypress-with-react/)
- [https://www.js2uix.com/frontend/cypress-study-step1/](https://www.js2uix.com/frontend/cypress-study-step1/)
- [https://www.youtube.com/watch?v=LcGHiFnBh3Y&t=1872s](https://www.youtube.com/watch?v=LcGHiFnBh3Y&t=1872s)
- [https://www.youtube.com/watch?v=CYcdT-tOvB0&list=PLhW3qG5bs-L9LTfxZ5LEBiM1WFfvX3dJo](https://www.youtube.com/watch?v=CYcdT-tOvB0&list=PLhW3qG5bs-L9LTfxZ5LEBiM1WFfvX3dJo)

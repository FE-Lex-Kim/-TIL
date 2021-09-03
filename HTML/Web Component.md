# 웹 컴포넌트

<br>

캡슐화 하여 재사용 가능한 커스텀 컴포넌트를 생성하고 웹 앱에서 활용할 수 있도록 해주는 다양한 기술들의 모음이다.

<br>

즉, 웹 컴포넌트란 웹 페이지 혹은 웹 앱에서 캡슐화된 HTML 태그를 만들 수 있는 기능이다.

<br>

웹 컴포넌트는 세가지 주요 기술들로 구성되며, 재사용이 원하는 어느곳이든 코드 충돌에 대한 걱정이 없는 캡슐화된 기능을 제공한다.

1. 커스텀 엘리먼트 : 컴포넌트를 새로운 명칭으로 만들어서 이것을 HTML 요소와 함께 사용한다.
2. 쉐도우 DOM : 컴포넌트 끼리는 스코프를 분리하여 DOM을 캡슐화 한다.
3. HTML 템플릿 : 컴포는트의 뷰를 생성한다.

<br>

## 구현 방법

<br>

### 컴포넌트 생성

ES6 클래스 문법을 사용해 웹 컴포넌트 기능을 명시하는 클래스를 생성한다.

```jsx
class UserCard extends HTMLElement {
  constructor() {
    super();
    this.innerHTML;
  }
}

console.log(new UserCard());
```

<br>

클래스가 `HTMLElement` 를 상속을 받는다면 해당 클래스는 HTML 요소 그 자체가 된다.

![스크린샷 2021-09-02 오후 8.24.47.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ecf98ccc-07fb-4865-b722-a81760755f8f/스크린샷_2021-09-02_오후_8.24.47.png)

<br>

`CustomElements.define()` 메소드를 사용해서 컴포넌트 클래스, 정의할 엘리먼트 이름을 인자로 넣어준다.

### 어트리뷰트 전달받기

React와 같이 props(어트리뷰트)를 전달 받는다면 `this.getAttribute()` 실행하고 안의 인자에 해당 props(어트리뷰트) 이름을 그대로 넣어준다.

```jsx
class UserCard extends HTMLElement {
  constructor() {
    super();
    this.innerHTML = `<h3>${this.getAttribute("name")}</h3> `;
  }
}
```

<br>

### Css 적용 주의사항

커스텀 엘리먼트에 내부에서 h3 엘리먼트에 대한 스타일을 넣는다면, css 스타일은 전역으로 공유하고 있다.

따라서 해당 커스텀 엘리먼트의 h3만 css 스타일을 가지는것이 아니라 HTML의 모든 h3 요소들이 스타일을 받는다.

예시코드를 보자.

<br>

```jsx
// index.html
<body>
  <h3 class="bt">Hello World</h3>
  <user-card name="Alex"></user-card>
  <script type="module" src="./UserCard.js"></script>
</body>
```

```jsx
// UserCard.js
class UserCard extends HTMLElement {
  constructor() {
    super();
    this.innerHTML = `
    <style>
        h3{
            color : coral
          }
    </style>
    <h3>
        ${this.getAttribute("name")}
    </h3> `;
  }
}

customElements.define("user-card", UserCard);
```

![스크린샷 2021-09-02 오후 9.00.58.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fbc4cb16-339f-4b17-ba40-bc0313f02057/스크린샷_2021-09-02_오후_9.00.58.png)

모든 h3들이 공통적으로 css를 적용받는다.

### shadow DOM

따라서 해당 컴포넌트가 하나의 모듈처럼 사용하고 다른 스코프와 공유하지 않으려면 shadow DOM을 사용하면된다.

<br>

`attachShadow({mode: 'open'})` 를 실행하면 쉐도우 루트를 생성한다.

![스크린샷 2021-09-02 오후 9.51.08.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5568f967-e6c6-459e-a2cd-ea7fabe363c6/스크린샷_2021-09-02_오후_9.51.08.png)

인자의 mode의 `open` 과 `closed` 이렇게 두가지가 있다.

<br>

1. `open` 은 자바스크립트로 shadow DOM을 직접적으로 접근할 수 있다. `Element.shadowRoot` 를 통해서 shadow root에 접근가능하다.
   - `let myShadowDom = myCustomElem.shadowRoot;`
     - 위와 같이 접근하면 곧바로 `shadow-root`부터 접근이 가능하다.
   - `let shadow = this.attachShadow({ mode: "open" });` 도 마찬가지로 접근 가능하다. 지금 같은 코드는 생성과 동시에 접근 가능하도록 변수를 만들기에 코드가 짧아진다.
2. `close`는 반대로 shadow DOM을 접근 할수 없다는 의미이다.

<br>

이것은 DOM 스코프의 역활을한다.

![스크린샷 2021-09-02 오후 9.10.12.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/63d37ca8-64c8-4536-9b51-3bc6d839f6b9/스크린샷_2021-09-02_오후_9.10.12.png)

출처: [https://developer.mozilla.org/ko/docs/Web/Web_Components/Using_shadow_DOM](https://developer.mozilla.org/ko/docs/Web/Web_Components/Using_shadow_DOM)

<br>

Shadow DOM 은 숨겨진 DOM 트리를 실제 DOM 트리에 붙여넣어준다.

Shadow DOM tree 는 shadow root로 부터 시작한다.

그 아래부터는 어떠한 엘리먼트든 일반 DOM처럼 붙여서 사용할 수 있다.

<br>

위의 그림에 대한 용어에대해 정리해보자

**Shadow host** : shadow DOM이 연결된 일반 DOM 노드이다.

**Shadow tree** : shadow DOM 안의 DOM tree 이다.

**Shadow boudnary** : shadow DOM이 끝나고 일반 DOM이 사작되는 장소이다.

**Shadow root** : shadow tree의 루트 노드이다.

<br>

shadow dom 내부에서 style의 어떠한 코드도 외부에 영향이 가지 않는다. 즉, 하나의 모듈, 스코프 처럼 동작한다.

<br>

### shadow DOM에 엘리먼트들 추가

이렇게 만든 shadow DOM 내부에 엘리먼트 들을 만들어 넣어보자.

<br>

this.innerHTML 넣게 되면 shadow root 의 형제요소로 들어가게 된다.

따라서 shadow DOM에 appendChid 메서드를 통해 자식요소로 삽입하면된다.

<br>

이떄 `<template>` 요소를 사용하면 된다.

template 요소는 페이지를 불러온 즉시 브라우저에 렌더링 되지 않지만, Javascript 파일에서 클래스 인스턴스를 생성해서 HTML 코드를 담을 방법을 제공한다.

※ [더 자세한 template 내용](https://developer.mozilla.org/ko/docs/Web/HTML/Element/template)

```jsx
**let template = document.createElement("template");

template.innerHTML = `
<style>
    h3{
        color : coral;
    }
</style>
<div class='user-card'>
    <h3></h3>
</div>
`;**

// UserCard.js
class UserCard extends HTMLElement {
  constructor() {
    super();
    let shadow = this.attachShadow({ mode: "open" });

    let imgSrc = this.getAttribute("scenery");
    let city = this.getAttribute("city");

    **shadow.appendChild(template.content.cloneNode(true));**
    shadow.querySelector("h3").innerHTML = city;
    shadow.querySelector("img").src = imgSrc;
  }
}

customElements.define("user-card", UserCard);
```

<br>

template의 내부의 값인 content프러퍼티를 통해 가져와서 `cloneNode(true)` 를 통해 내부의 노드까지 모두 깊은복사해 올 수 있다.

<br>

### `<slot>` 태그로 html 전달받기

index.html

```jsx
<user-card city="Vancouver" scenery="./src/img/Vancovuer.jpg">
   **<div slot="nation">Nation: Canada</div>
   <div slot="weather">Weather: rainy</div>**
</user-card>
<user-card city="Laos" scenery="./src/img/laos.jpg"></user-card>
```

위와 같이 user-card 컴포넌트 내부의 자식노드인 div들을 전달 받을수있다.

이때 slot 속성으로 이름을 정해주고, 해당 이름을 컴포넌트내부에서 `slot` 태그의 name으로 전달받은 slot을 가진 엘리먼트중에 골라서 사용가능하다.

<br>

UserCard.js

```jsx
let template = document.createElement("template");

template.innerHTML = /* html */ `
<style>
    h3{
        color : coral;
    }
    img {
      width : 300px;
    }
</style>
<div class='user-card'>
    <div>
      <h3></h3>
      <img/>
      <div class='info'>
        **<p>
          <slot name='nation'></slot>
        </p>
        <p>
          <slot name='weather'></slot>
        </p>**
      </div>
    </div>
    <button id='toggle-info'>hide info</button>
</div>
`;

// UserCard.js
class UserCard extends HTMLElement {
  constructor() {
    super();
    let shadow = this.attachShadow({ mode: "open" });

    let imgSrc = this.getAttribute("scenery");
    let city = this.getAttribute("city");

    shadow.appendChild(template.content.cloneNode(true));
    shadow.querySelector("h3").innerHTML = city;
    shadow.querySelector("img").src = imgSrc;
  }
}

customElements.define("user-card", UserCard);
```

<br>

## 라이플 사이클

신기하게도 React처럼 WebComponent도 라이플 사이클이 있다.

- `connectedCallback`: 커스텀 엘리먼트가 처음으로 실제 DOM 트리에 연결되었을 때 호출된다.
  - 이벤트, 렌더링 등의 처리를 이곳해서 하면된다.
- `disconnectedCallback`: 커스텀 엘리먼트가 실제 DOM 트리에으로부터 연결 해제되었을 때 호출된다.
  - 이벤트 해제와 같이 sideEffect를 제거하는데 사용하면된다.
- `adoptedCallback(oldDoc, newDoc)`: 커스텀 엘리먼트가 새로운 다큐먼트로 이동되었을 때 호출된다.
  - 다른 Document에서 옮겨지면 호출된다.
- `attributeChangedCallback(attrName, oldVal, newVal`): 커스텀 엘리먼트의 어트리뷰트가 추가, 제거 또는 변경되었을 때 호출된다.

<br>

### connectedCallback / disconnectedCallback

HTMLElement를 상속받은 Custom Elements의 `constructor` 실행시점은 아직 DOM에 추가되지 않은 상태이다.

따라서 `constructor`에서 어떠한 DOM 조작도 할 수 없다.

<br>

DOM 조작을 하기위해서 shadow DOM이 실제 DOM트리에 삽입된 이후 호출되는 `connectedCallback` 라이프 사이클 메서드를 제공한다.

<br>

`connectedCallback / disconnectedCallback` 의 실행시점은 custom Element가 DOM에 추가되거나 삭제되었을때 실행된다.

<br>

이때마다 인스턴스 객체가 생성 또는 파괴가 되는것이 아니므로 DOM을 수정을 이곳에서 얼마든지 해도 된다.

<br>

주의사항은 `connectedCallback` 이 실행되었더라도 엘리먼트가 DOM에 추가되어있긴 하지만 자식 엘리먼트는 아직 DOM에 추가되지 않은 상태여서 자식 엘리먼트에 접근이 불가능하다.

```jsx
class CurrentTime extends HTMLElement {
    ...
    connectedCallback() {
        // 이 엘리먼트는 DOM에 추가되었다.
        console.log(this.parentNode); // ok "<body></body>"
        console.log(this.firstChild); // null <--- 아직 자식 엘리먼트에 접근할 수는 없다.
        console.log(this.innerHTML); // "" <--- 아직 자식 엘리먼트에 접근할 수는 없다.
        console.log(this.getAttribute('locale')); // ok "ko=KR"
        this.setAttribute('locale', 'en-US'); // ok
        this.innerText = 'Arr'; // ok
    }
    ...
    disconnectedCallback() {
        // 이 엘리먼트가 DOM에서 제거되었다.
        // connectedElement에서 수행한 셋업을 청소하는 일을 하자
    }
}

출처 : https://ui.toast.com/weekly-pick/ko_20170609
```

<br>

완성코드

```jsx
let template = document.createElement("template");

template.innerHTML = /* html */ `
<style>
    h3{
        color : coral;
    }
    img {
      width : 300px;
    }
</style>
<div class='user-card'>
    <div>
      <h3></h3>
      <img/>
      <div class='info'>
        <p>
          <slot name='nation'></slot>
        </p>
        <p>
          <slot name='weather'></slot>
        </p>
      </div>
    </div>
    <button id='toggle-info'>hide info</button>
</div>
`;

// UserCard.js
class UserCard extends HTMLElement {
  constructor() {
    super();
    let shadow = this.attachShadow({ mode: "open" });

    let imgSrc = this.getAttribute("scenery");
    let city = this.getAttribute("city");

    this.showinfo = true;

    shadow.appendChild(template.content.cloneNode(true));
    shadow.querySelector("h3").innerHTML = city;
    shadow.querySelector("img").src = imgSrc;
  }

  toggleInfo() {
    let $info = this.shadowRoot.querySelector(".info");
    this.showinfo = !this.showinfo;
    if (this.showinfo) {
      $info.style.display = "block";
    } else {
      $info.style.display = "none";
    }
  }

  connectedCallback() {
    this.shadowRoot
      .querySelector("#toggle-info")
      .addEventListener("click", (e) => {
        this.toggleInfo();
      });
  }

  disconnectedCallback() {
    this.shadowRoot.closest("#toggle-info").removeEventListener();
  }
}

customElements.define("user-card", UserCard);
```

```jsx
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style></style>
  </head>
  <body>
    <style>
      h3 {
        color: indigo;
      }
    </style>
    <h3 class="bt">Hello World</h3>
    <user-card city="Vancouver" scenery="./src/img/Vancovuer.jpg">
      <div slot="nation">Nation: Canada</div>
      <div slot="weather">Weather: Rainy</div>
    </user-card>
    <user-card city="Laos" scenery="./src/img/laos.jpg">
      <div slot="nation">Nation: Laos</div>
      <div slot="weather">Weather: Sunny</div>
    </user-card>
    <script type="module" src="./UserCard.js"></script>
  </body>
</html>
```

참고

- [https://ko.reactjs.org/docs/web-components.html](https://ko.reactjs.org/docs/web-components.html)
- [https://ggodong.tistory.com/279](https://ggodong.tistory.com/279)
- [https://developer.mozilla.org/ko/docs/Web/Web_Components](https://developer.mozilla.org/ko/docs/Web/Web_Components)
- [https://ui.toast.com/weekly-pick/ko_20170428](https://ui.toast.com/weekly-pick/ko_20171215)
- [https://developer.mozilla.org/ko/docs/Web/Web_Components/Using_shadow_DOM](https://developer.mozilla.org/ko/docs/Web/Web_Components/Using_shadow_DOM)
- [https://developer.mozilla.org/ko/docs/Web/HTML/Element/template](https://developer.mozilla.org/ko/docs/Web/HTML/Element/template)

# Single Page Application & Routing

<br>

## 1. SPA (Single Page Application)

단일 페이지 애플리케이션(Single Page Application, SPA)는 모던 웹의 패러다임이다.

SPA는 기본적으로 단일 페이지로 구성된다.

<br>

**전통적인 웹 방식 단점**

link tag를 사용하는 전통적인 웹 방식은 새로운 페이지 요청 시마다 정적 리소스가 다운로드된다.

또한 전체 페이지를 다시 렌더링하는 방식을 사용하므로 새로고침이 발생되어 사용성이 좋지 않다.

변경이 필요없는 부분를 포함하여 전체 페이지를 갱신하므로 비효율적이다.

<br>

**그렇다면? SPA의 장점**

SPA는 기본적으로 웹 애플리케이션에 필요한 모든 정적 리소스를 최초에 한번 다운로드한다.

<br>

이후 새로운 페이지 요청 시, 

페이지 갱신에 필요한 데이터만을 전달받아 페이지를 갱신하므로 **전체적인 트래픽을 감소** 할 수 있고, (1)

전체 페이지를 다시 렌더링하지 않고 변경되는 부분만을 갱신하므로 **새로고침이 발생하지 않는다.** (2)

<br>

SPA의 핵심 가치는 **사용자 경험(UX) 향상** 에 있으며 부가적으로 애플리케이션 속도의 향상도 기대할 수 있어서 모바일 퍼스트(Mobile First) 전략에 부합한다.

<br>

**그렇다면 SPA 단점?**

<br>

**초기 구동 속도**

SPA는 웹 애플리케이션에 필요한 모든 정적 리소스를 최초에 한번 다운로드하기 때문에 **초기 구동 속도가 상대적으로 느리다.** 

<br>

하지만 SPA는 웹페이지보다는 애플리케이션에 적합한 기술이므로 트래픽의 감소와 속도, 사용성, 반응성의 향상 등의 장점을 생각한다면 결정적인 단점이라고 할 수는 없다.

<br>

**SEO(검색엔진 최적화) 문제**

SPA는 서버 렌더링 방식이 아닌 자바스크립트 기반 비동기 모델(클라이언트 렌더링 방식)이다. 

<br>

따라서 SEO는 언제나 단점으로 부각되어 왔던 이슈이다. 

하지만 SPA는 정보의 제공을 위한 웹페이지보다는 애플리케이션에 적합한 기술이므로 SEO 이슈는 심각한 문제로 볼 수 없다. 

<br>

Angular 또는 React 등의 SPA 프레임워크는 서버 렌더링을 지원하는 SEO 대응 기술이 이미 존재하고 있어 SEO 대응이 필요한 페이지에 대해서는 선별적 SEO 대응이 가능하다.

<br>

## 2. Routing

<br>

라우팅이란 출발지에서 목적지까지의 경로를 결정하는 기능이다.

<br>

일반적으로 사용자가 요청한 URL 또는 이벤트를 해석하고 **새로운 페이지로 전환하기 위한 데이터를 취득하기 위해** 

서버에 **필요 데이터를 요청하고 화면을 전환하는 위한 일련의 행위**를 말한다.

<br>

**브라우저가 화면을 전환하는 경우**

1. 브라우저의 주소창에 URL을 입력하면 해당 페이지로 이동한다.
2. 웹페이지의 링크를 클릭하면 해당 페이지로 이동한다.
3. 브라우저의 뒤로가기 또는 앞으로가기 버튼을 클릭하면 사용자가 방문한 웹페이지의 기록(history)의 뒤 또는 앞으로 이동한다.

<br>

**AJAX 요청에 의해 서버로부터 데이터를 응답받아 화면을 생성하는 경우**

브라우저의 주소창의 URL은 변경되지 않는다.

<br>

때문에

1 . 사용자의 방문 **history를 관리할 수 없음**을 의미하고 

2 . **SEO(검색엔진 최적화) 이슈의 발생** 원인이기도 하다.

<br>

**문제해결방법**

history 관리를 위해서는 

각 페이지는 브라우저의 주소창에서 구별할 수 있는 **유일한 URL을 소유**하여야 한다.

<br>

## 3. SPA와 Routing

<br>

### 3.1 전통적 링크 방식

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Link</title>
  <link rel="stylesheet" href="css/style.css">
</head>
<body>
  <nav>
    <ul>
      <li><a href="/">Home</a></li>
      <li><a href="service.html">Service</a></li>
      <li><a href="about.html">About</a></li>
    </ul>
  </nav>
  <section>
    <h1>Home</h1>
    <p>This is main page</p>
  </section>
</body>
</html>
```

<br>

link tag(`<a href="service.html">Service</a>` 등)을 클릭하면 href 어트리뷰트의 값인 리소스의 경로가 URL의 path에 추가되어 주소창에 나타나고 해당 리소스를 서버에 요청된다.

<br>

![SPA](../Images/SPA/SPA-1.png)

렌더링 과정

1 . 이때 서버는 html로 화면을 표시하는데 부족함이 없는 완전한 리소스를 클라이언트에 응답한다. 이를 **서버 렌더링**이라 한다.

<br>

2 . 브라우저는 서버가 응답한 html을 수신하고 렌더링한다.

<br>

3 . 이때 이전 페이지에서 수신된 html로 전환하는 과정에서 전체 페이지를 다시 렌더링하게 되므로 새로고침이 발생한다.

<br>

이 방식은 JavaScript가 필요없이 응답된 html만으로 렌더링이 가능하며 

각 페이지마다 고유의 URL이 존재하므로 history 관리 및 SEO 대응에 아무런 문제가 없다.

<br>

**단점**

1 . **중복된 리소스를 요청**마다 수신해야 하며, 

2 . 전체 페이지를 다시 렌더링하는 과정에서 **새로고침이 발생**하여 사용성이 좋지 않은 단점이 있다.

<br>

즉, 복잡한 웹페이지의 경우, 요청마다 중복된 HTML과 CSS, JavaScript를 매번 다운로드해야하므로 속도 저하의 요인이 된다.

<br>

### 3.2 AJAX 방식

<br>

전통적 링크 방식의 단점을 보완하기 위해 등장한 것이 AJAX(Asynchronous JavaScript and XML)이다.

<br>

AJAX는 자바스크립트를 이용해서 비동기적(Asynchronous)으로 서버와 브라우저가 데이터를 교환할 수 있는 통신 방식을 의미한다.

<br>

![SPA](../Images/SPA/SPA-2.png)

<br>

서버로부터 웹페이지가 반환되면 화면 전체를 새로 렌더링해야 하는데 

**페이지 일부만을 갱신**하고도 동일한 효과를 볼 수 있도록 하는 것이 AJAX이다.

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>AJAX</title>
  <link rel="stylesheet" href="css/style.css">
  <script src="js/index.js" defer></script>
</head>
<body>
  <nav>
    <ul id="navigation">
      <li><a id="home">Home</a></li>
      <li><a id="service">Service</a></li>
      <li><a id="about">About</a></li>
    </ul>
  </nav>
  <div class="app-root">Loading..</div>
</body>
</html>
```

<br>

link tag(`<a id="home">Home</a>` 등)에 href 어트리뷰트를 사용하지 않는다.

<br>

내비게이션이 클릭되면 link tag의 기본 동작을 prevent하고 

AJAX을 사용하여 서버에 필요한 리소스를 요청한다.

<br>

요청된 리소스가 응답되면 클라이언트에서 웹페이지에 그 내용을 갈아끼워 html을 완성한다.

<br>

**AJAX 장점**

1 . 이를 통해 불필요한 **리소스 중복 요청을 방지**할 수 있다. 

또한 

2 . 페이지 전체를 새로 렌더링할 필요가 없고 갱신이 필요한 일부만 로드하여 갱신하면 되므로 **빠른 퍼포먼스와 부드러운 화면 표시 효과**를 기대할 수 있으므로 **새로고침이 없는 보다 향상된 사용자 경험을 구현**할 수 있다는 장점이 있다.

<br>

JavaScript의 구현은 아래와 같다.

```jsx
(function () {
  const root = document.querySelector('.app-root');
  const navigation = document.getElementById('navigation');

  const routes = {
    // id: url
    home: '/data/home.json',
    service: '/data/service.json',
    about: '/data/about.html'
  };

  const render = async id => {
    try {
      const url = routes[id];
      if (!url) {
        root.innerHTML = `${url} Not Found`;
        return;
      }

      const res = await fetch(url);
      const contentType = res.headers.get('content-type');

      if (contentType?.includes('application/json')) {
        const json = await res.json();
        root.innerHTML = `<h1>${json.title}</h1><p>${json.content}</p>`;
      } else {
        root.innerHTML = await res.text();
      }
    } catch (err) {
      console.error(err);
    }
  };

  // AJAX 요청은 주소창의 url을 변경시키지 않으므로 history 관리가 되지 않는다.
  navigation.onclick = e => {
    if (!e.target.matches('#navigation > li > a')) return;
    e.preventDefault();
    render(e.target.id);
  };

  // DOMContentLoaded은 HTML과 script가 로드된 시점에 발생하는 이벤트로 load 이벤트보다 먼저 발생한다. (IE 9 이상 지원)
  // 새로고침이 클릭되었을 때, 웹페이지가 처음 로딩되었을 때, 현 페이지(예를들어 loclahost:5002)를 서버에 요청한다. 이때 Home에 필요한 리소스를 Ajax 요청한다.
  window.addEventListener('DOMContentLoaded', () => render('home'));
}());
```

<br>

**단점**

1 . AJAX는 URL을 변경시키지 않으므로 주소창의 주소가 변경되지 않는다. 

이는 브라우저의 뒤로가기, 앞으로가기 등의 **history 관리가 동작하지 않음을 의미한다.**

<br>

물론 코드 상의 history.back(), history.go(n) 등도 동작하지 않는다. 

새로고침을 클릭하면 주소창의 주소가 변경되지 않기 때문에 언제나 첫페이지가 다시 로딩된다. 

<br>

2 . 하나의 주소로 동작하는 AJAX 방식은 **SEO 이슈**에서도 자유로울 수 없다.

<br>

### 3.3 Hash 방식

<br>

AJAX의 단점을 보안한 방식이 Hash방식이다.

<br>

**AJAX의 단점**

1 . history 관리가 되지 않는 단점

2 . SEO 이슈 자유롭지 못함.

<br>

Hash 방식은 URI의 **fragment identifier(#service)**의 고유 기능인 앵커(anchor)를 사용한다.

fragment identifier는 hash mark 또는 hash라고 부르기도 한다.

```jsx
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>SPA</title>
  <link rel="stylesheet" href="css/style.css">
  <script src="js/index.js" defer></script>
</head>
<body>
  <nav>
    <ul>
      <li><a href="/">Home</a></li>
      <li><a href="#service">Service</a></li>
      <li><a href="#about">About</a></li>
    </ul>
  </nav>
  <div class="app-root">Loading...</div>
</body>
</html>
```

<br>

link tag(`<a href="#service">Service</a>` 등)의 href 어트리뷰트에 hash를 사용하고 있다.

즉, 내비게이션이 클릭되면 hash가 추가된 URI가 주소창에 표시된다.

**URL이 동일한 상태에서 hash가 변경되면 브라우저는 서버에 어떠한 요청도 하지 않는다.**

**즉, hash는 변경되어도 서버에 새로운 요청을 보내지 않으며 따라서 페이지가 갱신되지 않는다.**

<br>

hash는 요청을 위한 것이 아니라 fragment identifier(#service)의 고유 기능인 앵커(anchor)로 웹페이지 내부에서 이동을 위한 것이기 때문이다.

<br>

또한 hash 방식은 서버에 새로운 요청을 보내지 않으며 따라서 페이지가 갱신되지 않지만 페이지마다 고유의 **논리적 URL이 존재하므로 history 관리에 아무런 문제가 없다.**

```jsx
(function () {
  const root = document.querySelector('.app-root');

  const routes = {
    // hash: url
    '': '/data/home.json',
    service: '/data/service.json',
    about: '/data/about.html'
  };

  const render = async () => {
    try {
      // url의 hash를 취득
      const hash = location.hash.replace('#', '');
      const url = routes[hash];
      if (!url) {
        root.innerHTML = `${hash} Not Found`;
        return;
      }

      const res = await fetch(url);
      const contentType = res.headers.get('content-type');

      if (contentType?.includes('application/json')) {
        const json = await res.json();
        root.innerHTML = `<h1>${json.title}</h1><p>${json.content}</p>`;
      } else {
        root.innerHTML = await res.text();
      }
    } catch (err) {
      console.error(err);
    }
  };

  // 네비게이션을 클릭하면 uri의 hash가 변경된다. 주소창의 uri가 변경되므로 history 관리가 가능하다.
  // 이때 uri의 hash만 변경되면 서버로 요청을 수행하지 않는다.
  // 따라서 uri의 hash가 변경하면 발생하는 이벤트인 hashchange 이벤트를 사용하여 hash의 변경을 감지하여 필요한 AJAX 요청을 수행한다.
  // hash 방식의 단점은 uri에 불필요한 #이 들어간다는 것이다.
  window.addEventListener('hashchange', render);

  // DOMContentLoaded은 HTML과 script가 로드된 시점에 발생하는 이벤트로 load 이벤트보다 먼저 발생한다. (IE 9 이상 지원)
  // 새로고침이 클릭되었을 때, 웹페이지가 처음 로딩되었을 때, 현 페이지(예를 들어 loclahost:5003/#service)를 요청하므로
  // index.html이 다시 로드되고 DOMContentLoaded 이벤트가 발생하여 render가 호출된다.
  window.addEventListener('DOMContentLoaded', render);
}());
```

<br>

hash 방식은 uri의 hash가 변경하면 발생하는 이벤트인 **hashchange 이벤트** 를 사용하여 **hash의 변경을 감지하여** **필요한 AJAX 요청을 수행** 한다. 

<br>

**단점**

1 . hash 방식의 단점은 uri에 불필요한 #이 들어간다는 것이다. 

일반적으로 hash 방식을 사용할 때 #!을 사용하기도 하는데 이를 **해시뱅(Hash-bang)** 이라고 부른다.

<br>

2 . **SEO 이슈**

크롤러는 검색엔진이 웹사이트의 콘텐츠를 수집하기 위해 HTTP 1.1과 URL 스펙(RFC-2396같은)을 따른다. 이러한 크롤러는 JavaScript를 실행시키지 않기 때문에 hash 방식으로 만들어진 사이트의 콘텐츠를 수집할 수 없다.

<br>

### 3.4 PJAX 방식

<br>

Hash 방식의 단점을 보안해서 생긴 PJAX방식이 생겼다.

<br>

**Hash 단점**

1 . SEO 이슈

<br>

이를 보완한 방법이 HTML5의 Histroy API인 pushState와 popstate 이벤트를 사용한 PJAX 방식이다.

```jsx
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>PJAX</title>
  <link rel="stylesheet" href="css/style.css">
  <script src="js/index.js" defer></script>
</head>
<body>
  <nav>
    <ul id="navigation">
      <li><a href="/">Home</a></li>
      <li><a href="/service">Service</a></li>
      <li><a href="/about">About</a></li>
    </ul>
  </nav>
  <div class="app-root">Loading...</div>
</body>
</html>
```

<br>

link tag(`<a href="/service">Service</a>` 등)의 href 어트리뷰트에 path를 사용하고 있다

내비게이션이 클릭되면 path가 추가된 URI가 서버로 요청된다.

PJAX 방식은 내비게이션 클릭 이벤트를 캐치하고 preventDefault를 사용하여 서버로의 요청을 방지한다.

이후, href 어트리뷰트에 path을 사용하여 AJAX 요청을 하는 방식이다.

<br>

AJAX 요청은 주소창의 URL을 변경시키지 않아 history 관리가 불가능하다.

이때 사용하는 것이 pushState 메서드이다.

**pushState 메서드는 주소창의 URL을 변경하고 URL을 history entry로 추가하지만 요청하지는 않는다.**

```jsx
(function () {
  const root = document.querySelector('.app-root');
  const navigation = document.getElementById('navigation');

  const routes = {
    // path: url
    '/': '/data/home.json',
    '/service': '/data/service.json',
    '/about': '/data/about.html'
  };

  const render = async path => {
    try {
      const url = routes[path];
      if (!url) {
        root.innerHTML = `${path} Not Found`;
        return;
      }

      const res = await fetch(url);
      const contentType = res.headers.get('content-type');

      if (contentType?.includes('application/json')) {
        const json = await res.json();
        root.innerHTML = `<h1>${json.title}</h1><p>${json.content}</p>`;
      } else {
        root.innerHTML = await res.text();
      }
    } catch (err) {
      console.error(err);
    }
  };

  // popstate 이벤트는 history entry가 변경되면 발생한다.
  // PJAX 방식은 hash를 사용하지 않으므로 hashchange 이벤트를 사용할 수 없다.
  // popstate 이벤트는 pushState에 의해 발생하지 않는다.
  // 이전페이지 / 다음페이지 button 또는 history.back() / history.go(n)에 의해 발생한다.
  window.addEventListener('popstate', e => {
    // e.state는 pushState 메서드의 첫번째 인수
    console.log('[popstate]', e.state);
    // 이전페이지 / 다음페이지 button이 클릭되면 render를 호출
    render(e.state.path);
  });

  // 네비게이션을 클릭하면 주소창의 url이 변경되므로 HTTP 요청이 서버로 전송된다.
  // preventDefault를 사용하여 이를 방지하고 history 관리를 위한 처리를 실시한다.
  navigation.addEventListener('click', e => {
    if (!e.target.matches('#navigation > li > a')) return;
    e.preventDefault();
    // 이동 페이지
    const path = e.target.getAttribute('href');

    // 주소창의 url은 변경되지만 HTTP 요청이 서버로 전송되지는 않는다.
    history.pushState({ path }, null, path);
    // path에 의한 AJAX 요청
    render(path);
  });

  // 웹페이지가 처음 로딩되었을 때
  render('/');

  // 새로고침이 클릭되었을 때, 현 페이지(예를 들어 loclahost:5004/service)가 서버에 요청된다.
  // 이에 응답하는 기능이 서버 측에 추가되어야 한다.
}());
```

<br>

PJAX 방식은 서버에 새로운 요청을 보내지 않으며 따라서 페이지가 갱신되지 않는다. 

하지만 페이지마다 고유의 URL이 존재하므로 history 관리에 아무런 문제가 없다. 

또한 hash를 사용하지 않으므로 SEO에도 문제가 없다.

<br>

이는 **서버 렌더링 방식과 AJAX 방식이 혼재되어 있는 것이다.** 

서버는 클라이언트의 request hader의 Accept가 ‘text/html’이면 HTML을 응답하고, request hader의 Accept가 ‘application/json’이면 필요 리소스만 JSON으로 응답하도록 구현하여야 한다.

```jsx
// Client
(async () => {
  const res = await fetch('/service', {
    headers: { 'accept': 'application/json' }
  });

  render(await res.json());
})();
```

```jsx
// Server
const express = require('express');
const app = express();
const fs = require('fs');
const path = require('path');

app.get('/service', (req, res) => {
  res.format({
    // 새로고침에 의한 브라우저 요청
    'text/html': function(){
      res.sendFile(path.join(__dirname + '/public/data/service.html'));
    },
    // AJAX 요청
    'application/json': function(){
      res.send(JSON.parse(fs.readFileSync('./public/data/service.json', 'utf8')));
    },
    'default': function() {
      // log the request and respond with 406
      res.status(406).send('Not Acceptable');
    }
  });
});

app.listen(3000, function () {
  console.log('listening on http//localhost:3000');
});
```

<br>

## 4. Conclusion

<br>

**전통적 링크 방식에서 PJAX 방식까지 SPA의 발전 과정**

![SPA](../Images/SPA/SPA-3.png)

<br>

출처: [[https://poiemaweb.com/js-spa](https://poiemaweb.com/js-spa)]
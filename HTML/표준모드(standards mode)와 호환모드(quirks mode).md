## 표준모드(standards mode)와 쿽스모드(quirks mode)의 다른 점은 무엇인가요?

<br>

### 표준모드와 호환모드가 생겨난 이유

과거 웹 페이지는 넷스케이프 내비게이터용과 마이크로소프트의 인터넷 익스플로용의 두가지 버전으로 만들어졌다.

<br>

W3C에서 웹 표준을 제정할 당시, 기존의 브라우저들은 새롭게 만들어진 표준을 기반으로 대부분의 웹사이트들을 제대로 표현할 수 없었다.

따라서, 브라우저들은 새로운 표준으로 제작된 사이트와 예전 방식으로 제작된 사이트를 렌더링 하기 위한 두가지 모드를 제공했다.

<br>

### 렌더링하기 위해 어떠한 모드들이 있을까?

웹 브라우저는 두 가지 렌더링 모드를 가지고 있는데 호환모드(쿼크모드)와 표준모드이다.

즉, 브라우저는 선언된 doctype에 따라 렌더링할 모드를 선택하게 된다.

이과정을 Doctype Switching이라고 부른다.

<br>

### 표준모드

브라우저가 출력하고자 하는 문서가 최신이라고 판단하면 표준모드로 렌더링을 하는데 CSS2 스펙에 따라 CSS가 적용되었음을 의미한다.

<br>

### 쿼크모드

반면에 브라우저가 예전문서라고 판단하면 쿼크모드로 렌더링한다.

이 모드에서는 이전 세대의 브라우저에 맞는 비표준적 방법의 CSS를 적용한다.

<br>

즉, 쿼크모드의 목적은 오래된 웹페이지들이 최신 버전의 브라우저에서 깨져 보이지 않으려고 한다.

<br>

### 어떻게 브라우저는 문서가 최신인지 오래된 문서인지 구분할까?

DTD를 보고 쿼크모드 혹은 표준모드로 렌더링을 한다.

<br>

- DOCTYPE 선언이 존재하지 않거나 잘못 적혀있을 경우, 웹 브라우저는 문서를 쿼크 모드로 해석한다.
- DOCTYPE 선언 내의 URL이 생략된 경우, 웹 브라우저는 문서를 쿼크 모드로 해석한다. 예를 들어

<br>

```jsx
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
```

이 선언을

```jsx
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
```

<br>

### 그렇다면 HTML5의 doctype은 URL이 생략되어 있는데 쿼크모드로 구분할까?

```jsx
<!DOCTYPE html>
<html>
  <head>
    <meta charset=UTF-8>
    <title>Hello World!</title>
  </head>
  <body>
  </body>
</html>
```

<br>

예제에서 사용한 `<!DOCTYPE html>`은 HTML5에서 권장하는, 가장 간단한 방식이다.

오늘날 현존하는 모든 브라우저들은(심지어 옛날 인터넷 익스플로러 6조차도) 이 DOCTYPE은 **완전 표준 모드**로 렌더링한다.

만약 다른 DOCTYPE을 사용하게 된다면, 해당 페이지가 **거의 표준 모드나 호환 모드**로 렌더링될 수 있는 위험이 있다.

<br>

\*\* 완전 표준 모드 : 표준모드을 포함하고 있는 완벽하게 HTML과 CSS로 웹페이지를 구현

\*\* 거의 표준 모드 : 소수의 호환모드의 요소만 지원한다.

<br>

참고

- [https://developer.mozilla.org/ko/docs/Web/HTML/Quirks_Mode_and_Standards_Mode](https://developer.mozilla.org/ko/docs/Web/HTML/Quirks_Mode_and_Standards_Mode)
- [http://chongmoa.com/html/441](http://chongmoa.com/html/441)
- [https://ko.wikipedia.org/wiki/쿼크\_모드](https://ko.wikipedia.org/wiki/%EC%BF%BC%ED%81%AC_%EB%AA%A8%EB%93%9C)

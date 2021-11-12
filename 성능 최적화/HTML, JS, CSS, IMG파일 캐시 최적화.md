# HTML, JS, CSS, IMG파일 캐시 최적화

캐시 최적화를 하기전에, 사전 지식으로 캐시에대해 공부해오자.

- [캐시 기본 동작](https://github.com/FE-Lex-Kim/-TIL/blob/master/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/%EC%BA%90%EC%8B%9C%20%EA%B8%B0%EB%B3%B8%20%EB%8F%99%EC%9E%91.md)
- [캐시 검증 헤더](https://github.com/FE-Lex-Kim/-TIL/blob/master/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/%EC%BA%90%EC%8B%9C%20%EA%B2%80%EC%A6%9D%20%ED%97%A4%EB%8D%94%EC%99%80%20%EC%A1%B0%EA%B1%B4%EB%B6%80%20%EC%9A%94%EC%B2%AD.md#%EC%BA%90%EC%8B%9C-%EA%B2%80%EC%A6%9D-%ED%97%A4%EB%8D%94%EC%99%80-%EC%A1%B0%EA%B1%B4%EB%B6%80-%EC%9A%94%EC%B2%AD)

---

<br>

## HTML 캐시?

브라우저에서 HTML 파일을 캐시를 했으면 어떻게 될까?

**HTML 파일이 새롭게 빌드되어 변경되었다고 하더라도, 브라우저에서는 `max-age` 기간이 남아있다면, 캐시 되어 있는 HTML 파일을 사용할 것이다.**

<br>

HTML 파일은 항상 최신 버전을 사용해야 한다.

**그렇기 때문에 HTML 파일은 캐시를 해두면 안된다.**

따라서 HTML 파일은 항상 `**cache-control : no-cahce` 인 상태여야 한다.\*\*

<br>

`no-store`도 마찬가지로 서버에서 항상 최신 파일을 가져오지만,

`no-cache`는 만약 서버로 요청하는 네트워크 상태가 좋지 않으면, 캐시에 남아있는 오래된 파일이라도 사용해서 사용자에게 보여질 수도 있기 때문에 사용한다.

<br>

## JS, CSS, IMG 캐시?

JS와 CSS도 마찬가지로 항상 파일은 최신 버전을 사용해야한다.

하지만 no-cache를 사용하지 않아도된다.

<br>

파일명이 다르면 캐시에서 불러와 사용하지 않는다.

이러한 점을 사용한다.

<br>

**webpack의 bundling을 사용하면, JS와 CSS, IMG 파일의 이름은 빌드과정에서 해쉬화 되어져 매번 변경되어 네이밍이 지어진다.(예를들면, main.1kd8dx883b2hj54.js와 같은 파일)**

따라서 HTML 파일만 매번 서버에 새롭게 받아들여 온다면, JS와 CSS, IMG는 파일 이름이 새롭게 변경 되어 있기 때문에, **캐시 되어있는 파일과 이름이 달라 사용하지 않고 서버에서 새로운 파일을 받아온다.**

- 빌드시에만 파일명이 매번 변경된다.

<br>

**즉, 파일 이름에 따라 캐시를 사용할지 안할지를 결정한다.**

<br>

JS는 `"private, max-age=31536000"` 를 사용한다.

- js 파일에는 중요한 사용자 정보가 있을 수 도 있기 때문에 private을 사용한다
- 캐시 유효기간을 최대한 늘려서 3153600 즉, 1년으로 맞추어둔다.
  - **새롭게 빌드해서 번들링 할때만 JS 파일명이 변경되기 때문에, 최대한 길게 맞추어 놓는것이다.(새롭게 빌드되지 않으면, 파일명이 같아 계속 캐시파일을 사용함)**

<br>

CSS는 `"max-age=31536000"` 를 사용한다.

- 마찬가지로 캐시 유효기간을 최대한 늘려서 3153600 즉, 1년으로 맞추어둔다.
  - **새롭게 빌드해서 번들링 할때만 JS 파일명이 변경되기 때문에, 최대한 길게 맞추어 놓는것이다.(새롭게 빌드되지 않으면, 파일명이 같아 계속 캐시파일을 사용함)**

<br>

IMG는 `"max-age=31536000"` 를 사용한다.

- IMG 파일도 자주 바뀌는 이미지가 아니라면, 파일명을 빌드시 매번 바꾸게 해두어서 빌드시 때만, 최신으로 가져오게한다.
- 마찬가지로 캐시 유효기간을 최대한 늘려서 3153600 즉, 1년으로 맞추어둔다.
  - **새롭게 빌드해서 번들링 할때만 JS 파일명이 변경되기 때문에, 최대한 길게 맞추어 놓는것이다.(새롭게 빌드되지 않으면, 파일명이 같아 계속 캐시파일을 사용함)**

<br>

## 정리

- **HTML 파일은 캐쉬하지않아, 매번 최신 버전의 HTML을 가져온다.**
- **JS와 CSS, IMG는 webpack에 의해 파일명이 빌드시 매번 변경되기 때문에, 캐쉬를 최대한 길게 잡아, 새롭게 빌드가 되지 않으면, 캐쉬 데이터를 사용한다.**

<br>

참고

- 인프런 프론트엔드 최적화 강의
- [https://jyhwng.github.io/performance-optimization-with-cache-control](https://jyhwng.github.io/performance-optimization-with-cache-control)
- [https://ashton.codes/set-cache-control-max-age-1-year/](https://ashton.codes/set-cache-control-max-age-1-year/)

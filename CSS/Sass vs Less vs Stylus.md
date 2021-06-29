# Sass vs Less vs Stylus

<br>

## css 전처리기란

CSS 전처리기는 전처리기의 자신만의 특별한 문법을 가지고 CSS를 생성하도록 하는 통합적인 단어를 말한다.

<br>

다양한 CSS 전처리기가 있다.

이러한 전처리기들은 기존의 CSS에서 효율적이고 효과적인 기능을 추가해놓았다.

예를들어 중첩 선택자, 상속 선택자, 믹스인..등등 CSS 구조의 가독성을 더 좋고 유지 보수하기 좋게 만들어준다.

<br>

이 CSS전처리기 자체만으로는 웹서버가 인지하지 못하기 때문에 CSS전처리기에 맞는 `Compiler`를 사용해서 컴파일을 하면 실제로 우리가 사용하는 CSS 문서로 변환하게된다.

<br>

대표적인 CSS전처리기

- SASS
- LESS
- Stylus
- PostCSS

<br>

### 장점

- 재사용성 : 공통 요소 또는 반복적인 항복을 변수 또는 함수로 대체가능하다.
- 시간적 비용 감소 - 임의 함수 및 Built-in함수를 사용해 개발 시간이 절약가능하다.
- 유지 관리 - 중첩, 상속과 같은 요소로 인해 구조화된 코드로 유지 및 관리가 용이하다.

<br>

### 단점

실무에서 사용할때 퍼블리셔, 디자이너가 진행하는데 퍼블리셔나 디자이너가 배워야하는 부분이 어느정도 개발에대한 역량을 알아야한다는 점이 있다.

<br>

## Sass, Less, Stylus 비교

javscript로 작성된 Less가 초기에 가장 선호 되었다.

이후 node-sass가 나오고 sas가 선호되어 사용된다.

Stylus는 가장 늦게 나온 전처리기이다.

<br>

### Sass

Sass는 전처리기 중 가장 먼저 나온 전처리기이다.

최초에 Ruby언어를 기반으로 구동었지만 언어가 지닌 한계로 컴파일 속도가 느려 Less를 더 선호되었다.

<br>

하지만 `node-sass` 라는 Node.js 기반의 라이브러리가 나오고 Sass를 더 많이 사용하고 있다.

sass는 NPM에 등록되어있다.

<br>

### Less

Less는 Twitter의 Bootstrap에 사용되면서 전파되기 시작했고 브라우저에서 자바스크립트 Parser를 통해 실행된다.

<br>

Sass와 초반부터 다르게 Node.js 기반으로 구동되었으며 Sass에 비해 컴파일속도가 빠른것이 장점이였지만 Sass가 캐쉬를 적용한 뒤로부터 큰 차이가 없다.

<br>

### Stylus

Stylus는 가중 나중에 나온 전처리기로 Less 와 동일하기 `Node.js` 기반으로 구동된다.

가장 늦게 발표된 전처리기 지만 많은 기능이 구현되어있다.

<br>

참고

- [https://developer.mozilla.org/ko/docs/Glossary/CSS_preprocessor](https://developer.mozilla.org/ko/docs/Glossary/CSS_preprocessor)
- [https://kdydesign.github.io/2019/05/12/css-preprocessor/](https://kdydesign.github.io/2019/05/12/css-preprocessor/)

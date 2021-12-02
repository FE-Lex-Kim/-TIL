# pseudo-elements

<br>

## 가상요소란

- 선택자에 추가하는 키워드로, 선택한 요소의 **지정된 부분에 스타일을 적용할 수 있다.**
- 존재하지 않는 요소를 존재하는 것처럼 부여하여 **문서의 특정 부분 선택이 가능하다.**

<br>

가상요소는 여러가지가 있다.

- `::before` → 선택한 요소의 **맨 앞 자식으로** 의사 요소를 하나 생성한다.
- `::after` → 선택한 요소의 **맨 마지막 자식으로** 의사 요소를 하나 생성한다.
- `::first-letter` → 각 요소의 **첫 글자를** 선택한다.
- `::first-line` → 각 요소의 **첫 번째 줄을** 선택한다.

등등 있다.

<br>

## 자주 사용되는 방법

<br>

### 홈페이지 헤더나 푸터에 구분선 삽입

홈페이지 헤더 : 로그인 | 회원가입

<br>

### 가상 클래스

원하는 태그를 선택하기 위해 사용하는 선택자

:hover, :active, :focus...등등

<br>

### 내용의 앞 또는 뒤에 컨텐츠 추가(`content = ""`)

내용의 앞과 뒤에 콘텐츠를 추가하고, `content = ""`라는 속성을 넣는다.

이 속성을 사용해서 float의 레이아웃을 조정하는데 **clearfix를 적용**하는데 사용하기도 한다.

<br>

참고

- [https://developer.mozilla.org/ko/docs/Web/CSS/::after](https://developer.mozilla.org/ko/docs/Web/CSS/::after)
- [https://goddino.tistory.com/43](https://goddino.tistory.com/43)
- [https://velog.io/@eunoia/Pseudo-Element가상요소란](https://velog.io/@eunoia/Pseudo-Element%EA%B0%80%EC%83%81%EC%9A%94%EC%86%8C%EB%9E%80)

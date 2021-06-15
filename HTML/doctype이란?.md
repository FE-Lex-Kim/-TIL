## `doctype`이란?

<br>

HTML이 어떤 버전으로 작성되었는지 알려주어 해당 버전으로 파싱해서 웹 브라우저가 내용을 올바르게 표시하게 해준다.

<br>

문서형은 XHTML, HTML5, HTML 세가지가 존재한다.

이렇게 마크업 문서의 요소와 속성들을 처리하는 기준이되고(파싱) 유효성 검사에 사용된다.

<br>

\*\* [유효성 검사](https://mytory.net/archives/434)란? : 마크업 문법상 에러가 없는지 표준에 맞게 작성되었는지 알 수 있고, 크로스 브라우징, 사용성과 접근성, SEO 최적화 개선에도 도움이 된다.

- 즉, [웹표준을 지키는것](https://helloworld-88.tistory.com/96)

<br>

이전 버전의 HTML은 SGML에 기반을 두고 만들어졌기 때문에 DTD가 필요하다.

<br>

- [SGML이란?](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=kimone0915&logNo=150119128236) : Standard Generalized Markup Language, 즉, 표준 마크업 언어 규약이다. 컴퓨터에서 쓰이는 문서를 작성할때 어떠한 시스템 환경에서도 정보의 손실 없이 전송,저장이 가능하도록 국제 표준화 기구에서 정한것이다.
- [DTD란?](https://araikuma.tistory.com/769) : 문서별로 해당 문서에 대한 태그 정보가 다르면 가독성이 어렵고 파악하기 힘들기에, 공통 규칙에 따라 문서를 작성하게 한다. 따라서 어떤 요소의 값이 어떤 값을 나타내고 있는지 명확하게 알 수 있다. 또한 프로그램 등에서 문서 처리할때도 사용되는 태그가 알고 있기 때문에 처리하기 쉽다. 즉, 문서 구조를 정의하기 위한 언어이다.

<br>

```jsx
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

출처: https://araikuma.tistory.com/770?category=878258 [프로그램 개발 지식 공유]
```

<br>

하지만 HTML5는 이것을 설정하지 않아도 되지만 하위 호한을 위해 설정한다.

<br>

참고

- [https://araikuma.tistory.com/770?category=878258](https://araikuma.tistory.com/770?category=878258)
- [https://lelana.tistory.com/59](https://lelana.tistory.com/59)
- [https://araikuma.tistory.com/769](https://araikuma.tistory.com/769)

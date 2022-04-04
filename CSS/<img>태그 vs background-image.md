- [<img>태그 vs background-image](#태그-vs-background-image)
  - [Img 태그](#img-태그)
  - [background-image](#background-image)

# <img>태그 vs background-image

<br>

이미지를 페이지에서 보여줄때 방법이 두 가지가 있다.

첫 번째는 img 태그

두 번째는 background-image

를 통해 이미지를 보여줄 수 있다.

<br>

**언제, 어떤 방식으로 선택해야하는지 시멘틱 관점에서 선택해야한다.**

<br>

## Img 태그

- **이미지가 사용자에게 컨텐츠 이해에 도움을 된다고하면 `img` 태그를 사용한다.**
  - SEO, 성능 면에서 효율적이다.
- **이미지 자체에 의미가 있는 경우 사용한다.**
  - alt 속성을 통해 해당 이미지의 의미를 설명할 수 있다.
- **이미지가 페이지 전체 컨텐츠에서 의미를 가지고 있는 경우 사용한다.**

<br>

## background-image

- 의미를 가지지 않는 경우 사용한다.
  - ex) 아이콘
- 이미지 위에 텍스트가 들어가는 경우
- CSS sprites를 사용해서 이미지 로딩 속도 향상시키는 경우
- 순수하게 디자인을 위해서 사용하는 경우

<br>

참고

- [https://chlolisher.tistory.com/77](https://chlolisher.tistory.com/77)

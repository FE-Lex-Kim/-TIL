IMG의 srcset과 sizes 속성
===

## 1. srcset 속성

> 일반적으로 반응형 웹에서 이미지를 지원하기위해 css의 미디어쿼리를 사용하지만 더 편리하고 짧게 HTML에서 쓸수있다.

srcset 속성에서 쓸 이미지의 링크를 걸어준뒤 뒤에 widht를 의미하는 w의 크기를 결정해준다.

400w는 뷰포트가 400px이하일때 small 이미지가 사용되고

700w는 뷰포트가 401px에서 700px 일 때 medium 이미지가 사용되고

마지막 1000w 는 뷰포트가 701px이상일때 large 이미지가 사용된다
```html
<img srcset="small.jpg 400w,
medium.jpg 700w,
large.jpg 1000w"
alt="이미지">
```

>> 뷰포트는 가로 넓이를 의미를 한다.

## 2. sizes 속성

> sizes 속성은 미디어조건과 그 조건에 해당하는 이미지의 ‘최적화 출력 크기’를 지정한다.

srcset 속성을 기본으로 sizes속성의 min-width:1000px 의미는
1000px 이상일때 700px의 최적화 이미지를 쓴다는 의미이다.

700px 에서 가장 적합한 이미지는 medium 이미지 이다.

```html
<img srcset="small.jpg 400w,
medium.jpg 700w,
large.jpg 1000w"
sizes="(min-width: 1000px) 700px" alt="이미지">
```

자세한 내용과 내용에 대한 참고는 [여기 링크](https://heropy.blog/2019/06/16/html-img-srcset-and-sizes/)를 참고했습니다
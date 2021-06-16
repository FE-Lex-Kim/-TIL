## `srcset` 속성

같은 비율의 다양한 크기를 가지는 동일 이미지들을 2개이상 명시하는 속성이다.

<br>

이미 대표적으로 브라우저가 큰 이미지를 한 번 불러왔으면, 오히려 해상도 떨어지는 작은 이미지를 또 불러올 필요를 느끼지 못해 뷰포트 크기가 변경되어도 잉미지 갱신이 일어나지 않는다.

<br>

이미지를 불러오는 행동은 네트워크, 메모리, 성능, 시간등 여러 비용을 지불해야한다.

<br>

따라서 다른 비율의 다양한 크기의 다른 이미지를 사용하고 싶으면 srcset 속성이 아닌 @media 미디어쿼리 사용을 권장한다.

<br>

브라우저의 뷰포트에 따라 이미지 크기를 반응형으로 처리하기 위해서 사용한다.

<br>

적당한 크기보다 훨씬 작은 화면에 큰 이미지를 표시 할 필요가 없다.

그렇게 하는 것은 대역폭을 낭비하는 것이다.

특히, 모바일 사용자는 기기에 맞는 작은 이미지 대신 데스크톱에 사용되는 커다른 이미지를 다운로드하느라 대역폭을 낭비하고 싶어하지 않는다. 이상적인 상황은 다양한 해상도를 준비해 두고, 웹사이트 데이터를 받는 기기에 따라 적당한 사이즈를 제공하는 것이다.

<br>

```jsx
<img
  srcset="images/heropy_small.png 400w,
          images/heropy_medium.png 700w,
          images/heropy_large.png 1000w"
  width="500"
  src="images/heropy.png"
  alt="HEROPY"
/>
```

srcset 뒤의 이미지 경로 뒤의 w를 통해서 뷰포트 너비를 결정하고 해당 이하일때 이미지가 사용된다.

1. small은 400px 뷰포트 너비가 이하일떄
2. medium은 700px 뷰포트 이하일때
3. large는 701px 뷰포트 이상부터 사용된다.

<br>

### 장점

1. 모바일일때 굳이 큰이미지를 사용해서 대용량 이미지를 쓸필요가 없다.
2. CSS나 Javascript를 사용하면 브라우저 렌더링이 HTML srcset속성을 통해 이미지를 조정한것보다 초기에 페이지 렌더링 할때 느리다.

<br>

참고

- [https://heropy.blog/2019/06/16/html-img-srcset-and-sizes/](https://heropy.blog/2019/06/16/html-img-srcset-and-sizes/)
- [https://oddcode.tistory.com/174?category=836267](https://oddcode.tistory.com/174?category=836267)

# 이미지 사이즈 최적화 with picture Element, webp

**이미지는 브라우저의 리소스들 중에서도 큰 용량을 차지한다.**

**그 만큼 로딩 성능에 큰 영향을 준다.**

<br>

이미지 사이즈를 최적화 해서 로딩 성능을 높여보자.

브라우저에서 보여지는 이미지의 사이즈가 300 \* 300 이라고 해보자.

서버에서 로드해서 온 이미지의 원본 사이즈는 3000 \* 3000 이라고 한다면, 브라우저에서 10배나 줄여서 보여지게 되므로 **불필요한 이미지 크기를 로드하게 된다**.(물론 이미지의 퀄리티는 좋아지겠지만)

<br>

브라우저에서 보여지는 이미지 사이즈의 크기만큼 딱 알맞게 맞춰서 가져와보자.

<br>

하지만 **레티나 디스플레이에서는 눈으로 화소를 구분할 수 없는 화소의 밀도**를 가진다.

따라서 **브라우저 이미지의 사이즈가 300 \* 300 이라한다면, 레티나를 위해 2배정도 더 늘려주는 것이 좋다.**(600 \* 600)

<br>

## 이미지 변환기

<br>

### Webp

이미지 파일형식은 굉장히 많다.

그 중에서 브라우저에서 흔하게 사용하는 포맷은 JPG, PNG이다.

하지만 그 보다 더 나은 **Webp를 사용하면 이미지 용량에 도움이된다.**

<br>

구글이 개발한 **WebP는 손실/비손실 압축 이미지 파일을 위한 이미지 포맷**이다.

Webp는 사진 이미지 압축 효과가 크다.

**덕분에 30%가량 줄여 웹사이트에서 트래픽 감소 및 로딩 속도를 단축해준다.**

<br>

**JPG 보다 10~30% 압축하고**

**PNG 보다 26%나 더작게 이미지를 압축한다.**

<br>

### squoosh

![이미지 사이즈 최적화 with picture Element, webp](./../Images/이미지%20사이즈%20최적화/이미지%20사이즈%20최적화-1.png)

구글에서 개발한 이미지 변환기이다.

**이미지의 파일 형식을 변형할 수도 있고 사이즈도 변경이 가능하다.**

<br>

![이미지 사이즈 최적화 with picture Element, webp](./../Images/이미지%20사이즈%20최적화/이미지%20사이즈%20최적화-2.png)

<br>

### 단점

Webp는 용량도 굉장히 압축시켜주고 화질도 좋다.

하지만 Webp는 2010년에 구글에서 만든 이미지 포맷이다.

비교적 다른 포맷들보다 훨씬 최근에 만들어져서 지원하지 않는 브라우저가 있다.

<br>

![이미지 사이즈 최적화 with picture Element, webp](./../Images/이미지%20사이즈%20최적화/이미지%20사이즈%20최적화-3.png)

<br>

Wepb를 지원하지 않는 브라우저에서는 **다른 파일 포맷으로 로드하는 picture 태그를 사용하면 된다.**

<br>

### <picture> The Picture element

<br>

picutre HTML Element는 `<source>`, `<img>` 를 포함한다.

**viewport의 크기, 특정 이미지 파일 포맷을 지원하지 않을때, <source>을 대신해서 <img>를 사용한다.**

<br>

이중에서 이미지 사이즈 최적화를 하기때문에 Webp 파일을 지원하지 않는 브라우저일때 사용해보자.

```jsx
<picture>
  <source srcset="logo.webp" type="image/webp">
  <img src="logo.png" alt="logo">
</picture>
```

<br>

type에 따라 source Element를 사용할지 대신에 img Element를 사용할지 결정된다.

현재 type이 **"image/webp" 이라서 만약 브라우저에서 Webp를 지원하지 않으면 img Element를 사용한다.**

<br>

intersection observer과 같이 사용해보자.

```jsx
function Card(props) {
  let imgRef = useRef(null);

  useEffect(() => {
    function callback(entries, observer) {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          const target = entry.target;
          const previousSibling = entry.target.previousSibling;

          target.src = target.dataset.src;
          previousSibling.srcset = previousSibling.dataset.srcset;// <-----------
          observer.unobserve(entry.target);
        }
      });
    }

    const option = {
     ...
    };

    const observer = new IntersectionObserver(callback, option);
    observer.observe(imgRef.current);
  });

  return (
    <div>
      <picture>
        <source data-srcset={props.webp} type="image/webp" /> // <-----------
        <img data-src={props.image} ref={imgRef} />
      </picture>
    </div>
  );
}

export default Card;
```

<br>

source Element는 src 프로퍼티에 이미지 주소를 넣는것이 아니라 **srcset에 넣는다**.(src에 넣고 왜 안되지 한참 삽질했었다..)

<br>

**이제 브라우저가 webp를 지원하지 않으면 source Element 대신에 img Element를 사용한다.**

<br>

사용전

![이미지 사이즈 최적화 with picture Element, webp](./../Images/이미지%20사이즈%20최적화/이미지%20사이즈%20최적화-4.png)

<br>

사용후

![이미지 사이즈 최적화 with picture Element, webp](./../Images/이미지%20사이즈%20최적화/이미지%20사이즈%20최적화-5.png)

<br>

**Webp를 사용하니 이미지 용량이 굉장히 줄어들어 로드 속도까지 줄어든것까지 확인 할 수 있다.**

<br>

참고

- 인프런 프론트엔드 최적화 강의
- [https://www.wpbeginner.com/beginners-guide/speed-wordpress-save-images-optimized-web/](https://www.wpbeginner.com/beginners-guide/speed-wordpress-save-images-optimized-web/)
- [https://velog.io/@hustle-dev/웹-성능을-위한-이미지-최적화](https://velog.io/@hustle-dev/%EC%9B%B9-%EC%84%B1%EB%8A%A5%EC%9D%84-%EC%9C%84%ED%95%9C-%EC%9D%B4%EB%AF%B8%EC%A7%80-%EC%B5%9C%EC%A0%81%ED%99%94)
- [https://kinsta.com/blog/optimize-images-for-web/](https://kinsta.com/blog/optimize-images-for-web/)
- [https://ko.wikipedia.org/wiki/레티나\_디스플레이](https://ko.wikipedia.org/wiki/%EB%A0%88%ED%8B%B0%EB%82%98_%EB%94%94%EC%8A%A4%ED%94%8C%EB%A0%88%EC%9D%B4)
- [https://developers.google.com/speed/webp/gallery1](https://developers.google.com/speed/webp/gallery1)
- [https://developer.mozilla.org/en-US/docs/Web/HTML/Element/picture](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/picture)
- [https://squoosh.app/](https://squoosh.app/)

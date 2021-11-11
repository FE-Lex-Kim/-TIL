# 폰트 사이즈 최적화 with subset, unicode-range, data-uri

<br>

폰트 사이즈를 최적화해서 웹 로딩 성능을 향상 시켜보자.

<br>

## 폰트 포멧 변경

폰트의 포멧은 다양하다.

그중에 대표적인 포멧 4가지가 있다.

1. TTF / OTF : 일반적으로 웹 폰트로 많이 쓰이는 폰트 포멧이다. **거의 폰트가 압축이 되지않아 용량이 크다.**
2. WOFF : Mozila, Microsoft, Opera Software 에서 만들고 웹 폰트을 위한 포멧이다. TTF, OTF 포멧의 데이터 사이즈가 커서 나온 포맷이다. **압축했기 때문에 데이터 사이즈가 작다는 장점이 있다.**
3. WOFF2 : WOFF의 다음 세대의 포멧이다. **WOFF 보다도 평균 30% 더 데이터를 압축한다**. 하지만 여러 브라우저에서 호환이 안되는 점이 있다.

   ![폰트 사이즈 최적화](./../Images/폰트%20사이즈%20최적화/폰트%20사이즈%20최적화-1.png)

<br>

TTF와 OTF에 비해 WOFF, WOFF2를 사용하면 크게 용량을 줄여주기 때문에 **포멧 컨버터를 통해 변경하는게 좋다.**

<br>

### transfonter

[transfonter](https://transfonter.org/) 컨버터를 사용해보자.

<br>

![폰트 사이즈 최적화](./../Images/폰트%20사이즈%20최적화/폰트%20사이즈%20최적화-2.png)

WOFF, WOFF2로 포멧을 변경하기위해 체크박스를 클릭한뒤, 폰트를 업로드한뒤 다운로드를 하면된다.

<br>

하지만 위에서 설명한대로 WOFF와 WOFF2는 지원하지 않는 브라우저들이 있다.

그에대한 방법을 배워보자.

<br>

### format()

WOFF2 폰트를 사용한다고 했을때, 지원하지 않는 브라우가 있다면, 대신에 **다음으로 지정한 폰트 포멧을 사용하도록하는 방법이 있다.**

`src`의 속성중 `format()`을 사용하면된다.

<br>

```jsx
@font-face {
  font-family: BMYEONSUNG;
  src: url("BMYEONSUNG.woff2") format("woff2"),
    url("BMYEONSUNG.woff") format("woff"),
    url("./assets/fonts/BMYEONSUNG.ttf");
}
```

<br>

**폰트 포멧 타입을 format의 인수로 넣어준다.**

해당 포멧 타입을 지원한다면 앞서 적은 url 주소의 폰트를 사용한다.

<br>

만약 지원하지 않으면, **다음 url 주소의 폰트를 사용한다.**

<br>

## local()

항상 폰트를 네트워크를 통해 다운로드 받는 방법만 있는것은 아니다.

**자신의 기기에서 해당 폰트를 가지고 있으면 굳이 폰트를 다운로드 받지않고 사용하는 방법이 있다.**

<br>

`src` 속성중에 `local()` 를 사용하면된다.

```jsx
@font-face {
  font-family: BMYEONSUNG;
  src: local("BMYEONSUNG"),
		url("BMYEONSUNG.woff2") format("woff2"),
    url("BMYEONSUNG.woff") format("woff"),
    url("./assets/fonts/BMYEONSUNG.ttf");
}
```

<br>

local의 인수에 font-family 이름을 적어주면 된다.

<br>

위 코드는 다음 순서로 폰트를 찾는다.

1. `local("BMYEONSUNG")`
2. `url("BMYEONSUNG.woff2") format("woff2")`
3. `url("BMYEONSUNG.woff") format("woff")`
4. `url("./assets/fonts/BMYEONSUNG.ttf")`

<br>

## subset

영어는 26개의 알파벳으로 이루어져 있어서 대소문자를 포함해 총 72글자가 폰트에 사용된다.

하지만 **한글은 자음, 모음의 조합으로 총 11,172자나 된다고 한다. 그래서 한글 폰트의 용량이 크다.**

<br>

아래의 이미지를 보면, 실제로 일상생활에서 사용하지 않는 조합의 글자들이 많다.

![폰트 사이즈 최적화](./../Images/폰트%20사이즈%20최적화/폰트%20사이즈%20최적화-3.png)

이미지 출처 : [https://d2.naver.com/helloworld/4969726](https://d2.naver.com/helloworld/4969726)

<br>

노란색으로 표시된 글자는 **일상생활에서 보지도 못한 글자들이다.**

이런 글자들 까지도 포함하기 때문에 용량이 크다.

따라서 **이런 글자들을 제외한 글자의 개수는 2,350라고 한다.**

최근 폰트들은 2,350개의 글자만를 사용한다고 한다.

<br>

KS(국가 표준)에서 만든 2,350개의 글자는 [이곳에서](https://raw.githubusercontent.com/nacyot/korean_subset_glyphs/master/glyphs.txt) 확인 할 수 있다.

<br>

이제 subset 폰트로 만드는 방법을 알아보자.

이전에 사용했던 [transfonter](https://transfonter.org/) 폰트 컨버터를 사용해보자.

![폰트 사이즈 최적화](./../Images/폰트%20사이즈%20최적화/폰트%20사이즈%20최적화-2.png)

**Characters 부분에서 해당 2,350개의 글자를 넣으면 subset 폰트로 변경된다.**

다운로드를 받으면 2,350개의 글자들만 폰트가 변경되고 다른 글자들은 기본폰트가 된다.

<br>

대략 75%정도 용량이 줄어든다.

<br>

## unicode-range

지정된 유니코드만 폰트로 지정할 수 있다.

**즉, 내가 원하는 글자만 폰트로 지정하게 할 수 있다.**

<br>

`font-face`의 속성중 `unicode-range`를 사용하면된다.

<br>

```jsx
@font-face {
  font-family: BMYEONSUNG;
  src: local("BMYEONSUNG"), url("BMYEONSUNG.woff2") format("woff2"),
    url("BMYEONSUNG.woff") format("woff"), url("./assets/fonts/BMYEONSUNG.ttf");

  unicode-range: U+00-52F, U+1E00-1FFF, U+2200 –22FF;
}
```

<br>

장점으로는 브라우저에서 **내가 폰트로 적용하려는 글자가 없으면, 다운로드 하지않는다는 점이다.**

**웹 폰트를 사용하지 않으면 불필요한 다운로드를 막을 수 있다.**

<br>

아직은 unicode-range가 쓰일 곳이 있을까? 생각이 든다.

<br>

참고

- 인프런 프론트엔드 최적화 강의
- [https://d2.naver.com/helloworld/4969726](https://d2.naver.com/helloworld/4969726)
- [https://ko.wikipedia.org/wiki/오픈타입](https://ko.wikipedia.org/wiki/%EC%98%A4%ED%94%88%ED%83%80%EC%9E%85)
- [https://ko.wikipedia.org/wiki/웹*오픈*폰트\_형식](https://ko.wikipedia.org/wiki/%EC%9B%B9_%EC%98%A4%ED%94%88_%ED%8F%B0%ED%8A%B8_%ED%98%95%EC%8B%9D)
- [https://medium.com/@aitareydesign/understanding-of-font-formats-ttf-otf-woff-eot-svg-e55e00a1ef2](https://medium.com/@aitareydesign/understanding-of-font-formats-ttf-otf-woff-eot-svg-e55e00a1ef2)
- [https://transfonter.org/](https://transfonter.org/)
- [https://wit.nts-corp.com/2017/02/13/4258](https://wit.nts-corp.com/2017/02/13/4258)
- [https://raw.githubusercontent.com/nacyot/korean_subset_glyphs/master/glyphs.txt](https://raw.githubusercontent.com/nacyot/korean_subset_glyphs/master/glyphs.txt)

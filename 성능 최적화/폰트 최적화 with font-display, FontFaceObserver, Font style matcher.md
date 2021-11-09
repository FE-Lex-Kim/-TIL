# 폰트 최적화 with font-display, FontFaceObserver, **Font style matcher**

<br>

폰트 자체가 사이즈가 크다면 로드하는데 시간이 걸린다.

**이때 텍스트가 폰트에 적용되기 전에, 렌더링시 텍스트가 보여지는 방식이 2가지 형태로 나타난다.**

<br>

![font-display, FontFaceObserver, **Font style matcher](./../Images/font-display,%20FontFaceObserver,%20Font%20style%20matcher/font-display,%20FontFaceObserver,%20Font%20style%20matcher-1.gif)

이미지 출처: [https://d2.naver.com/helloworld/4969726](https://d2.naver.com/helloworld/4969726)

<br>

## FOUT(Flash Of Unstyled Text)

브라우저 별로 다르게 나타난다. FOUT 같은 형태는 IE, Edge 등의 브라우저에서 나타난다.

폰트가 로드되어 적용 되기전에,

1. **폰트가 적용되지 않은 상태의 텍스트가 먼저 나타나고**
2. 폰트가 로드된이후, 기본 폰트에서 **폰트가 적용된 텍스트로 변경되게 된다.**

<br>

이 방식은 웹 폰트 로딩 여부에 관계없이 **텍스트가 항상 보이는 장점이 있다.**

하지만 글꼴의 자간, 높이 등 서식이 달라 웹 폰트 **적용 전과 후에 레이아웃이 변경될 수 있다.**

<br>

예시) 메인 화면에서 텍스트가 중요한 내용이면, FOUT를 적용하고 텍스트가 바로 보이게 하면된다.

예시의 단점은 폰트가 적용되기 이전 레이아웃이 깨진것처럼 동작 할 수도 있다.

<br>

## FOIT(Flash Of Invisible Text)

Firefox, Chrome 등의 브라우저에서 나타난다.

폰트가 로드되기전에는,

1. **텍스트가 화면에 보이지 않는다.**
2. 폰트가 로드된이후, **폰트가 적용된 텍스트가 나타난다.**

<br>

**한 번에 웹 폰트를 보여줄 수 있다는 장점이 있다.**

하지만 웹 폰트의 로딩이 늦으면 **빈 텍스트가 노출되는 문제점이 있다.**

<br>

예시) 메인 화면에서 텍스트가 그리 중요한 내용이 아니라, 디자인 적인 내용이라면, FOIT를 적용하고 폰트가 적용된 텍스트가 이후에 화면에 보여지게 하면 된다.

예시의 단점은 폰트가 바로 보이지않아 사용자 경험 관점에서 안좋다.

<br>

두 가지 경우 모두 장,단점이 있다.

브라우저에서 텍스트가 처음에 어떻게 보일지에 따라, **폰트 적용 시점을 컨트롤 하는 방법이 있다.**

<br>

## font-display

**웹 폰트의 로딩 상태에 따른 동작을 설정할 수 있다.**

font-display는 @font-face의 프로퍼티에 포함되어 있다.

<br>

font-display는 5가지 속성이 있다.

1. auto
2. block
3. swap
4. fallback
5. optional

<br>

auto는 브라우저의 기본 동작이다.

<br>

### block

![font-display, FontFaceObserver, **Font style matcher](./../Images/font-display,%20FontFaceObserver,%20Font%20style%20matcher/font-display,%20FontFaceObserver,%20Font%20style%20matcher-2.png)

이미지 출처 : [https://d2.naver.com/helloworld/4969726](https://d2.naver.com/helloworld/4969726)

<br>

blcok 옵션은 FOIT와 비슷하게 동작한다.

로드되기 이전에 텍스트가 화면에 보이지 않는다.

<br>

다른점은 **3초이후에 까지 폰트가 로드되지 않아 화면에 보이지 않으면,**

**기본 폰트 텍스트를 보여준다.**

**이후 폰트가 로드 되면 폰트가 적용된 텍스트로 변경된다.**

<br>

※ fallback : 기본 폰트

<br>

### swap

![font-display, FontFaceObserver, **Font style matcher](./../Images/font-display,%20FontFaceObserver,%20Font%20style%20matcher/font-display,%20FontFaceObserver,%20Font%20style%20matcher-3.png)

이미지 출처 : [https://d2.naver.com/helloworld/4969726](https://d2.naver.com/helloworld/4969726)

<br>

FOUT와 똑같이 동작한다.

폰트가 로드되기전에 **기본 폰트로 텍스트를 보이게한다.**

이후 폰트 로드가 완료되면 **폰트가 적용된 텍스트로 보여지게된다.**

<br>

### fallback

FOUT와 FOIT 합쳐진 것처럼 동작한다.

<br>

1. 처음 화면에 텍스트를 보여주지 않는다. (0s ~ 0.1s)
2. 0.1초 안에 로드되지 않으면, 3초 까지 **기본 폰트로 텍스트를 보이게해 폰트 로드를 기다려준다.** (0.1 ~ 3s)
3. 3초안에 폰트가 로드 되지 않으면**, 3초 이후에 폰트가 로드되어도 기본 폰트로 계속 남겨둔다.** (3s ~ —s)
4. 3초 이후에 로드된 폰트는 **캐시에 저장**해두어 사용자가 이후에 방문했을때 빠르게 적용하게 해준다.

<br>

### optional

fallback 옵션과 비슷하게 동작한다.

다른 점은 0.1초 까지 화면에 텍스트가 보이지 않는다.

0.1초 이후 기본 폰트 텍스트로 화면에 보여준다.

**이후 폰트를 로드 받아도 네트워크 환경에 따라서 폰트를 텍스트에 적용하고 말기를 결정한다.**

<br>

### font-display 브라우저 지원

`font-display` 속성은 Internet Explorer 계열 브라우저를 제외한 브라우저가 지원한다

<br>

![font-display, FontFaceObserver, **Font style matcher](./../Images/font-display,%20FontFaceObserver,%20Font%20style%20matcher/font-display,%20FontFaceObserver,%20Font%20style%20matcher-4.png)

<br>

## FOIT with FontFaceObserver

FOIT의 단점 중 하나인 갑자기 **화면에서 깜빡하고 확! 나오는 점이다.**

FOIT를 **FontFaceObserver 라이브러리와 animation을 사용하면 보다 부드럽게 구현 가능하다.**

<br>

**animation을 통해 opacity를 0에서 1로 자연스럽게 나타나는 방법을 사용하면 된다.**

이렇게 구현하려면 **폰트가 로드되는 시점을 지속적으로 관찰해야한다.**

**FontFaceObserver 라이브러리가 폰트가 로드되는지 지속적으로 관찰해준다.**

<br>

간단하게 **FontFaceObserver 라이브러리 사용법을 알아보자**

<br>

### **FontFaceObserver**

설치

```bash
npm i fontfaceobserver
```

<br>

사용방법

```jsx
import FontFaceObserver from "fontfaceobserver";

var font = new FontFaceObserver("My Family");

font.load().then(
  function () {
    console.log("Font is available");
  },
  function () {
    console.log("Font is not available");
  }
);
```

<br>

FontFaceObserver에 인수로 **font-family 이름을 넣어주면된다.**

font.load 메서드를 호출해서 폰트가 로드 완료되면, Promise를 반환한다.

<br>

콜백함수를 두개를 넣어줄 수 있다.

첫번째 콜백 함수는 폰트 로드가 **성공했을때 호출한다.**

두번째 콜백 함수는 폰트 로드가 **실패했을때 호출한다.**

<br>

이제 본격적으로 FOIT에 animation과 함께 적용해보자

<br>

### FOIT with **FontFaceObserver and Animation** in React

드디어 다 준비물이 다 준비되었다. React에 적용해보자

<br>

```jsx
import FontFaceObserver from "fontfaceobserver";

function BannerVideo() {
  let [isFontLoaded, setIsFontLoaded] = useState(false);
  useEffect(() => {
    var font = new FontFaceObserver("My Family");

    font.load().then(function () {
      console.log("My Family has loaded");
      setIsFontLoaded(true);
    });
  });

  const animation = {
    transition: "opacity 1s ease",
    opacity: isFontLoaded ? 1 : 0,
  };

  return (
      <div
        className="w-full h-full flex justify-center items-center"
        style={animation}>
        <div className="text-white text-center">
          <div className="text-6xl leading-none font-semibold">KEEP</div>
          <div className="text-6xl leading-none font-semibold">CALM</div>
          <div className="text-3xl leading-loose">AND</div>
          <div className="text-6xl leading-none font-semibold">RIDE</div>
          <div className="text-5xl leading-tight font-semibold">LONGBOARD</div>
        </div>
      </div>
    </div>
  );
}

export default BannerVideo;
```

<br>

최초한번만 FontFaceObserver을 등록하기 때문에 useEffect 내부에서 작성한다.

useState를 사용해서 폰트가 로드 완료되면 opacity를 0에서 1로 변경하게 해준다.

이제 trasition을 통해 자연스럽게 텍스트가 스르륵 보여지게 된다.

<br>

기존 FOIT 보다 훨씬 자연스럽고 디자인스럽게 텍스트가 보여지게된다.

<br>

## FOUT with **Font style matcher**

FOUT의 단점은 기존 폰트를 적용하기 때문에 **레이아웃이 깨진다는 점이였다.**

<br>

폰트가 적용된 이후 **font-size**, **letter-spacing**, **line-height 등등** 달라 레이아웃이 깨졌다.

[\*\*Font style matcher](https://sangziii.github.io/fontStyleMatcher/)을 사용하면\*\* font-size, letter-spacing, line-height, letter-spacing, word-spacing의 CSS 속성을 변경시켜 폰트 적용전,후를 비교해서 상태를 확인할 수 있다.

<br>

![font-display, FontFaceObserver, **Font style matcher](./../Images/font-display,%20FontFaceObserver,%20Font%20style%20matcher/font-display,%20FontFaceObserver,%20Font%20style%20matcher-5.png)

<br>

참고

- 인프런 프론트엔드 최적화 강의
- [https://developer.mozilla.org/en-US/docs/Learn/Performance/video#optimizing_video_delivery](https://developer.mozilla.org/en-US/docs/Learn/Performance/video#optimizing_video_delivery)
- [https://d2.naver.com/helloworld/4969726](https://d2.naver.com/helloworld/4969726)
- [https://www.uscreen.tv/blog/5-ways-to-make-video-files-smaller/](https://www.uscreen.tv/blog/5-ways-to-make-video-files-smaller/)
- [https://www.npmjs.com/package/fontfaceobserver](https://www.npmjs.com/package/fontfaceobserver)
- [https://sangziii.github.io/fontStyleMatcher/](https://sangziii.github.io/fontStyleMatcher/)

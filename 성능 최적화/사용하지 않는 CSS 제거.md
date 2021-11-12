# 사용하지 않는 CSS 제거

실제로 서비스에서 사용하지 않는 CSS를 제거하는 방법에 대해 알아보자.

TailwindCSS, Bootstrap, MaterializeCSS.. 등등 과 같은 CSS Framework를 사용한다면, 사용하지 않는 굉장히 많은 CSS들이 포함되어 있다.

<br>

먼저 사용하지 않는 CSS가 있는지 분석해보자

<br>

## Chrome Coverage

Chrome Coverage 패널에 들어가서 새로고침 버튼을 클릭해보자.

![사용하지 않는 CSS 제거](./../Images/사용하지%20않는%20CSS%20제거/사용하지%20않는%20CSS%20제거-1.png)

<br>

Unused Bytes 탭을 보면 서비스에서 29.6%는 사용하지 않는 부분이라는 의미이다.

위와 같이 빨간색과 파란색 바가 보인다.

빨간색은 : 서비스에서 사용하지 않는 것을 의미한다.

파란색은 : 서비스에서 사용하는 코드를 의미한다.

<br>

![사용하지 않는 CSS 제거](./../Images/사용하지%20않는%20CSS%20제거/사용하지%20않는%20CSS%20제거-2.png)

위와같이 해당 파일을 클릭하면 위의 Source 패널이 보여진다.

빨간색 영영은 사용하지 않는 코드라는 것을 의미한다.

빨간색이 많을 수록 사용하지 않으니, 성능에 좋지 않은 코드이다.

<br>

어떻게 사용하지 않는 CSS를 지울수 있을까?

<br>

## PurgeCSS

[PurgeCSS](https://purgecss.com/) 을 사용하면 사용하지 않는 CSS를 분석해서 지워준다.

<br>

![사용하지 않는 CSS 제거](./../Images/사용하지%20않는%20CSS%20제거/사용하지%20않는%20CSS%20제거-3.png)

위의 이미지 처럼 색칠한 부분을 **HTML 파일에서 모든 문자열을 추출한뒤, CSS 파일과 비교해 사용하지 않는 클래스가 있으면 삭제한다.**

<br>

설치

```bash
npm i -D purgecss
```

Dev에서만 사용하기위해서 -D flag을 적용했다.

<br>

### - -css

- -css 뒤에 어떤 **css 파일을 비교할건지 적어주면된다.**

해당 css 파일은 **html또는 js 파일과 비교해 사용하지 않는 클래스 있으면 지워준다.**

<br>

예시

```bash
purgecss --css css/app.css css/palette.css --content src/index.html
```

css/app.css, css/palette.css 파일을 분석한다.

<br>

### - -content

**구체적으로 분석할 파일을 - -content 뒤에 적어준다. 예를들어 html, js 파일을 넣어준다.**

```bash
purgecss --css css/app.css --content src/index.html src/**/*.js
```

src/index.html, src/\*_/_.js 파일을 앞서 적은 css 파일들과 비교한다.

<br>

### - -output

default 값으로는 console에서 결과가 보여진다.

만약 분석해서 사용하지 않은 **css를 제거한 css 파일을 얻고 싶으면 적어준다.**

**- -output 뒤에 구체적인 폴더 위치를 적어주면 해당 폴더에 생성 또는 덮어씌어준다.**

<br>

```bash
purgecss --css css/app.css --content src/index.html "src/**/*.js" --output build/css/
```

build/css/ 폴더에 css파일을 생성 또는 덮어씌워준다.

<br>

### - -config

만약 purgeCSS의 **여러 옵션을 설정한** **Configuration 파일을 만든다면, Configuration 파일을 적어준다.**

<br>

```bash
purgecss --config ./purgecss.config.js
```

./purgecss.config.js 파일을 적용한다.

<br>

### 적용

**package.json 파일의 script에 추가해서 실행해보자.**

<br>

```bash
"purge": "purgecss --css ./build/static/css/*.css --content ./build/index.html ./build/static/js/*.js --output ./build/static/css/"
```

<br>

사용하지 않는 css 들이 싹 지워져있을 것이다.

<br>

### **Configuration file**

purgecss를 사용해서 사용하지 않는 css를 제거했지만, **아직도 사용하지 않는 css 들이 남은것을 볼 수 있다.**

문자열을 추출하는 과정에서 문자열을 나누는 기준이 애매해, **제대로 나누지 않은 문자열이 있는것이 문제이다.**

<br>

![사용하지 않는 CSS 제거](./../Images/사용하지%20않는%20CSS%20제거/사용하지%20않는%20CSS%20제거-4.png)

<br>

위와 같이 특수문자로 클래스를 지정한다면, 문자열을 추출하는 과정에서 하나의 클래스를 두 개, 세 개로 쪼개서 비교 했을것이다. (**예를들면 hover:opacity-100을 하나의 문자열로 추출하지 않고 hover, opacity, 100으로 나누어 css 파일의 클래스와 비교한것이다.**)

<br>

커스텀으로 구체적으로 설정해서 추출해보자.

먼저 config 파일을 **purgecss.config.js 이름으로 만든다.**

**우리가 커스텀으로 문자열을 추출하는 기준을 설정하는 옵션은 defaultExtractor이다.**

<br>

```jsx
module.exports = {
  defaultExtractor: (content) => content.match(/[\w-/:]+(?<!:)/g) || [],
};
```

<br>

모든 content를 어떤 기준으로 나눌건지를 정규표현식으로 적어주면된다.

이제 특수 문자도 포함했기 때문에 올바르게 문자열을 추출해서 css을 비교한뒤 제거한다.

<br>

※ config의 다른 옵션들은 [이곳에서](https://purgecss.com/configuration.html#configuration-file) 찾아보면 된다.

<br>

참고

- 인프런 프론트엔드 최적화 강의
- [https://purgecss.com/#sponsors-🥰](https://purgecss.com/#sponsors-%F0%9F%A5%B0)
- [https://tailwindcss.com/docs/optimizing-for-production#writing-purgeable-html](https://tailwindcss.com/docs/optimizing-for-production#writing-purgeable-html)
- [https://www.affde.com/ko/remove-unused-css.html](https://www.affde.com/ko/remove-unused-css.html)
- [https://www.keycdn.com/blog/remove-unused-css](https://www.keycdn.com/blog/remove-unused-css)
- [https://purgecss.com/configuration.html#configuration-file](https://purgecss.com/configuration.html#configuration-file)

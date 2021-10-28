# Chrome Performance 패널로 bottleneck 현상 분석

<br>

> 병목(bottleneck) 현상은 **전체 시스템의 성능이나 용량이 하나의 구성 요소로 인해 제한을 받는 현상을** 말한다. "병목"이라는 용어는 물이 병 밖으로 빠져나갈 때 병의 몸통보다 병의 목부분의 내부 지름이 좁아서 물이 상대적으로 천천히 쏟아지는 것에 비유한 것이다. -위키백과

<br>

JS의 코드로 인해 bottleneck 현상을 분석해보고 해결해보자.

<br>

![Chrome Performance 패널로 bottleneck 현상 분석](./../Images/bottleneck%20분석/bottleneck%20분석-1.png)

<br>

최초에 network에서 자바스크립트 파일을 로드한다.

이때 자바스크립트 파일중 가장 긴 로드시간이 걸리는것은 **main.chu..**와 **main.a5d6e...** 이 두 가지이다.

<br>

![Chrome Performance 패널로 bottleneck 현상 분석](./../Images/bottleneck%20분석/bottleneck%20분석-2.png)

<br>

두 가지의 자바스크립트 파일이 로드가 완료된 시점때, Main의 프레임 차트를 보자.

**index.js, APp.js, ViewPage/index.js**, 등등 우리가 알고있는 컴포넌트 이름들이 보인다.

자바스크립트 파일이 로드가 완료가 된 시점때 해당 컴포넌트들이 실행되었다는 의미이다.

<br>

![Chrome Performance 패널로 bottleneck 현상 분석](./../Images/bottleneck%20분석/bottleneck%20분석-3.png)

<br>

프레임 차트에서 각각 컴포넌트들이 실행이 완료된 시점때, network 프레임 차트에서 art..이라고 하는 부분이 보인다.

art를 로드하면서 동시에 아래에

<br>

DCL, FP, FCP, L이 위에서 보인다.

**DCL**은 DOMContentLoaded Event가 시작된 시점인 파란색 선으로 표시된다.

**FP**는 First Paint가 시작된 시점인 연초색으로 표시된다.

**FCP**는 First Contentful Paint가 시작된 시점인 녹색선으로 표시된다.

**L**은 Onloaded Event가 시작점 시점인 빨간색 선으로 표시된다.

<br>

![Chrome Performance 패널로 bottleneck 현상 분석](./../Images/bottleneck%20분석/bottleneck%20분석-4.png)

<br>

art를 로드하면서 그 다음 뭉탱이로 파일들이 실행됬다.

프레임 차트의 가장 아래시점 부터 보게 되면 remove..acter이라는 것이 여러번 보인다.

remove..acter은 실제로 코드에 있는 함수이다.

<br>

```jsx
/*
 * 파라미터로 넘어온 문자열에서 일부 특수문자를 제거하는 함수
 * (Markdown으로 된 문자열의 특수문자를 제거하기 위함)
 * */
function removeSpecialCharacter(str) {
  const removeCharacters = [
    "#",
    "_",
    "*",
    "~",
    "&",
    ";",
    "!",
    "[",
    "]",
    "`",
    ">",
    "\n",
    "=",
    "-",
  ];
  let _str = str;
  let i = 0,
    j = 0;

  for (i = 0; i < removeCharacters.length; i++) {
    j = 0;
    while (j < _str.length) {
      if (_str[j] === removeCharacters[i]) {
        _str = _str.substring(0, j).concat(_str.substring(j + 1));
        continue;
      }
      j++;
    }
  }

  return _str;
}
```

<br>

해당 함수는 문자열에서 특수문자을 제거하는 부분이다.

위의 프레임 차트에서는 **removeSpecialCharacter**가 실행되는 시점이 **오래걸려 렌더링 되는 성능을 지연시키고 있다.**

**removeSpecialCharacter은 실행이 오래 걸려서 나누어서 여러번 실행된다.**

여러번 나누어 실행되는 이유는 오래걸리기 때문에 가비지 컬렉션이 메모리를 정리시켜서 다시 실행 시키는 것이다.

<br>

## Bottleneck 현상 해결

<br>

**removeSpecialCharacter의 로직을 줄여서 렌더링 속도를 높여보자.**

```jsx
function removeSpecialCharacter(str) {
  let _str = str;
  _str.replace(/[\#\_\*\~\&\;\!\[\]\`>\n=\-`]/g, "");

  return _str;
}
```

<br>

![Chrome Performance 패널로 bottleneck 현상 분석](./../Images/bottleneck%20분석/bottleneck%20분석-5.png)

<br>

확실히 두번째 Task 부분이 굉장히 줄어들어있다.

<br>

## 정리

컴포넌트 내부의 처리 시간이 오래걸리는 JS 로직이 있으면 bottleneck 현상 즉, 전체적으로 성능이 굉장히 느려진다.

이러한 로직이 있는지 Performance 패널을 통해 분석할 수 있어야한다.

어떤 문제가 있는지 확인했으면 해당 로직을 더 효율적으로 코드를 리팩토링 해야한다.

<br>

참고

- 인프런 프론트엔드 개발자를 위한, 실전 웹 성능 최적화 강의
- [https://ko.wikipedia.org/wiki/병목](https://ko.wikipedia.org/wiki/%EB%B3%91%EB%AA%A9)

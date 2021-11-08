# Chrome Performance 패널

Performance 패널은 **런타임을 분석하는 툴이다.**

<br>

시작하기전 **팁 부터 알아 두자.**

마우스로 frame 차트를 움직여가면서 보기 힘들다.

W, A, S, D를 사용하면 **좌우, 확대가 키보드로써 가능**하므로 빠르게 움직일 수 있다.

<br>

## 옵션

<br>

![Chrome Performance 패널](./images/../../Images/Chrome%20Performance%20패널%20사용법/Chrome%20Performance-1.png)

### Disable JavaScript samples

Main 섹션에서 Javascript 함수의 시스템상 자세한 call stacks들을 보여주지 않게 하는 옵션이다.

<br>

Javascript call stack들을 제한했기 때문에 Main 섹션의 프레임 차트들이 굉장히 짧아진것을 볼 수 있다.

<br>

적용 전

![Chrome Performance 패널](./images/../../Images/Chrome%20Performance%20패널%20사용법/Chrome%20Performance-2.png)

<br>

적용후

![Chrome Performance 패널](./images/../../Images/Chrome%20Performance%20패널%20사용법/Chrome%20Performance-3.png)

<br>

### Enable advanced paint instrumentation

브라우저에서 레이어들이 composite 되기전 paint 단계에서 자세하게 볼 수 있는 옵션이다.

<br>

layer의 정보를 자세히 보는 방법은

1. Frames 섹션에서 하나의 frame을 선택한다.

   ![Chrome Performance 패널](./images/../../Images/Chrome%20Performance%20패널%20사용법/Chrome%20Performance-4.png)

2. display information에서 Layers tab을 선택하면 자세히 보여진다.

   ![Chrome Performance 패널](./images/../../Images/Chrome%20Performance%20패널%20사용법/Chrome%20Performance-5.png)

<br>

## 최상단 cpu 타임라인

<br>

![Chrome Performance 패널](./images/../../Images/Chrome%20Performance%20패널%20사용법/Chrome%20Performance-6.png)

<br>

CPU가 태스크를 처리하는데 얼마나 차지했는지, 처리한 시간을 그래픽으로 보여준다.

recording이 끝난후 최초에는 가장 활발하게 태스크가 활동한 부분을 먼저 줌인 해주어 둔다.

<br>

이 부분에서 드래그를 통해 태스크를 처리하는 부분에대한 자세한 분석이 Main 섹션에서 보여진다.

<br>

### FPS

<br>

![Chrome Performance 패널](./images/../../Images/Chrome%20Performance%20패널%20사용법/Chrome%20Performance-7.png)

<br>

![Chrome Performance 패널](./images/../../Images/Chrome%20Performance%20패널%20사용법/Chrome%20Performance-8.png)

<br>

우측을 보면 FPS, CPU, NET가 보인다.

<br>

FPS는 frames per second을 의미한다.

초록색 바는 frame 비율이 괜찮은 것을 의미한다.

빨간색 바는 frame 비율이 드랍되어 낮아진것을 의미한다.

- frame이 낮아지면 그만큼 user experience는 안 좋아진다.

![Chrome Performance 패널](./images/../../Images/Chrome%20Performance%20패널%20사용법/Chrome%20Performance-9.png)

<br>

### Frame 섹션

Frame 섹션은 특정한 프레임이 얼마나 걸렸는지, 드랍이 일어났는지 보여준다.

<br>

### 색깔에 따른 태스크 종류

프레임 차트에서 보여지는 색깔

<br>

노란색 : **Javascript** 활동을 의미한다.

보라색 : **레이아웃** 활동을 의미한다.

초록색 : **paint, composite**을 의미한다.

<br>

## Network

<br>

![Chrome Performance 패널](./images/../../Images/Chrome%20Performance%20패널%20사용법/Chrome%20Performance-10.png)

recording 하는동안 요청한 network 활동을 보여준다.

<br>

### 네트워크 섹션에서 색깔의 의미

- 노란색 : Javacript 요청을 의미한다.
- 초록색 : Images 요청을 의미한다.
- 파란색 : HTML 요청을 의미한다.
- 보라색 : CSS 요청을 의미한다.

<br>

### 요청 우선순위

왼쪽-위 **진한 파란색 네모난 요청의 의미는 높은 우선순위**의 요청을 의미한다. 왼쪽-위 **밝은 색상의 파란색은 좀더 낮은 우선순위의 요청**을 의미한다.

<br>

예를 들면 위의 사진에서 프레임이 클릭되어 빨간색 테두리 프레임은 왼쪽-위가 밝은 파란색이다. 그 아래의 프레임은 상대적으로 진하다. 우선 순위가 높은 요청이라는 의미다.

<br>

### 좌측 얇은 선, 하얀 박스, 진한 박스, 우측 얉은 선

![Chrome Performance 패널](./images/../../Images/Chrome%20Performance%20패널%20사용법/Chrome%20Performance-11.png)

<br>

- **좌측의** **얇은 선은 연결의 시작을 의미한다.** 얇은 선이 이어져 있는 부분은 실질적인 요청을 보내기전에 Connection 시간을 의미한다.
- **밝은 부분의 네모 박스는 요청을 보내고 기다리는 시간**을 의미한다.
- 가운데 **진한 네모 박스는 실제 요청을 다운로드하는 시간**을 의미한다.
- 우측의 얇은 선은 **main thread가 태스크를 처리하는 시간**을 의미한다.

<br>

**네트워크 패널의 Timing 섹션을** 보면 더 자세히 알 수 있다.

![Chrome Performance 패널](./images/../../Images/Chrome%20Performance%20패널%20사용법/Chrome%20Performance-12.png)

## Timings

<br>

![Chrome Performance 패널](./images/../../Images/Chrome%20Performance%20패널%20사용법/Chrome%20Performance-13.png)

<br>

![Chrome Performance 패널](./images/../../Images/Chrome%20Performance%20패널%20사용법/Chrome%20Performance-14.png)

<br>

DCL, FCP, FMP, L 등의 타이밍 순서를 알려준다.

또한

React라면 어떤 컴포넌트가 실행되는 순서인지 확인 가능하다.

<br>

DCL : DOMContentLoaded 이벤트

- HTML, CSS 파싱이 끝나는 지점
- DOM, CSSOM 만든이후 Render Tree를 만들 시점

[FCP : Frist Contentful Paint](https://web.dev/i18n/ko/fcp/)

- 사용자가 화면에서 컨텐츠를 볼 수 있는 첫번째 지점을 표시한 것이다.
- 사용자가 감지하는 로드 속도를 측정할 수 있는 중요한 표시이다.
- FCP가 빠르면 사용자가 페이지에서 뭔가 진행되고 있음을 인지할 수 있어 안심이된다.

[FMP : First Meaningful Paint](https://web.dev/first-meaningful-paint/)

- 브라우저에서 중요한 컨텐츠가 페이지에 보여지는 시점이다.

L : Load 이벤트

- HTML의 모든 리소스가 로드된 시점

<br>

## Main

<br>

![Chrome Performance 패널](./images/../../Images/Chrome%20Performance%20패널%20사용법/Chrome%20Performance-15.png)

페이지 recording 시 페이지의 main thread의 활동을 보여준다.

Performance 패널에서 제일 중요한 섹션이라고 할 수 있다.

<br>

프레임을 클릭하면 자세한 정보를 Summarary 탭에서 볼 수 있다.

차트의 X좌표 방향은 recording 하는 동안 시간을 의미한다.

차트의 Y좌표 방향은 call stack을 보여준다.

- 태스크가 위에서 발생하면 아래방향으로 일어난다.

<br>

### 프레임 색깔의 의미

노란색 : 스크립트 활동을 의미한다.

보라색 : 렌더링 활동을 의미한다.

<br>

### 팁

Network, Main 섹션에서 더 자세하게 태스크 부분을 마우스로 선택하고 싶으면, Shift + 클릭 드래그 하면된다

<br>

## activities table

Main 섹션 프레임만 보지 않고 더 자세한 분석을 한 것을 볼 수 있다.

<br>

![Chrome Performance 패널](./images/../../Images/Chrome%20Performance%20패널%20사용법/Chrome%20Performance-16.png)

<br>

### Summary

선택한 부분의 태스크의 처리한 시간과, 비율을 차트로 보여준다.

<br>

### Bottom-Up

해당 태스크의 최하단 태스크부터 보여준다.

즉, 가장 마지막으로 실행된 작업들을 먼저 보여준다.

<br>

![Chrome Performance 패널](./images/../../Images/Chrome%20Performance%20패널%20사용법/Chrome%20Performance-17.png)

<br>

![Chrome Performance 패널](./images/../../Images/Chrome%20Performance%20패널%20사용법/Chrome%20Performance-18.png)

<br>

Self Time : 해당 태스크만을 처리한 시간을 보여준다.

Total Time : 태스크의 하위 자식들의 처리시간을 총합해서 보여준다.

<br>

### Call Tree

Bottom-Up과 반대로 root 태스크부터 보여준다.

<br>

### Event Log

recording 하는동안 발생한 Event 들을 보여준다.

<br>

![Chrome Performance 패널](./images/../../Images/Chrome%20Performance%20패널%20사용법/Chrome%20Performance-19.png)

<br>

이렇게 발생한 Event들은 Call Tree, Bottom-Up, Event-Log에서 확인 할 수 있다.

<br>

예를들어서 페이지에서 클릭했을때, 브라우저에서 Event를 root ativity로 발생시킨다.

**Call Tree, Event-Log** 프레임 차트에서 보면 최상위이 올라가 있을것이다.

<br>

참고

- 프론트엔드 최적화 강의 인프런
- [https://developer.chrome.com/docs/devtools/evaluate-performance/](https://developer.chrome.com/docs/devtools/evaluate-performance/)
- [https://web.dev/i18n/ko/fcp/](https://web.dev/i18n/ko/fcp/)
- [https://web.dev/first-meaningful-paint/](https://web.dev/first-meaningful-paint/)
- [https://tuhbm.github.io/2019/04/02/devTools-performance/](https://tuhbm.github.io/2019/04/02/devTools-performance/)

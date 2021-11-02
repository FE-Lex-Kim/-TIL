# Reflow, Repaint

<br>

오늘날 대부분 기기는 초당 60회(60fps)의 빈도로 화면을 새로고침 한다.

브라우저에서 애니메이션이 있다면, 브라우저는 새로고침 빈도에 맞춰서 화면에 \*프레임을 제공해주어야한다.

<br>

※ 애니메이션은 **여러개의 프레임(이미지)들이 합쳐져서** **연속적으로 동작하는** 것처럼 보이는 것이다.

<br>

각 프레임에는 16ms 가량의 시간만 할당된다.(1초/60 = 16.66ms)

하지만 실제로 브라우저는 **화면에 보여주기전에 미리 실행 준비를** 해야하므로 10ms 이내에 모든 작업을 완료해야한다.

이 제한시간내에 작업을 수행하지 못하면 **프레임 속도가 떨어지고 화면에서 컨텐츠가 버벅거리는 현상(Jank 쟁크현상)이 발생해** 거부감을 준다.

<br>

브라우저에서 CSS 애니메이션 작업을 최적화할 수 있다.

이미지 또는 어떠한 컨텐트를 이동시킬때 postion으로 이동시키는 것이 아닌 transform, opacity을 통해 **GPU를 사용하면 빠른 최적화를 통해 성능 향상 시킬 수 있다.**

<br>

왜 GPU를 사용하는게 빠른 최적화를 보여줄까?

먼저 브라우저 렌더링 과정을 단순하게 알아보자

## 브라우저 렌더링 과정

<br>

![Reflow, Repaint](./../Images/Reflow,%20Repaint/Reflow,%20Repaint-1.png)

이미지 출처 : [Chrome Dev](https://developers.google.com/web/fundamentals/performance/rendering?hl=ko)

<br>

### DOM + CSSOM = Render Tree

HTML과 CSS를 파싱해서 각각 DOM과 CSSOM을 만들어낸다.

이렇게 만들어낸 DOM + CSSOM을 통해 브라우저에 보여질 Render Tree를 만들어 낸다.

<br>

### Layout

Render Tree를 바탕으로 각각의 요소 위치가 브라우저에 들어간다. 예를 들어 width, height, position 등등 과 같은 공간을 차지하고 어디에 배치 하는지 계산하는 단계이다.

<br>

### Paint

각 요소들이 올바른 위치로 이동한후 paint 단계가 시작된다.

paint 단계는 Render Tree를 화면의 픽셀로 변환해준다(paint, 색칠하는것 처럼). 쉽게 설명하면 텍스트, 색, 이미지, border(경계) 및 shadow(그림자) 등 모든 **시각적 부분을 그리는 작업을 말한다.**

<br>

### Composite

HTML과 CSS을 통해 여러개의 layer가 만들어진 경우, 여러개의 layer들을 합쳐서 한장의 layer로 만드는 과정이다.

<br>

아래의 동영상을 보면 Layout과 paint 실행된다.

이후 layer가 여러개 겹쳐있는것을 composite하는 과정을 보여준다.

[리플로우, 리페인트 프로세스를 시각화한 동영상](https://www.youtube.com/watch?v=ZTnIxIA5KGw&t=26s)

<br>

## Reflow, Repaint

### reflow

flow 앞에 re가 붙었으니 다시 flow를 실행한다는 의미이다.

어째서 flow가 붙었는지는 모르겠지만 flow를 layout이라고 생각하면 될것같다.

앞에서 설명했던데로 layout은 document의 위치가 변경됬을때의 단계였다.

**즉, 위치가 다시 변경되었을때, layout 단계가 다시 실행되는 것을 reflow라고 한다.**

<br>

### repaint

paint 앞에 re가 붙었으니 다시 paint를 실행한다는 의미이다.

앞에서 설명했던데로 paint는 텍스트, 색, 이미지, visibility ..와 같은것을 브라우저에 나타내는 단계였다.

**즉, 이러한 값들이 다시 변경되었을때, paint 단계가 다시 실행되는 것을 repaint 라고한다.**

<br>

하지만 중요한 사실은!

레이아웃의 변화는 reflow고 나머지가 repaint라고 생각하면된다.

**레이아웃이 변화하지 않았다면 repaint만 일어날 수 있다는 점이다.**

<br>

## Reflow와 Repaint가 발생하면 어떻게 된다는 것일까?

이렇게 어떠한 요소가 브라우저에서 움직이게된다면, Reflow and Repaint가 발생하게 된다.

<br>

이렇게 변경이 된다면, 브라우저가 다른 모든 요소를 확인하고 페이지를 Reflow를 수행해야한다.

만약 영향을 받은 영역이 있다면 다시 Repaint를 해야하고 최종적으로 Composite를 진행한다.

<br>

**정리하자면 Reflow, Repaint 발생시 CPU 비용이 많이들게 된다.**

만약 변경되는 요소가 많으면, 10ms 안에 화면에 보여주기 위한 실행 준비를 완료하지 못해 Jank 현상이 발생한다.

<br>

## 언제 Reflow와 Repaint가 발생할까?

- only Reflow : DOM요소가 추가, 삭제, 업데이트 되었을때
- Reflow and Repaint : DOM 요소가 `display: none` 되었을때
- only Repaint : DOM 요소가 `Visibility: hidden` 일때
  - 레이아웃 or 위치의 변화가 없기 때문이다.
- Reflow and Repaint : DOM 요소가 애니메이션으로 움직였을때
- Reflow : 윈도우 사이즈가 변경되었을때

등등..

<br>

간단하게 정리하려고 했지만, 이해하기 위한 예시가 들어가다보니 내용이 길어졌다.

<br>

## **transform, opacity을 이용한** GPU 사용

CSS 속성중 transform, opacity를 사용하면 GPU를 사용해서 계산한후 Composite 단계에 넘겨주게된다.

![Reflow, Repaint](./../Images/Reflow,%20Repaint/Reflow,%20Repaint-2.png)

이미지 출처 : [Chrome Dev](https://developers.google.com/web/fundamentals/performance/rendering?hl=ko)

<br>

**따라서 Reflow, Repaint를 GPU가 대신 실행하므로, CPU의 부담을 줄이게 되어 성능 향상에 큰 도움이든다.**

<br>

참고

- [https://developers.google.com/web/fundamentals/design-and-ux/animations/animations-and-performance?hl=ko](https://developers.google.com/web/fundamentals/design-and-ux/animations/animations-and-performance?hl=ko)
- [https://cresumerjang.github.io/2019/06/24/critical-rendering-path/](https://cresumerjang.github.io/2019/06/24/critical-rendering-path/)
- [https://dev.to/gopal1996/understanding-reflow-and-repaint-in-the-browser-1jbg](https://dev.to/gopal1996/understanding-reflow-and-repaint-in-the-browser-1jbg)
- [https://developers.google.com/web/fundamentals/performance/rendering?hl=ko](https://developers.google.com/web/fundamentals/performance/rendering?hl=ko)
- [https://stackoverflow.com/questions/2549296/whats-the-difference-between-reflow-and-repaint](https://stackoverflow.com/questions/2549296/whats-the-difference-between-reflow-and-repaint)

# 모바일 touch 이벤트시 click 이벤트 발생 이슈 해결

<br>

pc에서 click 이벤트를 거는 요소가 있을때, 마찬가지로 모바일에서 touch 이벤트를 걸어야할때도 있다.

<br>

하지만 touch 이벤트가 발생시에

1. touchStart
2. touchEnd
3. mouseMove
4. mouseDown
5. mouseUp
6. click

이런 순서대로 다른 이벤트들도 같이 발생하게된다.

따라서 touch 이벤트만을 발생시키고 싶었지만, pc을 위해 사용했던 click 이벤트 또한 같이 발생한다.

<br>

이러한 문제를 해결하기 위해서는 touchend 이벤트에 `preventDefault()` 를 호출해야한다.

<br>

참고

- [https://ui.toast.com/weekly-pick/ko_20220106](https://ui.toast.com/weekly-pick/ko_20220106)

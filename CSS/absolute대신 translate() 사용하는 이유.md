# absolute대신 `translate()`

<br>

## 1. 목적

- absolute : 특정 위치에 위치시키기 위해
- translate: 디자인 모션

<br>

## 2. x축, y축 시작점

- absolute : 조상 엘리먼트의 최상단좌측이 시작점
- translate : 부모 요소에 따라 지정해주는것이 아니라면 요소가 갖고 있는 최상단좌측이 기준점이다.

<br>

## 3. 영향력

- absolute : 주변 요소에 영향이 있다.
- translate : 좌표 공간을 변형시키기에 다른 요소에 영향을 미치지 않는다.

<br>

## 4. 성능

- absolute : positioning을 사용하면 리플로우, 리페인팅이 발생해서 렌더링에 영향을 주기에 cpu 계산이 발생한다.
- translate : GPU 처리를 한다.

<br>

참고

- [https://developer.mozilla.org/en-US/docs/Learn/Performance/CSS](https://developer.mozilla.org/en-US/docs/Learn/Performance/CSS)
- [https://bbosong-develop.tistory.com/5](https://bbosong-develop.tistory.com/5)

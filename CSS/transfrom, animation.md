# transfrom, animation

## transform:translate(x, y)

X 또는 Y축만큼 이동 시킨다.

## transition

transition을 사용하여 동적인 효과를 준다.

```css
transition: height 0.4s ease;
```

**transition-property** - 프로퍼티를 지정한다.(가로길이, 세로길이 등)

**transition-duration** - 동적인 효과가 진행되는 시간을 지정한다.

**transition-timing-function** - 효과의 진행속도를 조절한다.(점점 빠르게, 점점 느리게 등)

**transition-delay** - transition 요청을 받은 후 실제 실행할 때까지 지연시간을 설정한다.

## animation

```css
.div-name {
    animation: 이름 | 시작지연시간 | 재생속도 | 재생시간 | 반복횟수 | 진행방향
 }

@keyframes animation이름 {
   from {
     color: blue;
     transform: rotate(1 turn);
 }
   to {
     color: yellow;
     transform: translateX(30px)
 }
}

```

위처럼 단축표현이 아니라

프로퍼티별로 써줄수 있다.

```css
animation-delay

animation-direction

animation-duration

animation-name
```

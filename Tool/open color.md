# opne color 라이브러리 사용법

<br>

설치

```jsx
$ npm install open-color
```

<br>

styled component 사용

```jsx
import oc from 'open-color';

const StyledInput = styled.input`
	color: ${oc.teal[7]};
`;

console.log(oc);
```

<br>

oc 객체

![open color](../Images/open%20color/open-color.png)

<br>

- 이 글은 [open color](https://yeun.github.io/open-color/) 문서를 참고하여 정리한 내용입니다.
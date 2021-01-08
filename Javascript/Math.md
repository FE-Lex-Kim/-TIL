# Math

- 출처 [모던 자바스크립트 Deep Dive](http://www.yes24.com/Product/Goods/92742567?OzSrank=1)을 보고 정리한 내용입니다.

<br>

표준 빌트인 객체(standard built-in object)인 Math는 수학적인 상수와 함수를 위한 프로퍼티와 메서드를 제공한다. 

Math는 생성자 함수가 아니다. 따라서 Math는 정적 프로퍼티와 정적 메서드만 제공한다.

<br>

## 1. Math 프로퍼티

<br>

### 1.1. Math.PI

<br>

원주율 PI 값(π ≈ 3.141592653589793)을 반환한다.

```jsx
Math.PI; // -> 3.141592653589793
```

<br>

## 2. Math 메서드

<br>

### 2.1. Math.abs

<br>

Math.abs 메서드는 인수로 전달된 숫자의 절대값(absolute value)을 반환한다. 절대값은 반드시 0 또는 양수이어야 한다.

```jsx
Math.abs(-1);        // -> 1
Math.abs('-1');      // -> 1
Math.abs('');        // -> 0
Math.abs([]);        // -> 0
Math.abs(null);      // -> 0
Math.abs(undefined); // -> NaN
Math.abs({});        // -> NaN
Math.abs('string');  // -> NaN
Math.abs();          // -> NaN
```

<br>

### 2.2. Math.round

<br>

Math.round 메서드는 인수로 전달된 숫자의 소수점 이하를 반올림한 정수를 반환한다.

```jsx
Math.round(1.4);  // -> 1
Math.round(1.6);  // -> 2
Math.round(-1.4); // -> -1
Math.round(-1.6); // -> -2
Math.round(1);    // -> 1
Math.round();     // -> NaN
```

<br>

### 2.3. Math.ceil

<br>

Math.ceil 메서드는 인수로 전달된 숫자의 소수점 이하를 올림한 정수를 반환한다.

```jsx
Math.ceil(1.4);  // -> 2
Math.ceil(1.6);  // -> 2
Math.ceil(-1.4); // -> -1
Math.ceil(-1.6); // -> -1
Math.ceil(1);    // -> 1
Math.ceil();     // -> NaN
```

<br>

### 2.4. Math.floor

<br>

Math.floor 메서드는 인수로 전달된 숫자의 소수점 이하를 내림한 정수를 반환한다. 즉, Math.ceil 메서드의 반대 개념이다.

<br>

인수가 양수인 경우 소수점 이하를 떼어 버린 다음 정수를 반환하고, 음수인 경우 소수점 이하를 떼어 버린 다음 -1을 곱한 정수를 반환한다.

```jsx
Math.floor(1.9);  // -> 1
Math.floor(9.1);  // -> 9
Math.floor(-1.9); // -> -2
Math.floor(-9.1); // -> -10
Math.floor(1);    // -> 1
Math.floor();     // -> NaN
```

<br>

### 2.5. Math.sqrt

<br>

Math.sqrt 메서드는 인수로 전달된 숫자의 제곱근을 반환한다.

```jsx
Math.sqrt(9);  // -> 3
Math.sqrt(-9); // -> NaN
Math.sqrt(2);  // -> 1.414213562373095
Math.sqrt(1);  // -> 1
Math.sqrt(0);  // -> 0
Math.sqrt();   // -> NaN
```

<br>

### 2.6. Math.random

<br>

Math.random 메서드는 임의의 난수(랜덤 숫자)를 반환한다.

Math.random 메서드가 반환한 난수는 0에서 1 미만의 실수다. 즉, 0은 포함되지만 1은 포함되지 않는다.

```jsx
Math.random(); // 0에서 1 미만의 랜덤 실수(0.8208720231391746)

/*
1에서 10 범위의 랜덤 정수 취득
1) Math.random으로 0에서 1 미만의 랜덤 실수를 구한 다음, 10을 곱해 0에서 10 미만의 랜덤 실수를 구한다.
2) 0에서 10 미만의 랜덤 실수에 1을 더해 1에서 10 범위의 랜덤 실수를 구한다.
3) Math.floor로 1에서 10 범위의 랜덤 실수의 소수점 이하를 떼어 버린 다음 정수를 반환한다.
*/
const random = Math.floor((Math.random() * 10) + 1);
console.log(random); // 1에서 10 범위의 정수
```

<br>

### 2.7. Math.pow

<br>

Math.pow 메서드는 첫 번째 인수를 밑(base)으로, 두 번째 인수를 지수(exponent)로 거듭제곱한 결과를 반환한다.

```jsx
Math.pow(2, 8);  // -> 256
Math.pow(2, -1); // -> 0.5
Math.pow(2);     // -> NaN
```

<br>

Math.pow 메서드 대신 ES7에서 도입된 지수 연산자 를 사용하면 가독성이 더 좋다.

```jsx
// ES7 지수 연산자
2 ** 2 ** 2; // -> 16
Math.pow(Math.pow(2, 2), 2); // -> 16
```

<br>

### 2.8. Math.max

<br>

Math.max 메서드는 전달받은 인수 중에서 가장 큰 수를 반환한다. 

인수가 전달되지 않으면 -Infinity를 반환한다.

```jsx
Math.max(1); // -> 1
Math.max(1, 2); // -> 2
Math.max(1, 2, 3); // -> 3
Math.max(); // -> -Infinity
```

<br>

배열을 인수로 전달받아 배열의 요소 중에서 최대값을 구하려면 Function.protootype.apply 메서드 또는 스프레드 문법을 사용해야 한다.

```jsx
// 배열 요소 중에서 최대값 취득
Math.max.apply(null, [1, 2, 3]); // -> 3

// ES6 스프레드 문법
Math.max(...[1, 2, 3]); // -> 3
```

<br>

### 2.9. Math.min

<br>

Math.min 메서드는 전달받은 인수 중에서 가장 작은 수를 반환한다. 인수가 전달되지 않으면 Infinity를 반환한다.

```jsx
Math.min(1); // -> 1
Math.min(1, 2); // -> 1
Math.min(1, 2, 3); // -> 1
Math.min(); // -> Infinity
```

<br>

배열을 인수로 전달받아 배열의 요소 중에서 최소값을 구하려면 Function.protootype.apply 메서드 또는 스프레드 문법을 사용해야 한다.

```jsx
// 배열 요소 중에서 최소값 취득
Math.min.apply(null, [1, 2, 3]); // -> 1

// ES6 스프레드 문법
Math.min(...[1, 2, 3]); // -> 1
```
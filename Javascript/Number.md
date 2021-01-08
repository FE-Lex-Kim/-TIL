# Number

- 출처 [모던 자바스크립트 Deep Dive](http://www.yes24.com/Product/Goods/92742567?OzSrank=1)을 보고 정리한 내용입니다.

<br>

## 1. Number 생성자 함수

<br>

표준 빌트인 객체인 Number 객체는 생성자 함수 객체다.

따라서 new 연산자와 함께 호출하여 Number 인스턴스를 생성할 수 있다.

<br>

Number 생성자 함수에 인수를 전달하지 않고 new 연산자와 함께 호출하면 [[NumberData]] 내부 슬롯에 0을 할당한 Number 래퍼 객체를 생성한다.

```jsx
const numObj = new Number();
console.log(numObj); // Number {[[PrimitiveValue]]: 0}
```

<br>

Number 생성자 함수의 인수로 숫자를 전달하면서 new 연산자와 함께 호출하면 [[NumberData]] 내부 슬롯에 인수로 전달받은 숫자를 할당한 Number 래퍼 객체를 생성한다.

```jsx
const numObj = new Number(10);
console.log(numObj); // Number {[[PrimitiveValue]]: 10}
```

<br>

인수로 숫자가 아닌 값을 전달하면 인수를 숫자로 강제 변환한 후,

[[NumberData]] 내부 슬롯에 변환된 숫자를 할당한 Number 래퍼 객체를 생성한다.

인수를 숫자로 변환할 수 없다면 NaN을 [[NumberData]] 내부 슬롯에 할당한 Number 래퍼 객체를 생성한다.

```jsx
let numObj = new Number('10');
console.log(numObj); // Number {[[PrimitiveValue]]: 10}

numObj = new Number('Hello');
console.log(numObj); // Number {[[PrimitiveValue]]: NaN}
```

<br>

Number 생성자 함수를 호출하면 Number 인스턴스가 아닌 숫자를 반환한다.

```jsx
// 문자열 타입 => 숫자 타입
Number('0');     // -> 0
Number('-1');    // -> -1
Number('10.53'); // -> 10.53

// 불리언 타입 => 숫자 타입
Number(true);  // -> 1
Number(false); // -> 0
```

<br>

## 2. Number 프로퍼티

<br>

## 2.1.Number.EPSILON

<br>

## 2.2. Number.MAX_VALUE

<br>

Number.MAX_VALUE는 자바스크립트에서 표현할 수 있는 가장 큰 양수 값(1.7976931348623157 x 10308)이다. Number.MAX_VALUE보다 큰 숫자는 Infinity다.

```jsx
Number.MAX_VALUE; // -> 1.7976931348623157e+308
Infinity > Number.MAX_VALUE; // -> true
```

<br>

## 2.3. Number.MIN_VALUE

<br>

Number.MIN_VALUE는 자바스크립트에서 표현할 수 있는 가장 작은 양수 값(5 x 10-324)이다. Number.MIN_VALUE보다 작은 숫자는 0이다.

```jsx
Number.MIN_VALUE; // -> 5e-324
Number.MIN_VALUE > 0; // -> true
```

<br>

## 2.4. Number.MAX_SAFE_INTEGER

<br>

Number.MAX_SAFE_INTEGER 자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수값(9007199254740991)이다.

```jsx
Number.MAX_SAFE_INTEGER; // -> 9007199254740991
```

<br>

## 2.5. Number.MIN_SAFE_INTEGER

<br>

Number.MIN_SAFE_INTEGER는 자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수값(-9007199254740991)이다.

```jsx
Number.MIN_SAFE_INTEGER; // -> -9007199254740991
```

<br>

## 2.6. Number.POSITIVE_INFINITY

<br>

Number.POSITIVE_INFINITY)는 양의 무한대를 나타내는 숫자 값 Infinity와 같다.

```jsx
Number.POSITIVE_INFINITY; // -> Infinity
```

<br>

## 2.7. Number.NEGATIVE_INFINITY

<br>

Number.NEGATIVE_INFINITY는 음의 무한대를 나타내는 숫자 값 -Infinity와 같다.

```jsx
Number.NEGATIVE_INFINITY; // -> -Infinity
```

<br>

## 2.8. Number.NaN

<br>

Number.NaN은 숫자가 아님(Not-a-Number)을 나타내는 숫자 값이다. Number.NaN은 window.NaN과 같다.

```jsx
Number.NaN; // -> NaN
```

<br>

## 3. Number 메서드

<br>

### 3.1. Number.isFinite

<br>

infinity 또는 -Infinity가 아닌지 검사하여 그 결과를 불리언 값으로 반환한다.

```jsx
// 인수가 정상적인 유한수이면 true를 반환한다.
Number.isFinite(0);                // -> true
Number.isFinite(Number.MAX_VALUE); // -> true
Number.isFinite(Number.MIN_VALUE); // -> true

// 인수가 무한수이면 false를 반환한다.
Number.isFinite(Infinity);  // -> false
Number.isFinite(-Infinity); // -> false
```

<br>

만약 인수가 NaN이면 언제나 false를 반환한다.

```jsx
Number.isFinite(NaN); // -> false
```

<br>

Number.isFinite는 전달받은 인수를 숫자로 암묵적 타입 변환하지 않는다. 따라서 숫자가 아닌 인수가 주어졌을 때 반환값은 언제나 false다.

```jsx
// Number.isFinite는 인수를 숫자로 암묵적 타입 변환하지 않는다.
Number.isFinite(null); // -> false

// isFinite는 인수를 숫자로 암묵적 타입 변환한다. null은 0으로 암묵적 타입 변환된다.
isFinite(null); // -> true
```

<br>

### 3.2. Number.isInteger

<br>

Number.isInteger 정적 메서드는 인수로 전달된 숫자값이 정수(integer)인지 검사하여 그 결과를 불리언 값으로 반환한다.

```jsx
// 인수가 정수이면 true를 반환한다.
Number.isInteger(0)     // -> true
Number.isInteger(123)   // -> true
Number.isInteger(-123)  // -> true

// 0.5는 정수가 아니다.
Number.isInteger(0.5)   // -> false
// '123'을 숫자로 암묵적 타입 변환하지 않는다.
Number.isInteger('123') // -> false
// false를 숫자로 암묵적 타입 변환하지 않는다.
Number.isInteger(false) // -> false
// Infinity/-Infinity는 정수가 아니다.
Number.isInteger(Infinity)  // -> false
Number.isInteger(-Infinity) // -> false
```

<br>

### 3.3. Number.isNaN

<br>

ES6에서 도입된 Number.isNaN 정적 메서드는 인수로 전달된 숫자값이 NaN인지 검사하여 그 결과를 불리언 값으로 반환한다.

```jsx
// 인수가 NaN이면 true를 반환한다.
Number.isNaN(NaN); // -> true
```

<br>

빌트인 전역 함수 isNaN은 전달받은 인수를 숫자로 암묵적 타입 변환하여 검사를 수행하지만 Number.isNaN 메서드는 전달받은 인수를 숫자로 암묵적 타입 변환하지 않는다.

```jsx
// Number.isNaN은 인수를 숫자로 암묵적 타입 변환하지 않는다.
Number.isNaN(undefined); // -> false

// isFinite는 인수를 숫자로 암묵적 타입 변환한다. undefined는 NaN으로 암묵적 타입 변환된다.
isNaN(undefined); // -> true
```

<br>

### 3.4. Number.isSafeInteger

<br>

Number.isSafeInteger 정적 메서드는 인수로 전달된 숫자값이 안전한 정수인지 검사하여 그 결과를 불리언 값으로 반환한다.

```jsx
// 0은 안전한 정수이다.
Number.isSafeInteger(0); // -> true
// 1000000000000000은 안전한 정수이다.
Number.isSafeInteger(1000000000000000); // -> true

// 10000000000000001은 안전하지 않다.
Number.isSafeInteger(10000000000000001); // -> false
// 0.5은 정수가 아니다.
Number.isSafeInteger(0.5); // -> false
// '123'을 숫자로 암묵적 타입 변환하지 않는다.
Number.isSafeInteger('123'); // -> false
// false를 숫자로 암묵적 타입 변환하지 않는다.
Number.isSafeInteger(false); // -> false
// Infinity/-Infinity는 정수가 아니다.
Number.isSafeInteger(Infinity); // -> false
```

<br>

### 3.5. Number.prototype.toExponential

<br>

toExponential 메서드는 숫자를 지수 표기법으로 변환하여 문자열로 반환한다. 

지수 표기법이란 매우 크거나 작은 숫자를 표기할 때 주로 사용하며 e(exponent) 앞에 있는 숫자에 10의 n승을 곱하는 형식으로 수를 나타내는 방식이다.

```jsx
(77.1234).toExponential();  // -> "7.71234e+1"
(77.1234).toExponential(4); // -> "7.7123e+1"
(77.1234).toExponential(2); // -> "7.71e+1"
```

<br>

다음과 같이 숫자 리터럴과 함께 Number 프로토타입 메서드를 사용할 경우 에러가 발생한다.

```jsx
77.toExponential(); // -> SyntaxError: Invalid or unexpected token
```

<br>

```jsx
77.1234.toExponential(); // -> "7.71234e+1"
```

.은 명백하게 부동 소수점 숫자의 소수 구분 기호다. 숫자에 소수점은 하나만 존재하므로 두 번째 .은 프로퍼티 접근 연산자로 해석된다. 

따라서 숫자 리터럴과 함께 메서드를 사용할 경우 혼란을 방지하기 위해 그룹 연산자를 사용해야한다.

```jsx
(77).toExponential(); // -> "7.7e+1"
```

<br>

### 3.6. Number.prototype.toFixed

<br>

toFixed 메서드는 숫자를 반올림하여 문자열로 반환한다.

```jsx
// 소수점 이하 반올림. 인수를 생략하면 기본값 0이 지정된다.
(12345.6789).toFixed(); // -> "12346"
// 소수점 이하 1자리수 유효, 나머지 반올림
(12345.6789).toFixed(1); // -> "12345.7"
// 소수점 이하 2자리수 유효, 나머지 반올림
(12345.6789).toFixed(2); // -> "12345.68"
// 소수점 이하 3자리수 유효, 나머지 반올림
(12345.6789).toFixed(3); // -> "12345.679"
```

<br>

### 3.7. Number.prototype.toPrecision

<br>

toPrecision 메서드는 인수로 전달받은 전체 자리수까지 유효하도록 나머지 자리수를 반올림하여 문자열로 반환한다.

```jsx
// 전체 자리수 유효. 인수를 전달하지 않으면 기본값 0이 전달된다.
(12345.6789).toPrecision(); // -> "12345.6789"
// 전체 1자리수 유효, 나머지 반올림
(12345.6789).toPrecision(1); // -> "1e+4"
// 전체 2자리수 유효, 나머지 반올림
(12345.6789).toPrecision(2); // -> "1.2e+4"
// 전체 6자리수 유효, 나머지 반올림
(12345.6789).toPrecision(6); // -> "12345.7"
```

<br>

### 3.8. Number.prototype.toString

<br>

toString 메서드는 숫자를 문자열로 변환하여 반환한다.

```jsx
// 인수를 생략하면 10진수 문자열을 반환한다.
(10).toString(); // -> "10"
// 2진수 문자열을 반환한다.
(16).toString(2); // -> "10000"
// 8진수 문자열을 반환한다.
(16).toString(8); // -> "20"
// 16진수 문자열을 반환한다.
(16).toString(16); // -> "10"
```
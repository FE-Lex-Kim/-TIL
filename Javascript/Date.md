# Date

<br>

표준 빌트인 객체인 Date는 날짜와 시간(년, 월, 일, 시, 분, 초, 밀리초(millisecond/ms. 천분의 1초))을 위한 메서드를 제공하는 빌트인 객체이면서 생성자 함수다.

<br>

UTC(협정 세계시, Coordinated Universal Time) 는 국제 표준시를 말한다.

KST(한국 표준시, Korea Standard Time)는 UTC에 9시간을 더한 시간이다.

<br>

현재 날짜와 시간은 자바스크립트 코드가 실행된 시스템의 시계에 의해 결정된다.

<br>

## 1. Date 생성자 함수

<br>

Date는 생성자 함수다. 

Date 생성자 함수로 생성한 Date 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 갖는다.

<br>

이 값은 1970년 1월 1일 00:00:00(UTC)을 기점으로 Date 객체가 나타내는 날짜와 시간까지의 밀리초를 나타낸다.

<br>

Date 생성자 함수로 객체를 생성하는 방법은 다음과 같이 4가지가 있다.

<br>

### 1.1. new Date()

<br>

Date 생성자 함수를 인수 없이 new 연산자와 함께 호출하면 현재 날짜와 시간을 가지는 Date **객체를 반환**한다.

```jsx
new Date(); // -> Mon Jul 06 2020 01:03:18 GMT+0900 (대한민국 표준시)
```

<br>

Date 생성자 함수를 new 연산자 없이 호출하면 Date 객체를 반환하지 않고 날짜와 시간 정보를 나타내는 **문자열을 반환**한다.

```jsx
Date(); // -> "Mon Jul 06 2020 01:10:47 GMT+0900 (대한민국 표준시)"
```

<br>

### 1.2. new Date(milliseconds)

<br>

Date 생성자 함수에 숫자 타입의 밀리초를 인수로 전달하면 1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 밀리초만큼 경과한 날짜와 시간을 나타내는 Date 객체를 반환한다.

```jsx
// 한국 표준시 KST는 협정 세계시 UTC에 9시간을 더한 시간이다.
new Date(0); // -> Thu Jan 01 1970 09:00:00 GMT+0900 (대한민국 표준시)

/*
86400000ms는 1day를 의미한다.
1s = 1,000ms
1m = 60s * 1,000ms = 60,000ms
1h = 60m * 60,000ms = 3,600,000ms
1d = 24h * 3,600,000ms = 86,400,000ms
*/
new Date(86400000); // -> Fri Jan 02 1970 09:00:00 GMT+0900 (대한민국 표준시)
```

<br>

### 1.3. new Date(dateString)

<br>

Date 생성자 함수에 날짜와 시간을 나타내는 문자열을 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다.

```jsx
new Date('May 26, 2020 10:00:00');
// -> Tue May 26 2020 10:00:00 GMT+0900 (대한민국 표준시)

new Date('2020/03/26/10:00:00');
// -> Thu Mar 26 2020 10:00:00 GMT+0900 (대한민국 표준시)
```

<br>

### 1.4. new Date(your, month[, day, hour, minute, second, millisecond])

<br>

Date 생성자 함수에 연, 월, 일, 시, 분, 초, 밀리초를 의미하는 숫자를 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다.

연, 월은 반드시 지정해야 한다.

지정하지 않은 옵션 정보는 0 또는 1으로 초기화된다.

```jsx
// 월을 나타내는 2는 3월을 의미한다. 2020/3/1/00:00:00:00
new Date(2020, 2);
// -> Sun Mar 01 2020 00:00:00 GMT+0900 (대한민국 표준시)

// 월을 나타내는 2는 3월을 의미한다. 2020/3/26/10:00:00:00
new Date(2020, 2, 26, 10, 00, 00, 0);
// -> Thu Mar 26 2020 10:00:00 GMT+0900 (대한민국 표준시)

// 다음처럼 표현하면 가독성이 훨씬 좋다.
new Date('2020/3/26/10:00:00:00');
// -> Thu Mar 26 2020 10:00:00 GMT+0900 (대한민국 표준시)
```

<br>

## 2. Date 메서드

### 2.1 Date.now

1970년 1월 1일 00:00:00(UTC)을 기점으로 현재 시간까지 경과한 밀리초를 숫자로 반환한다.

```jsx
Date.now(); // -> 1593971539112
```

<br>

### 2.2. Date.parse

1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 지정 시간(new Date(dateString)의 인수와 동일한 형식)까지의 밀리초를 숫자로 반환한다.

```jsx
// UTC
Date.parse('Jan 2, 1970 00:00:00 UTC'); // -> 86400000

// KST
Date.parse('Jan 2, 1970 09:00:00'); // -> 86400000

// KST
Date.parse('1970/01/02/09:00:00');  // -> 86400000
```

<br>

### 2.3. Date.UTC

1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환한다.

```jsx
Date.UTC(1970, 0, 2); // -> 86400000
Date.UTC('1970/1/2'); // -> NaN
```

<br>

### 2.4. Date.prototype.getFullYear

Date 객체의 연도를 나타내는 정수를 반환한다.

```jsx
new Date('2020/07/24').getFullYear(); // -> 2020
```

<br>

### 2.5. Date.prototype.setFullYear

Date 객체에 연도를 나타내는 정수를 설정한다. 

연도 이외에 옵션으로 월, 일도 설정할 수 있다.

```jsx
const today = new Date();

// 년도 지정
today.setFullYear(2000);
today.getFullYear(); // -> 2000

// 년도/월/일 지정
today.setFullYear(1900, 0, 1);
today.getFullYear(); // -> 1900
```

<br>

### 2.6. Date.prototype.getMonth

Date 객체의 월을 나타내는 0 ~ 11의 정수를 반환한다. 

1월은 0, 12월은 11이다.

```jsx
new Date('2020/07/24').getMonth(); // -> 6
```

<br>

### 2.7. Date.prototype.setMonth

Date 객체에 월을 나타내는 0 ~ 11의 정수를 설정한다. 1월은 0, 12월은 11이다. 월 이외에 옵션으로 일도 설정할 수 있다.

```jsx
const today = new Date();

// 월 지정
today.setMonth(0); // 1월
today.getMonth(); // -> 0

// 월/일 지정
today.setMonth(11, 1); // 12월 1일
today.getMonth(); // -> 11
```

<br>

### 2.8. Date.prototype.getDate

Date 객체의 날짜(1 ~ 31)를 나타내는 정수를 반환한다.

```jsx
new Date('2020/07/24').getDate(); // -> 24
```

<br>

### 2.9. Date.prototype.setDate

Date 객체에 날짜(1 ~ 31)를 나타내는 정수를 설정한다.

```jsx
const today = new Date();

// 날짜 지정
today.setDate(1);
today.getDate(); // -> 1
```

<br>

### 2.10. Date.prototype.getDay

Date 객체의 요일(0 ~ 6)을 나타내는 정수를 반환한다. 반환값은 다음과 같다.

```jsx
new Date('2020/07/24').getDay(); // -> 5
```

<br>

### 2.11. Date.prototype.getHours

Date 객체의 시간(0 ~ 23)를 나타내는 정수를 반환한다.

```jsx
new Date('2020/07/24/12:00').getHours(); // -> 12
```

<br>

### 2.12. Date.prototype.setHours

Date 객체에 시간(0 ~ 23)을 나타내는 정수를 설정한다. 시간 이외에 옵션으로 분, 초, 밀리초도 설정할 수 있다.

```jsx
const today = new Date();

// 시간 지정
today.setHours(7);
today.getHours(); // -> 7

// 시간/분/초/밀리초 지정
today.setHours(0, 0, 0, 0); // 00:00:00:00
today.getHours(); // -> 0
```

<br>

### 2.13. Date.prototype.getMinutes

Date 객체의 분(0 ~ 59)를 나타내는 정수를 반환한다.

```jsx
new Date('2020/07/24/12:30').getMinutes(); // -> 30
```

<br>

### 2.14 Date.prototype.setMinutes

Date 객체에 분(0 ~ 59)를 나타내는 정수를 설정한다. 분 이외에 옵션으로 초, 밀리초도 설정할 수 있다.

```jsx
const today = new Date();

// 분 지정
today.setMinutes(50);
today.getMinutes(); // -> 50

// 분/초/밀리초 지정
today.setMinutes(5, 10, 999); // HH:05:10:999
today.getMinutes(); // -> 5
```

<br>

### 2.15. Date.prototype.getSeconds

Date 객체의 초(0 ~ 59)를 나타내는 정수를 반환한다.

```jsx
new Date('2020/07/24/12:30:10').getSeconds(); // -> 10
```

<br>

### 2.16 Date.prototype.setSeconds

Date 객체에 초(0 ~ 59)를 나타내는 정수를 설정한다. 초 이외에 옵션으로 밀리초도 설정할 수 있다.

```jsx
const today = new Date();

// 초 지정
today.setSeconds(30);
today.getSeconds(); // -> 30

// 초/밀리초 지정
today.setSeconds(10, 0); // HH:MM:10:000
today.getSeconds(); // -> 10
```

<br>

### 2.17. Date.prototype.getMilliseconds

Date 객체의 밀리초(0 ~ 999)를 나타내는 정수를 반환한다.

```jsx
new Date('2020/07/24/12:30:10:150').getMilliseconds(); // -> 150
```

<br>

### 2.18. Date.prototype.setMilliseconds

Date 객체에 밀리초(0 ~ 999)를 나타내는 정수를 설정한다.

```jsx
const today = new Date();

// 밀리초 지정
today.setMilliseconds(123);
today.getMilliseconds(); // -> 123
```

<br>

### 2.19. Date.prototype.getTime

1970년 1월 1일 00:00:00(UTC)를 기점으로 Date 객체의 시간까지 경과된 밀리초를 반환한다.

```jsx
new Date('2020/07/24/12:30').getTime(); // -> 1595561400000
```

<br>

### 2.20. Date.prototype.setTime

Date 객체에 1970년 1월 1일 00:00:00(UTC)를 기점으로 경과된 밀리초를 설정한다.

```jsx
const today = new Date();

// 1970년 1월 1일 00:00:00(UTC)를 기점으로 경과된 밀리초 설정
today.setTime(86400000); // 86400000는 1day를 나타낸다.
console.log(today); // -> Fri Jan 02 1970 09:00:00 GMT+0900 (대한민국 표준시)
```

<br>

### 2.21. Date.prototype.getTimezoneOffset

UTC와 Date 객체에 지정된 로캘(locale) 시간과의 차이를 분 단위로 반환한다. KST는 UTC에 9시간을 더한 시간이다. 즉, UTC = KST - 9h다.

```jsx
const today = new Date(); // today의 지정 로캘은 KST다.

//UTC와 today의 지정 로캘 KST와의 차이는 -9시간이다.
today.getTimezoneOffset() / 60; // -9
```

<br>

### 2.22. Date.prototype.toDateString

사람이 읽을 수 있는 형식의 문자열로 Date 객체의 날짜를 반환한다.

```jsx
const today = new Date('2020/7/24/12:30');

today.toString();     // -> Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toDateString(); // -> Fri Jul 24 2020
```

<br>

### 2.23. Date.prototype.toTimeString

사람이 읽을 수 있는 형식으로 Date 객체의 시간을 표현한 문자열을 반환한다.

```jsx
const today = new Date('2020/7/24/12:30');

today.toString();     // -> Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toTimeString(); // -> 12:30:00 GMT+0900 (대한민국 표준시)
```

<br>

### 2.24. Date.prototype.toISOString

ISO 8601 형식으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환한다.

```jsx
const today = new Date('2020/7/24/12:30');

today.toString();    // -> Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toISOString(); // -> 2020-07-24T03:30:00.000Z

today.toISOString().slice(0, 10); // -> 2020-07-24
today.toISOString().slice(0, 10).replace(/-/g, ''); // -> 20200724
```

<br>

### 2.25. Date.prototype.toLocaleString

인수로 전달한 로캘을 기준으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환한다. 인수를 생략한 경우 브라우저가 동작 중인 시스템의 로캘을 적용한다.

```jsx
const today = new Date('2020/7/24/12:30');

today.toString(); // -> Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toLocaleString(); // -> 2020. 7. 24. 오후 12:30:00
today.toLocaleString('ko-KR'); // -> 2020. 7. 24. 오후 12:30:00
today.toLocaleString('en-US'); // -> 7/24/2020, 12:30:00 PM
today.toLocaleString('ja-JP'); // -> 2020/7/24 12:30:00
```

<br>

### 2.26. Date.prototype.toLocaleTimeString

인수로 전달한 로캘을 기준으로 Date 객체의 시간을 표현한 문자열을 반환한다. 인수를 생략한 경우 브라우저가 동작 중인 시스템의 로캘을 적용한다.

```jsx
const today = new Date('2020/7/24/12:30');

today.toString(); // -> Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toLocaleTimeString(); // -> 오후 12:30:00
today.toLocaleTimeString('ko-KR'); // -> 오후 12:30:00
today.toLocaleTimeString('en-US'); // -> 12:30:00 PM
today.toLocaleTimeString('ja-JP'); // -> 12:30:00
```

정리

```js
// 지난만큼 밀리초를 반환한다.
Date.parse() // 70년도에서 string 인수 지난만큼 (문자열)
Date.UTC() // 70년도에서 숫자 인수 지난만큼 (숫자)
Date.now() // 70년도에서 지금 지난만큼 (지금)

// 70년을 기준으로 객체의 시간까지 경과된 만큼 밀리초 반환
new Date().getTime(); // (객체가 지난만큼)

// 객체를 문자열로 반환
new Date().toDateString(); // 날짜까지 표현
new Date().toTimeString(); // 시간까지 표현

// 나머지 get,set뒤는 그 의미에 맞게 반환 ex)
new Date().getDate();

```
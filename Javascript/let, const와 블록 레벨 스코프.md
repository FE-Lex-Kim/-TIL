# let, const와 블록 레벨 스코프

- 출처 [모던 자바스크립트 Deep Dive](http://www.yes24.com/Product/Goods/92742567?OzSrank=1)을 보고 정리한 내용입니다.

[Notion](https://www.notion.so/15-let-const-ce540b8b8d2649418b1159741ca594de)

<br>

## 1. var 키워드로 선언한 변수의 문제점

<br>

ES5까지 변수를 선언할 수 있는 유일한 방법은 

var 키워드를 사용하는 것이었다.

<br>

var 키워드는 심각한 문제들이 많다.

<br>

**What? 어떤 문제점들이 있을까?**

<br>

### 1.1. 변수 중복 선언 허용

<br>

```jsx
var x = 1;
var y = 1;

// var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
// 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
var x = 100;
// 초기화문이 없는 변수 선언문은 무시된다.
var y;

console.log(x); // 100
console.log(y); // 1
```

<br>

var 키워드는 **중복 선언이 가능해**

긴 코드를 작성시 이미 선언 되었는지 모르고

다시 **선언하는 실수 발생 시키는 확률이 높다.**

<br>

### 1.2. 함수 레벨 스코프

<br>

var키워드는 **함수 레벨 스코프**로

**다른 블록에서는 전역변수로 선언**되어

무분별한 전역변수가 생겨 

**중복 선언 가능성이 높아진다.**

<br>

```jsx
var x = 1;

if (true) {
  // x는 전역 변수다. 이미 선언된 전역 변수 x가 있으므로 x 변수는 중복 선언된다.
  // 이는 의도치 않게 변수값이 변경되는 부작용을 발생시킨다.
  var x = 10;
}

console.log(x); // 10
```

<br>

for문, if문 코드 블럭안에 모두 전역변수로 선언된다.

<br>

```jsx
var i = 10;

// for문에서 선언한 i는 전역 변수이다. 이미 선언된 전역 변수 i가 있으므로 중복 선언된다.
for (var i = 0; i < 5; i++) {
  console.log(i); // 0 1 2 3 4
}

// 의도치 않게 i 변수의 값이 변경되었다.
console.log(i); // 5
```

<br>

### 1.3. 변수 호이스팅

<br>

var키워드는 **호이스팅이 발생해**

프로그램 흐름상 맞지 않고

**가독성을 떨어트린다.**

<br>

```jsx
// 이 시점에는 변수 호이스팅에 의해 이미 foo 변수가 선언되었다(1. 선언 단계)
// 변수 foo는 undefined로 초기화된다. (2. 초기화 단계)
console.log(foo); // undefined

// 변수에 값을 할당(3. 할당 단계)
foo = 123;

console.log(foo); // 123

// 변수 선언은 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 실행된다.
var foo;
```

<br>
<br>

## 2. let 키워드

<br>

var키워드의 **단점을 보안하기 위해**

**ES6에서 let 과 const 키워드를 도입했다.**

<br>

### 2.1. 변수 중복 선언 금지

<br>

var 키워드는 **중복선언이 가능해**

**의도치 않은 재할당**이 되어 값이 변경 되는 부작용이 있었다.

<br>

```jsx
var foo = 123;
// var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
// 아래 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
var foo = 456;

let bar = 123;
// let이나 const 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용하지 않는다.
let bar = 456; // SyntaxError: Identifier 'bar' has already been declared
```

<br>

**하지만!**

**let은 중복선언시 문법에러(SyntaxError)** 가 발생한다.

<br>

### 2.2. 블록 레벨 스코프

<br>

var키워드는 함수 레벨 스코프로

전역변수가 많이 생기게 되었다.

<br>

**하지만!**

let은 블록 레벨 스코프로

모든 코드 블록을 지역스코프로 인정해준다.

```jsx
let foo = 1; // 전역 변수

{
  let foo = 2; // 지역 변수
  let bar = 3; // 지역 변수
}

console.log(foo); // 1
console.log(bar); // ReferenceError: bar is not defined
```

<br>

**예제풀이)**

foo는 

전역변수로 참조가 가능하고

지역변수는 참조가 안된다.

<br>

bar 변수도 let키워드로 **블록 레벨 스코프**라

지역변수가 되어 **전역에서 참조가 안된다.**

<br>

### 2.3. 변수 호이스팅

<br>

let 키워드는 변수 호이스팅이 발생하지

않는 것 처럼 동작한다.

```jsx
console.log(foo); // ReferenceError: foo is not defined
let foo;
```

<br>

참조시 에러가 발생한다.

<br>

**복습! var키워드의 호이스팅 동작원리!**

var키워드는 선언시

**런타임 이전에 "선언단계" 와 "초기화 단계" 가**

**한번에 진행된다.**

<br>

따라서 선언문 이전에 참조해도

undefined 가 반환된다.

```jsx
// var 키워드로 선언한 변수는 런타임 이전에 선언 단계와 초기화 단계가 실행된다.
// 따라서 변수 선언문 이전에 변수를 참조할 수 있다.
console.log(foo); // undefined

var foo;
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행된다.
console.log(foo); // 1
```

<br>

**그렇다면 let 키워드는 호이스팅이 일어날까?**

**let 키워드는 "선언단계" 와 "초기화 단계" 가**

**분리되어 진행된다.**

<br>

**선언 단계는 런타임 이전에**

**초기화 단계는 선언문에 도달 했을때 실행 된다.**

<br>

**따라서!**

**스코프의 시작 지점** 부터

**선언문에 도달해 초기화 단계 까지**

변수를 참조 할 수 없는 구간을

**일시적 사각지대(Temporal Dead Zone; TDZ)[용어]** 라 부른다.

```jsx
// 런타임 이전에 선언 단계가 실행된다. 아직 변수가 초기화되지 않았다.
// 초기화 이전의 일시적 사각 지대에서는 변수를 참조할 수 없다.
console.log(foo); // ReferenceError: foo is not defined

let foo; // 변수 선언문에서 초기화 단계가 실행된다.
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행된다.
console.log(foo); // 1
```

<br>

**하지만!**

호이스팅이 발생하지 않아 보이지만...

```jsx
let foo = 1; // 전역 변수

{
  console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
  let foo = 2; // 지역 변수
}
```

<br>

**예제풀이)**

호이스팅이 발생하지 않으면

전역변수 foo가 값을 출력해야한다.

<br>

**하지만!**

let키워드로 선언한 변수도 여전히

**호이스팅이 발생해 참조에러(ReferenceError)가 발생**한다.

<br>

**즉,**

let, const를 포함

**모든 선언문(var, let, const, function, function*, class 등)을 호이스팅**을 한다.

<br>

→ let, const, class는 호이스팅이 발생하지 않는 것처럼만 동작한다.

<br>

### 2.4. 전역 객체와 let

<br>

**var키워드로 선언된 변수**는

**전역객체 window의 프로퍼티**가 되고

<br>

전역객체의 프로퍼티 참조시 window 는 생략 가능하다.

```jsx
// 이 예제는 브라우저 환경에서 실행해야 한다.

// 전역 변수
var x = 1;
// 암묵적 전역
y = 2;
// 전역 함수
function foo() {}

// var 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티다.
console.log(window.x); // 1
// 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
console.log(x); // 1

// 암묵적 전역은 전역 객체 window의 프로퍼티다.
console.log(window.y); // 2
console.log(y); // 2

// 함수 선언문으로 정의한 전역 함수는 전역 객체 window의 프로퍼티다.
console.log(window.foo); // ƒ foo() {}
// 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
console.log(foo); // ƒ foo() {}
```

<br>

**But!**

**let 키워드**로 선언한 전역변수는

**전역객체의 프로퍼티가 아니다.**

<br>

**그렇다면 어디에 존재할까?**

let 전역변수는 보이지 않는

개념적인 블록내에 존재한다.

<br>
<br>

## 3. const 키워드

<br>

let 키워드와 다른점.

<br>

### 3.1. 선언과 초기화

<br>

**const키워드로 선언한 변수는 반드시 선언과**

**동시에 초기화 해야한다.**

```jsx
const foo = 1;
```

<br>

그렇지 않으면 에러가 발생한다.

```jsx
const foo; // SyntaxError: Missing initializer in const declaration
```

<br>

const 키워드 또한

- **블록 레벨 스코프**를 가지며
- **변수 호이스팅**이 발생하지 않는 것처럼 동작한다.

```jsx
{
  // 변수 호이스팅이 발생하지 않는 것처럼 동작한다
  console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
  const foo = 1;
  console.log(foo); // 1
}

// 블록 레벨 스코프를 갖는다.
console.log(foo); // ReferenceError: foo is not defined
```

<br>

### 3.2. 재할당 금지

<br>

**const는** var, let 키워드와 다르게

**'재할당' 이 금지된다.**

```jsx
const foo = 1;
foo = 2; // TypeError: Assignment to constant variable.
```

<br>

### 3.3. 상수

<br>

const는 **재할당이 금지**되므로

**원시값을 할당 한 경우 값 변경이 안된다.**

<br>

**이러한 특징으로!**

**const 키워드를 상수로 표현**하는데 사용한다.

<br>

**상수**는 **재할당이 금지된 변수**를 말한다.

<br>

**What? 상수의 장점은 무엇일까?**

상수는

- **상태 유지**
- **가독성**
- **유지보수의 편의**

가 좋아 적극적으로 사용해야 한다.

<br>

```jsx
// 세전 가격
let preTaxPrice = 100;

// 세후 가격
// 0.1의 의미를 명확히 알기 어렵기 때문에 가독성이 좋지 않다.
let afterTaxPrice = preTaxPrice + (preTaxPrice * 0.1);

console.log(afterTaxPrice); // 110
```

<br>

**예제풀이)**

코드내에 0.1은 어떤 값으로

사용 한지 명확하게 하기 어렵다.

<br>

**쉽게 바뀌지 않는 값으로,** 세율을 의미 하므로

**상수로 정의**하면 **가독성이 상승한다.**

<br>

**즉!**

**const키워드**로 선언된 변수에

**원시값 할당시** 할당 값이 **변경 가능한 방법이 없다.**

<br>

이로인해 프로그램 전체에서 공통적으로

사용하므로 **유지보수성이 대폭 향상된다.**

<br>

**주의!**

상수는 

- **대문자로 선언**
- **스네이크 케이스로 표현**

상수임을 명확히 나타내야 한다.

```jsx
// 세율을 의미하는 0.1은 변경할 수 없는 상수로서 사용될 값이다.
// 변수 이름을 대문자로 선언해 상수임을 명확히 나타낸다.
const TAX_RATE = 0.1;

// 세전 가격
let preTaxPrice = 100;

// 세후 가격
let afterTaxPrice = preTaxPrice + (preTaxPrice * TAX_RATE);

console.log(afterTaxPrice); // 110
```

<br>

### 3.4. const 키워드와 객체

const 키워드로 선언된 변수에

**객체를 할당한 경우,**

**값을 변경 할 수 있다.**

<br>

**How? 어떻게 const 키워드로 값을 변경 가능할까?**

재할당이 금지된 const 키워드 이지만

<br>

**객체는 재할당 없이도 직접 변경이 가능**하기 때문이다.

```jsx
const person = {
  name: 'Lee'
};

// 객체는 변경 가능한 값이다. 따라서 재할당없이 변경이 가능하다.
person.name = 'Kim';

console.log(person); // {name: "Kim"}
```

<br>

**즉,**

const 키워드는 **재할당을 금지**할뿐

"**불변(immutable)"** 을 의미하지 않는다.

<br>

이때 객체가 변경하더라도

변수의 **참조값은 변경 되지 않는다.**

<br>

## 4. var vs. let vs. const

<br>

변수 선언시 **기본적으로 const를 사용**하고

<br>

**let은 재할당이 필요한 경우** 한정해 사용한다.

<br>

**Why? const 키워드를 사용 권장 할까?**

const 키워드 사용으로 

**의도치 않은 재할당을 방지** 하기 위해서 이다.

<br>

**var, let, const 다음과 같이 사용을 권장한다.**

- ES6를 사용한다면 **var 사용 금지.**

- **재할당이 필요한 경우**에만, **let 사용**

→ 이때 주의! **변수 스코프는 최대한 좁게** 만든다.

- **재할당이 필요없는 원시값과 객체에는** **const 키워드를 사용**한다.

<br>

**즉!**

변수 선언 시점에는 재할당 할지 모르는 경우가 많으므로,

<br>

**const 키워드를 사용하고**

**재할당이 필요하면 let으로 교체하여 사용**한다.


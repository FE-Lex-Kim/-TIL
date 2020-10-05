# ES6 함수의 추가 기능

<br>

## 1. 함수의 구분

<br>

ES6 이전의 함수는 동일한 함수라도 다양한 형태로 호출할 수 있다.

```jsx
var foo = function () {
  return 1;
};

// 일반적인 함수로서 호출
foo(); // -> 1

// 생성자 함수로서 호출
new foo(); // -> foo {}

// 메서드로서 호출
var obj = { foo: foo };
obj.foo(); // -> 1
```

<br>

이처럼 ES6 이전의 함수는 사용 목적에 따라 명확히 구분되지 않는다.

**ES6 이전의 모든 함수는 일반 함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출할 수 있다.**

<br>

```jsx
var foo = function () {};

// ES6 이전의 모든 함수는 callable이면서 constructor다.
foo(); // -> undefined
new foo(); // -> foo {}
```

<br>

ES6 이전에 객체에 바인딩된 함수도 일반 함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출할 수도 있다.

```jsx
// 프로퍼티 f에 바인딩된 함수는 callable이며 constructor다.
var obj = {
  x: 10,
  f: function () { return this.x; }
};

// 프로퍼티 f에 바인딩된 함수를 메서드로서 호출
console.log(obj.f()); // 10

// 프로퍼티 f에 바인딩된 함수를 일반 함수로서 호출
var bar = obj.f;
console.log(bar()); // undefined

// 프로퍼티 f에 바인딩된 함수를 생성자 함수로서 호출
console.log(new obj.f()); // f {}
```

<br>

객체에 바인딩된 함수를 생성자 함수로 호출하는 경우가 흔치는 않겠지만 문법상 가능하다는 것은 문제가 있다.

<br>

콜백 함수도 constructor이기 때문에 불필요한 프로토타입 객체를 생성한다.

```jsx
// 콜백 함수를 사용하는 고차 함수 map. 콜백 함수도 constructor이며 프로토타입을 생성한다.
[1, 2, 3].map(function (item) {
  return item * 2;
}); // -> [ 2, 4, 6 ]
```

<br>

ES6 이전의 모든 함수는 사용 목적에 따라 명확한 구분이 없으므로

혼란스러우며 실수를 유발할 가능성이 있고 성능에도 좋지 않다.

<br>

문제를 해결하기 위해 ES6에서는 함수를 사용 목적에 따라 세 가지 종류로 명확히 구분했다.

<br>

일반 함수는 함수 선언문이나 함수 표현식으로 정의한 함수를 말하며, ES6 이전의 함수와 차이가 없다. 

하지만 ES6의 메서드와 화살표 함수는 ES6 이전의 함수와 명확한 차이가 있다.

<br>

## 2. 메서드

<br>

ES6 이전 사양에는 메서드에 대한 명확한 정의가 없었다.

**ES6 사양에서 메서드는 메서드 축약 표현으로 정의된 함수만을 의미한다.**

```jsx
const obj = {
  x: 1,
  // foo는 메서드이다.
  foo() { return this.x; },
  // bar에 바인딩된 함수는 메서드가 아닌 일반 함수이다.
  bar: function() { return this.x; }
};

console.log(obj.foo()); // 1
console.log(obj.bar()); // 1
```

<br>

**ES6 사양에서 정의한 메서드(이하 ES6 메서드)는 인스턴스를 생성할 수 없는 non-constructor다.**

```jsx
new obj.foo(); // -> TypeError: obj.foo is not a constructor
new obj.bar(); // -> bar {}
```

<br>

ES6 메서드는 인스턴스를 생성할 수 없으므로 prototype 프로퍼티가 없고 프로토타입도 생성하지 않는다.

```jsx
// obj.foo는 constructor가 아닌 ES6 메서드이므로 prototype 프로퍼티가 없다.
obj.foo.hasOwnProperty('prototype'); // -> false

// obj.bar는 constructor인 일반 함수이므로 prototype 프로퍼티가 있다.
obj.bar.hasOwnProperty('prototype'); // -> true
```

<br>

표준 빌드인 객체가 제공하는 프로토타입 메서드와 정적 메서드는 모두 non-constructor다.

```jsx
String.prototype.toUpperCase.prototype; // -> undefined
String.fromCharCode.prototype           // -> undefined

Number.prototype.toFixed.prototype; // -> undefined
Number.isFinite.prototype;          // -> undefined

Array.prototype.map.prototype; // -> undefined
Array.from.prototype;          // -> undefined
```

<br>

**ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 [[HomeObject]]를 갖는다.**

super 참조는 내부 슬롯 [[HomeObject]]를 사용하여 수퍼클래스의 메서드를 참조하므로 내부 슬롯 [[HomeObject]]를 갖는 ES6 메서드는 super 키워드를 사용할 수 있다.

```jsx
const base = {
  name: 'Lee',
  sayHi() {
    return `Hi! ${this.name}`;
  }
};

const derived = {
  __proto__: base,
  // sayHi는 ES6 메서드다. ES6 메서드는 [[HomeObject]]를 갖는다.
  // sayHi의 [[HomeObject]]는 derived.prototype을 가리키고
  // super는 sayHi의 [[HomeObject]]의 프로토타입인 base.prototype을 가리킨다.
  sayHi() {
    return `${super.sayHi()}. how are you doing?`;
  }
};

console.log(derived.sayHi()); // Hi! Lee. how are you doing?
```

<br>

ES6 메서드가 아닌 함수는 super 키워드를 사용할 수 없다. ES6 메서드가 아닌 함수는 내부 슬롯 [[HomeObject]]를 갖지 않기 때문이다.

```jsx
const derived = {
  __proto__: base,
  // sayHi는 ES6 메서드가 아니다.
  // 따라서 sayHi는 [[HomeObject]]를 갖지 않으므로 super 키워드를 사용할 수 없다.
  sayHi: function () {
    // SyntaxError: 'super' keyword unexpected here
    return `${super.sayHi()}. how are you doing?`;
  }
};
```

<br>

이처럼 ES6 메서드는 본연의 기능(super)을 추가하고 의미적으로 맞지 않는 기능(constructor)은 제거했다.

<br>

## 3. 화살표 함수

<br>

화살표 함수(arrow function)는 function 키워드 대신 화살표(=>, fat arrow)를 사용하여 기존의 함수 정의 방식보다 간략하게 함수를 정의할 수 있다.

특히 화살표 함수는 콜백 함수 내부에서 this가 전역 객체를 가리키는 문제를 해결하기 위한 대안으로 유용하다.

<br>

### 3.1. 화살표 함수 정의

<br>

**1 . 함수 정의**

화살표 함수는 함수 선언문으로 정의할 수 없고 함수 표현식으로 정의해야 한다. 호출 방식은 기존 함수와 동일하다.

```jsx
const multiply = (x, y) => x * y;
multiply(2, 3); // -> 6
```

<br>

**2 . 매개변수 선언**

매개변수가 여러 개인 경우 소괄호 () 안에 매개변수를 선언한다.

```jsx
const arrow = (x, y) => { ... };
```

<br>

매개변수가 한 개인 경우 소괄호 ()를 생략할 수 있다.

```jsx
const arrow = x => { ... };
```

<br>

매개변수가 없는 경우 소괄호 ()를 생략할 수 없다.

```jsx
const arrow = () => { ... };
```

<br>

**3 . 함수 몸체 정의**

함수 몸체가 하나의 문으로 구성된다면 함수 몸체를 감싸는 중괄호 {}를 생략할 수 있다. 

이때 함수 몸체 내부의 문이 값으로 평가될 수 있는 표현식인 문이라면 암묵적으로 반환된다.

```jsx
// concise body
const power = x => x ** 2;
power(2); // -> 4

// 위 표현은 다음과 동일하다.
// block body
const power = x => { return x ** 2; };
```

<br>

함수 몸체를 감싸는 중괄호 {}를 생략한 경우 함수 몸체 내부의 문이 표현식이 아닌 문이라면 에러가 발생한다.

```jsx
onst arrow = () => const x = 1; // SyntaxError: Unexpected token 'const'

// 위 표현은 다음과 같이 해석된다.
const arrow = () => { return const x = 1; };
```

<br>

따라서 함수 몸체가 하나의 문으로 구성된다 해도 함수 몸체의 문이 표현식이 아닌 문이라면 중괄호를 생략할 수 없다.

```jsx
const arrow = () => { const x = 1; };
```

<br>

객체 리터럴을 반환하는 경우 객체 리터럴을 소괄호 ()로 감싸 주어야 한다.

```jsx
const create = (id, content) => ({ id, content });
create(1, 'JavaScript'); // -> {id: 1, content: "JavaScript"}

// 위 표현은 다음과 동일하다.
const create = (id, content) => { return { id, content }; };
```

<br>

객체 리터럴을 소괄호 ()로 감싸지 않으면 객체 리터럴의 중괄호 {}를 함수 몸체를 감싸는 중괄호 {}로 잘못 해석한다.

```jsx
// { id, content }를 함수 몸체 내의 쉼표 연산자문으로 해석한다.
const create = (id, content) => { id, content };
create(1, 'JavaScript'); // -> undefined
```

<br>

함수 몸체가 여러 개의 문으로 구성된다면 함수 몸체를 감싸는 중괄호 {}를 생략할 수 없다. 이때 반환값이 있다면 명시적으로 반환해야 한다.

```jsx
const sum = (a, b) => {
  const result = a + b;
  return result;
};
```

<br>

화살표 함수도 즉시 실행 함수(IIFE)로 사용할 수 있다.

```jsx
const person = (name => ({
  sayHi() { return `Hi? My name is ${name}.`; }
}))('Lee');

console.log(person.sayHi()); // Hi? My name is Lee.
```

<br>

화살표 함수도 일급 객체이므로 Array.prototype.map, Array.prototype.filter, Array.prototype.reduce 같은 고차 함수(Higher-Order Function, HOF)에 인수로 전달할 수 있다. 

이 경우 일반적인 함수 표현식보다 표현이 간결하고 가독성이 좋다.

```jsx
// ES5
[1, 2, 3].map(function (v) {
  return v * 2;
});

// ES6
[1, 2, 3].map(v => v * 2); // -> [ 2, 4, 6 ]
```

<br>

이처럼 화살표 함수는 콜백 함수로서 정의할 때 유용하다.

<br>

### 3.2. 화살표 함수와 일반 함수의 차이

<br>

**1. 화살표 함수는 인스턴스를 생성할 수 없는 non-constructor다.**

```jsx
const Foo = () => {};
// 화살표 함수는 생성자 함수로서 호출할 수 없다.
new Foo(); // TypeError: Foo is not a constructor
```

<br>

화살표 함수는 인스턴스를 생성할 수 없으므로 prototype 프로퍼티가 없고 프로토타입도 생성하지 않는다.

```jsx
const Foo = () => {};
// 화살표 함수는 prototype 프로퍼티가 없다.
Foo.hasOwnProperty('prototype'); // -> false
```

<br>

**2. 중복된 매개변수 이름을 선언할 수 없다.**

일반 함수는 중복된 매개변수 이름을 선언해도 에러가 발생하지 않는다.

```jsx
function normal(a, a) { return a + a; }
console.log(normal(1, 2)); // 4
```

<br>

strict mode에서 중복된 매개변수 이름을 선언하면 에러가 발생한다.

```jsx
'use strict';

function normal(a, a) { return a + a; }
// SyntaxError: Duplicate parameter name not allowed in this context
```

<br>

화살표 함수에서도 중복된 매개변수 이름을 선언하면 에러가 발생한다.

```jsx
const arrow = (a, a) => a + a;
// SyntaxError: Duplicate parameter name not allowed in this context
```

<br>

**3. 화살표 함수는 함수 자체의 this, arguments, super, new.target 바인딩을 갖지 않는다.**

<br>

화살표 함수 내부에서 this, arguments, super, new.target을 참조하면 스코프 체인을 통해 상위 스코프의 this, arguments, super, new.target을 참조한다.

<br>

### 3.3. this

<br>

화살표 함수가 일반 함수와 구별되는 가장 큰 특징은 바로 this다.

화살표 함수는 다른 함수의 인수로 전달되어 콜백 함수로 사용되는 경우가 많다.

<br>

화살표 함수의 this는 일반 함수의 this와 다르게 동작한다. 

이는 “콜백 함수 내부의 this 문제”, 즉 **콜백 함수 내부의 this가 외부 함수의 this와 다르기 때문에 발생하는 문제를 해결하기 위해 의도적으로 설계된 것이다.**

<br>

```jsx
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }

  add(arr) {
    // add 메서드는 인수로 전달된 배열 arr을 순회하며 배열의 모든 요소에 prefix를 추가한다.
    // ①
    return arr.map(function (item) {
      return this.prefix + item; // ②
      // -> TypeError: Cannot read property 'prefix' of undefined
    });
  }
}

const prefixer = new Prefixer('-webkit-');
console.log(prefixer.add(['transition', 'user-select']));
```

<br>

프로토타입 메서드 내부인 ①에서 this는 메서드를 호출한 객체(위 예제의 경우 prefixer 객체)를 가리킨다. 

그런데 Array.prototype.map의 인수로 전달한 콜백 함수의 내부인 ②에서 this는 undefined를 가리킨다. 

이는 Array.prototype.map 메서드가 콜백 함수를 일반 함수로서 호출하기 때문이다.

<br>

strict mode에서 일반 함수로서 호출된 모든 함수 내부의 this에는 전역 객체가 아니라 undefined가 바인딩되므로 일반 함수로서 호출되는 Array.prototype.map 메서드의 콜백 함수 내부의 this에는 undefined가 바인딩된다.

<br>

이와 같은 “콜백 함수 내부의 this 문제”를 해결하기 위해 ES6 이전에는 다음과 같은 방법을 사용했다.

<br>

**1. add 메서드를 호출한 prefixer 객체를 가리키는 this를 일단 회피시킨 후에 콜백 함수 내부에서 사용한다.**

```jsx
...
add(arr) {
  // this를 일단 회피시킨다.
  const that = this;
  return arr.map(function (item) {
    // this 대신 that을 참조한다.
    return that.prefix + ' ' + item;
  });
}
...
```

<br>

**2. Array.prototype.map의 두 번째 인수로 add 메서드를 호출한 prefixer 객체를 가리키는 this를 전달한다.**

ES5에서 도입된 Array.prototype.map은 “콜백 함수 내부의 this 문제”를 해결하기 위해 두 번째 인수로 콜백 함수 내부에서 this로 사용할 객체를 전달할 수 있다.

```jsx
...
add(arr) {
  return arr.map(function (item) {
    return this.prefix + ' ' + item;
  }, this); // this에 바인딩된 값이 콜백 함수 내부의 this에 바인딩된다.
}
...
```

<br>

**3. Function.prototype.bind 메서드를 사용하여 add 메서드를 호출한 prefixer 객체를 가리키는 this를 바인딩한다.**

```jsx
...
add(arr) {
  return arr.map(function (item) {
    return this.prefix + ' ' + item;
  }.bind(this)); // this에 바인딩된 값이 콜백 함수 내부의 this에 바인딩된다.
}
...
```

<br>

ES6에서는 화살표 함수를 사용하여 “콜백 함수 내부의 this 문제”를 해결할 수 있다.

```jsx
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }

  add(arr) {
    return arr.map(item => this.prefix + item);
  }
}

const prefixer = new Prefixer('-webkit-');
console.log(prefixer.add(['transition', 'user-select']));
// ['-webkit-transition', '-webkit-user-select']
```

<br>

**화살표 함수는 함수 자체의 this 바인딩을 갖지 않는다. 따라서 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this를 그대로 참조한다.** 

**이를 lexical this라 한다.**

<br>

렉시컬 스코프를 통해 상위스코프의 this 바인딩 된값을 가진다.

<br>

```jsx
// 화살표 함수는 상위 스코프의 this를 참조한다.
() => this.x;

// 익명 함수에 상위 스코프의 this를 주입한다. 위 화살표 함수와 동일하게 동작한다.
(function () { return this.x; }).bind(this);
```

<br>

만약 화살표 함수와 화살표 함수가 중첩되어 있다면 상위 화살표 함수에도 this 바인딩이 없으므로 스코프 체인 상에서 가장 가까운 상위 함수 중에서 화살표 함수가 아닌 함수의 this를 참조한다.

```jsx
// 중첩 함수 foo의 상위 스코프는 즉시 실행 함수다.
// 따라서 화살표 함수 foo의 this는 상위 스코프인 즉시 실행 함수의 this를 가리킨다.
(function () {
  const foo = () => console.log(this);
  foo();
}).call({ a: 1 }); // { a: 1 }

// bar 함수는 화살표 함수를 반환한다.
// bar 함수가 반환한 화살표 함수의 상위 스코프는 화살표 함수 bar다.
// 하지만 화살표 함수는 함수 자체의 this 바인딩을 갖지 않으므로 bar 함수가 반환한
// 화살표 함수 내부에서 참조하는 this는 화살표 함수가 아닌 즉시 실행 함수의 this를 가리킨다.
(function () {
  const bar = () => () => console.log(this);
  bar()();
}).call({ a: 1 }); // { a: 1 }
```

<br>

만약 화살표 함수가 전역 함수라면 화살표 함수의 this는 전역 객체를 가리킨다. 전역 함수의 상위 스코프는 전역이고 전역에서 this는 전역 객체를 가리키기 때문이다.

```jsx
// 전역 함수 foo의 상위 스코프는 전역이므로 화살표 함수 foo의 this는 전역 객체를 가리킨다.
const foo = () => console.log(this);
foo(); // window
```

<br>

프로퍼티에 할당한 화살표 함수도 스코프 체인 상에서 가장 가까운 상위 함수 중에서 화살표 함수가 아닌 함수의 this를 참조한다.

```jsx
// increase 프로퍼티에 할당한 화살표 함수의 상위 스코프는 전역이다.
// 따라서 increase 프로퍼티에 할당한 화살표 함수의 this는 전역 객체를 가리킨다.
const counter = {
  num: 1,
  increase: () => ++this.num
};

console.log(counter.increase()); // NaN
```

<br>

화살표 함수는 함수 자체의 this 바인딩을 갖지 않기 때문에 Function.prototype.call, Function.prototype.apply, Function.prototype.bind 메서드를 사용해도 화살표 함수 내부의 this를 교체할 수 없다.

```jsx
window.x = 1;

const normal = function () { return this.x; };
const arrow = () => this.x;

console.log(normal.call({ x: 10 })); // 10
console.log(arrow.call({ x: 10 }));  // 1
```

<br>

화살표 함수는 함수 자체의 this 바인딩을 갖지 않기 때문에 this를 교체할 수 없고 언제나 상위 스코프의 this 바인딩을 참조한다.

```jsx
const add = (a, b) => a + b;

console.log(add.call(null, 1, 2));    // 3
console.log(add.apply(null, [1, 2])); // 3
console.log(add.bind(null, 1, 2)());  // 3
```

<br>

메서드를 화살표 함수로 정의하는 것은 피해야 한다.

```jsx
// Bad
const person = {
  name: 'Lee',
  sayHi: () => console.log(`Hi ${this.name}`)
};

// sayHi 프로퍼티에 할당된 화살표 함수 내부의 this는 상위 스코프인 전역의 this가 가리키는
// 전역 객체를 가리키므로 이 예제를 브라우저에서 실행하면 this.name은 빈 문자열을 갖는
// window.name과 같다. 전역 객체 window에는 빌트인 프로퍼티 name이 존재한다.
person.sayHi(); // Hi
```

<br>

위 스코프인 전역의 this가 가리키는 전역 객체를 가리킨다.

메서드를 정의할 때는 ES6 메서드 축약 표현으로 정의한 ES6 메서드를 사용하는 것이 좋다.

```jsx
// Good
const person = {
  name: 'Lee',
  sayHi() {
    console.log(`Hi ${this.name}`);
  }
};

person.sayHi(); // Hi Lee
```

<br>

프로토타입 객체의 프로퍼티에 화살표 함수를 할당하는 경우도 동일한 문제가 발생한다.

```jsx
// Bad
function Person(name) {
  this.name = name;
}

Person.prototype.sayHi = () => console.log(`Hi ${this.name}`);

const person = new Person('Lee');
// 이 예제를 브라우저에서 실행하면 this.name은 빈 문자열을 갖는 window.name과 같다.
person.sayHi(); // Hi
```

<br>

프로퍼티를 동적 추가할 때는 ES6 메서드 정의를 사용할 수 없으므로 일반 함수를 할당한다.

```jsx
// Good
function Person(name) {
  this.name = name;
}

Person.prototype.sayHi = function () { console.log(`Hi ${this.name}`); };

const person = new Person('Lee');
person.sayHi(); // Hi Lee
```

<br>

일반 함수가 아닌 ES6 메서드를 동적 추가하고 싶다면 다음과 같이 객체 리터럴을 바인딩하고 프로토타입의 constructor 프로퍼티와 생성자 함수 간의 연결을 재설정한다.

```jsx
function Person(name) {
  this.name = name;
}

Person.prototype = {
  // constructor 프로퍼티와 생성자 함수 간의 연결을 재설정
  constructor: Person,
  sayHi() { console.log(`Hi ${this.name}`); }
};

const person = new Person('Lee');
person.sayHi(); // Hi Lee
```

<br>

**클래스 필드 정의 제안**을 사용하여 클래스 필드에 화살표 함수를 할당할 수도 있다.

```jsx
// Bad
class Person {
  // 클래스 필드 정의 제안
  name = 'Lee';
  sayHi = () => console.log(`Hi ${this.name}`);
}

const person = new Person();
person.sayHi(); // Hi Lee
```

<br>

sayHi 클래스 필드는 인스턴스 프로퍼티이므로 다음과 같은 의미다.

```jsx
class Person {
  constructor() {
    this.name = 'Lee';
    // 클래스가 생성한 인스턴스(this)의 sayHi 프로퍼티에 화살표 함수를 할당한다.
    // sayHi 프로퍼티는 인스턴스 프로퍼티이다.
    this.sayHi = () => console.log(`Hi ${this.name}`);
  }
}
```

<br>

따라서 메서드를 정의할 때는 ES6 메서드 축약 표현으로 정의한 ES6 메서드를 사용하는 것이 좋다.

```jsx
// Good
class Person {
  // 클래스 필드 정의
  name = 'Lee';

  sayHi() { console.log(`Hi ${this.name}`); }
}
const person = new Person();
person.sayHi(); // Hi Lee
```

<br>

### 3.4. super

화살표 함수는 함수 자체의 super 바인딩을 갖지 않는다. ([[HomeObject]]이 없다는 의미)

따라서 화살표 함수 내부에서 super를 참조하면 this와 마찬가지로 상위 스코프의 super를 참조한다.

```jsx
class Base {
  constructor(name) {
    this.name = name;
  }

  sayHi() {
    return `Hi! ${this.name}`;
  }
}

class Derived extends Base {
  // 화살표 함수의 super는 상위 스코프인 constructor의 super를 가리킨다.
  sayHi = () => `${super.sayHi()} how are you doing?`;
}

const derived = new Derived('Lee');
console.log(derived.sayHi()); // Hi! Lee how are you doing?
```

<br>

super는 내부 슬롯 [[HomeObject]]를 갖는 ES6 메서드 내에서만 사용할 수 있는 키워드다.

<br>

sayHi 클래스 필드에 할당한 화살표 함수는 ES6 메서드는 아니지만 함수 자체의 super 바인딩을 갖지 않으므로 super를 참조해도 에러가 발생하지 않고 상위 스코프인 constructor의 super 바인딩을 참조한다.

<br>

### 3.5. arguments

화살표 함수는 함수 자체의 arguments 바인딩을 갖지 않는다. 

따라서 화살표 함수 내부에서 arguments를 참조하면 this와 마찬가지로 상위 스코프의 arguments를 참조한다.

```jsx
(function () {
  // 화살표 함수 foo의 arguments는 상위 스코프인 즉시 실행 함수의 arguments를 가리킨다.
  const foo = () => console.log(arguments); // [Arguments] { '0': 1, '1': 2 }
  foo(3, 4);
}(1, 2));

// 화살표 함수 foo의 arguments는 상위 스코프인 전역의 arguments를 가리킨다.
// 하지만 전역에는 arguments 객체가 존재하지 않는다. arguments 객체는 함수 내부에서만 유효하다.
const foo = () => console.log(arguments);
foo(1, 2); // ReferenceError: arguments is not defined
```

<br>

arguments 객체는 가변 인자 함수를 구현할 때 유용하다.

상위 스코프의 arguments 객체를 참조할 수는 있지만 화살표 함수 자신에게 전달된 인수 목록을 확인할 수 없고 상위 함수에게 전달된 인수 목록을 참조하므로 그다지 도움이 되지 않는다.

<br>

따라서 화살표 함수로 가변 인자 함수를 구현해야 할 때는 반드시 Rest 파라미터를 사용해야 한다.

<br>

## 4. Rest 파라미터

<br>

### 4.1. 기본 문법

Rest 파라미터(Rest parameter, 나머지 매개변수)는 매개변수 이름 앞에 세개의 점 …을 붙여서 정의한 매개변수를 의미한다. 

**Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받는다.**

```jsx
function foo(...rest) {
  // 매개변수 rest는 인수들의 목록을 배열로 전달받는 Rest 파라미터다.
  console.log(rest); // [ 1, 2, 3, 4, 5 ]
}

foo(1, 2, 3, 4, 5);
```

<br>

일반 매개변수와 Rest 파라미터는 함께 사용할 수 있다. 

이때 함수에 전달된 인수들은 매개변수와 Rest 파라미터에 순차적으로 할당된다.

```jsx
function foo(param, ...rest) {
  console.log(param); // 1
  console.log(rest);  // [ 2, 3, 4, 5 ]
}

foo(1, 2, 3, 4, 5);

function bar(param1, param2, ...rest) {
  console.log(param1); // 1
  console.log(param2); // 2
  console.log(rest);   // [ 3, 4, 5 ]
}

bar(1, 2, 3, 4, 5);
```

<br>

Rest 파라미터는 이름 그대로 먼저 선언된 매개변수에 할당된 인수를 제외한 나머지 인수들로 구성된 배열이 할당된다. 

따라서 Rest 파라미터는 반드시 마지막 파라미터이어야 한다.

```jsx
function foo(...rest, param1, param2) { }

foo(1, 2, 3, 4, 5);
// SyntaxError: Rest parameter must be last formal parameter
```

<br>

Rest 파라미터는 단 하나만 선언할 수 있다.

```jsx
function foo(...rest1, ...rest2) { }

foo(1, 2, 3, 4, 5);
// SyntaxError: Rest parameter must be last formal parameter
```

<br>

Rest 파라미터는 함수 정의 시 선언한 매개변수 개수를 나타내는 함수 객체의 length 프로퍼티에 영향을 주지 않는다.

```jsx
function foo(...rest) {}
console.log(foo.length); // 0

function bar(x, ...rest) {}
console.log(bar.length); // 1

function baz(x, y, ...rest) {}
console.log(baz.length); // 2
```

<br>

### 4.2. Rest 파라미터와 arguments 객체

<br>

arguments 객체는 함수 호출 시 전달된 인수(argument)들의 정보를 담고 있는 순회 가능한(iterable) 유사 배열 객체(array-like object)이며, 함수 내부에서 지역 변수처럼 사용할 수 있다.

```jsx
// 매개변수의 개수를 사전에 알 수 없는 가변 인자 함수
function sum() {
  // 가변 인자 함수는 arguments 객체를 통해 인수를 전달받는다.
  console.log(arguments);
}

sum(1, 2); // {length: 2, '0': 1, '1': 2}
```

<br>

하지만 arguments 객체는 배열이 아닌 유사 배열 객체이므로 배열 메서드를 사용하려면 Function.prototype.call이나 Function.prototype.apply 메서드를 사용해 arguments 객체를 배열로 변환해야 하는 번거로움이 있었다.

```jsx
function sum() {
  // 유사 배열 객체인 arguments 객체를 배열로 변환한다.
  var array = Array.prototype.slice.call(arguments);

  return array.reduce(function (pre, cur) {
    return pre + cur;
  }, 0);
}

console.log(sum(1, 2, 3, 4, 5)); // 15
```

<br>

ES6에서는 rest 파라미터를 사용하여 가변 인자 함수의 인수 목록을 배열로 직접 전달받을 수 있다.

```jsx
function sum(...args) {
  // Rest 파라미터 args에는 배열 [1, 2, 3, 4, 5]가 할당된다.
  return args.reduce((pre, cur) => pre + cur, 0);
}
console.log(sum(1, 2, 3, 4, 5)); // 15
```

<br>

함수와 ES6 메서드는 Rest 파라미터와 arguments 객체를 모두 사용할 수 있다. 하지만 화살표 함수는 함수 자체의 arguments 객체를 갖지 않는다.

<br>

## 5. 매개변수 기본값

<br>

```jsx
function sum(x, y) {
  return x + y;
}

console.log(sum(1)); // NaN
```

<br>

다음 예제와 같이 매개변수에 인수가 전달되었는지 확인하여 인수가 전달되지 않은 경우 매개변수에 기본값을 할당할 필요가 있다. 

즉, 방어 코드가 필요하다.

```jsx
function sum(x, y) {
  // 인수가 전달되지 않아 매개변수의 값이 undefined인 경우 기본값을 할당한다.
  x = x || 0;
  y = y || 0;

  return x + y;
}

console.log(sum(1, 2)); // 3
console.log(sum(1));    // 1
```

<br>

ES6에서 도입된 매개변수 기본값을 사용하면 함수 내에서 수행하던 인수 체크 및 초기화를 간소화할 수 있다.

```jsx
function sum(x = 0, y = 0) {
  return x + y;
}

console.log(sum(1, 2)); // 3
console.log(sum(1));    // 1
```

<br>

매개변수 기본값은 매개변수에 인수를 전달하지 않은 경우와 undefined를 전달한 경우에만 유효하다.

```jsx
function logName(name = 'Lee') {
  console.log(name);
}

logName();          // Lee
logName(undefined); // Lee
logName(null);      // null
```

<br>

앞서 살펴본 Rest 파라미터에는 기본값을 지정할 수 없다.

```jsx
function foo(...rest = []) {
  console.log(rest);
}
// SyntaxError: Rest parameter may not have a default initializer
```

<br>

매개변수 기본값은 함수 정의 시 선언한 매개변수 개수를 나타내는 함수 객체의 length 프로퍼티와 arguments 객체에 아무런 영향을 주지 않는다.

```jsx
function sum(x, y = 0) {
  console.log(arguments);
}

console.log(sum.length); // 1

sum(1);    // Arguments { '0': 1 }
sum(1, 2); // Arguments { '0': 1, '1': 2 }
```
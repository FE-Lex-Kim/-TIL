- [TypeScript(3) - 유니온 타입(Union), 교차타입(Intersection), 리터럴 타입, Enum , never , 제네릭(Generic)](#typescript3---유니온-타입union-교차타입intersection-리터럴-타입-enum--never--제네릭generic)
  - [유니온 타입 (Union)](#유니온-타입-union)
  - [교차 타입(Intersection)](#교차-타입intersection)
  - [리터럴 타입](#리터럴-타입)
  - [Enum 타입](#enum-타입)
  - [never](#never)
  - [제네릭 함수 (Generics)](#제네릭-함수-generics)
    - [1.제네릭 호출 시그니처](#1제네릭-호출-시그니처)
    - [2.제네릭을 함수처럼 사용하기](#2제네릭을-함수처럼-사용하기)
    - [3.제한된 제네릭](#3제한된-제네릭)
    - [4. 제네릭 맵드 타입](#4-제네릭-맵드-타입)
    - [제네릭 사용 예시(API)](#제네릭-사용-예시api)
  - [추론(Inference)](#추론inference)
    - [주의해야할 점](#주의해야할-점)

# TypeScript(3) - 유니온 타입(Union), 교차타입(Intersection), 리터럴 타입, Enum , never , 제네릭(Generic)

<br>

## 유니온 타입 (Union)

유니온 타입은 여러 타입 중 하나일 수 있는 값을 나타내는 TypeScript의 타입이다.

예를들어 ( A | B ) 는 A 또는 B 타입중 하나가 될 수 있다는 말이다.

```jsx
// 숫자 또는 문자열을 받아들이는 함수
function processInput(input: number | string): void {
  if (typeof input === "number") {
    console.log("Input is a number:", input);
    console.log("Result:", input * 2);
  } else {
    console.log("Input is a string:", input);
    console.log("Result:", input.toUpperCase());
  }
}

// 숫자 입력 예시
processInput(10); // Output: Input is a number: 10, Result: 20

// 문자열 입력 예시
processInput("hello"); // Output: Input is a string: hello, Result: HELLO
```

input은 number or string 값이 들어올 수 있게 파이프( | ) 를 사용해서 설정한다.

이를 유니온이라고 부른다.

<br>

```tsx
function returnNumber(value: string | number): number {
  return parseInt(value);
}
```

value는 숫자, 문자열이 될수도 있다는 뜻이다. parseInt(value)는 에러가 발생한다.

string 타입 매개변수로 넣을 수 없다고 나온다.

타입스크립트에서는 parseInt(1)은 어차피 1이므로 의미가 없어서 하지말라는 의미로 에러가나온다.

따라서 if 문으로 타입을 좁혀야한다.

```tsx
function returnNumber(value: string | number): number {
  if (typeof value === "string") {
    return parseInt(value);
  } else {
    return value;
  }
}
```

따라서 유니온 타입을 사용할 때는 각 타입에 대해 정확히 이해하고, 예상치 못한 결과가 발생하지 않도록 주의해야 한다.

<br>

## 교차 타입(Intersection)

**교차 타입을 사용하면 여러가지 타입을 결합해서 새로 생성된 하나의 단일 타입으로 만들 수 있다.**

예를 들어 A타입과 B타입이 있다면, A & B 타입의 교차타입인 C는 A와 B의 모든 멤버를 포함해서 가지고 있는 타입이다.

<br>

교차 타입은 & 을 사용해서 표기한다.

```tsx
type NameAge = {
  name: string;
  age: number;
};

type Email = {
  email: string;
};

type Person = NameAge & Email;

const person: Person = {
  name: "alex",
  age: 29,
  email: "lexkim.dev@gmail.com",
};

console.log(person.name, person.age, person.email);
```

<br>

## 리터럴 타입

( | ) 로 구분하는 리터럴 타입을 사용하면 **정해진 문자열이나 수치만 대입할 수 있다.**

```tsx
let Name: "Alex" | "James" | "Andrew";

Name = "Alex";
console.log("name: ", Name);
Name = "James";
console.log("name: ", Name);
Name = "Smith"; // 타입 선언에 없는 값이므로 에러 발생
console.log("name: ", Name);
```

<br>

함수 반환값으로도 사용될 수 있다.

```tsx
function compare(a: string, b: string): -1 | 0 | 1 {
  return a === b ? 0 : a > b ? 1 : -1;
}
```

<br>

## Enum 타입

enum을 사용하면 이름이 붙은 상수를 정의할 수 있다.

```tsx
enum Direction {
  Up, // 0
  Down, // 1
  Left, // 2
  Right, // 3
}

let direction: Direction = Direction.Left;
console.log(direction);

direction = 4; // Direction에 열겨된 값이외의 값이 할당되므로 에러가 된다.

console.log(direction);
```

enum은 정의된 순서대로 자동으로 숫자가 증가한다.

위의 예제처럼 Up에 값을 정의하지 않으면 0부터 숫자가 증가하면서 설정된다.

Direction에 열거된 값 이외의 값은 할당할 수 없다.

```tsx
enum Direction {
  Up = 1,
  Down, // 2
  Left, // 3
  Right, // 4
}
```

<br>

문자열도 enum 타입으로 사용할 수 있다.

문자열 기반 enum 타입을 사용할때는 반드시 문자열 상수로 초기화해야 한다.

```tsx
enum Direction {
  Up = "UP",
  Down = "DOWN",
  Left = "LEFT",
  Right = "RIGHT",
}
```

문자열은 자동 증가하는 기능이 없지만, 직렬화를 잘한다는 이점이 있다.

만약 숫자값을 디버깅하고 있는데 그 값을 읽어야 한다면, 종종 그 값이 숫자만으로는 이것이 어떤 의미인지 잘 모른다.

**반면, 문자열로 열겨형을 이용하면 코드를 실행할 때, 유의미하고 읽기 좋은 값을 이용하여 실행 할 수 있다.**

<br>

**enum 타입은 브랜딩을 위해 사용하면 좋다.**

```tsx
enum Name {
  alex,
  james,
}

interface Alex {
  type: Name.alex;
}

interface James {
  type: Name.james;
}

function alexOrJames(param: Alex | James) {
  if (param.type === Name.alex) {
    param; // param : Alex
  } else {
    param; // param : James
  }
}
```

<br>

## never

never 절대로 발생하지 않는 값의 종류를 나타낸다.

```tsx
function fn(input: never) {}

// 오직 `never` 만 받는다.
declare let myNever: never;
fn(myNever); // ✅

// 아무 값이나 전달하거나 아무 값도 전달하지 않으면 타입 에러 발생
fn(); // ❌ 인자 'input'에 아무 값도 주어지지 않음
fn(1); // ❌ 'number' 타입은 'never' 타입에 할당할 수 없음
fn("foo"); // ❌ 'string' 타입은 'never' 타입에 할당할 수 없음

// `any`도 통과할 수 없다.
declare let myAny: any;
fn(myAny); // ❌ 'any' 타입은 'never' 타입에 할당할 수 없음
```

<br>

자바스크립트에서는 의도적으로 런타임때 에러를 발생시키고 캐치할 수 있다.

이는 값을 반환하는 것이 아니다. 따라서 에러를 던지는 작업을 한다면, 해당 함수의 반환 타입은 never이다.

if문이나 switch문에서 `default` 는 남아 있는것은 never 타입이여서 모든 상황에 대처할 수 있다.

```tsx
function unknownColor(x: string): never {
  throw new Error("unknown color");
}

type Color = "red" | "green" | "blue";

function getColorName(c: Color): string {
  switch (c) {
    case "red":
      return "is red";
    case "green":
      return "is green";
    default:
      return unknownColor(c); // 'string' 타입은 'never' 타입에 할당할 수 없음
  }
}

getColorName("blue");
```

```tsx
function error(message: string): never {
  throw new Error(message);
}

function foo(x: string | number | number[]): boolean {
  if (typeof x === "string") {
    return true;
  } else if (typeof x === "number") {
    return false;
  }
  return error("Never happens");
}
```

never 타입은 모든 타입의 하위 타입이다. 따라서 never는 자신을 제외한 어떤 타입도 never 타입에 할당 할 수 없다. any도 never 타입에 할당 할 수 없다.

<br>

## 제네릭 함수 (Generics)

함수, 타입, 클래스 등에서 내부적으로 사용할 타입을 미리 정해두지 않고 타입 변수를 사용해서 해당 위치를 비워 둔 다음에, 실제로 그 값을 사용할때 외부에서 타입 **변수 자리에** 타입을 지정하여 사용하는 방식을 말한다.

<br>

타입 변수 or 타입 매개변수란

Person<T> 에서 T를 의미한다.

<br>

이렇게 하면 함수, 타입, 클래스 등 여러 타입에 대해 하나하나 따로 정의하지 않아도 되기 때문에 재사용성이 크게 향상된다.

타입 변수는 일반적으로 <T>와 같이 꺽쇠 괄호 내부에 정의하며, 사용할 때는 함수에 매개변수를 넣는 것과 같이 유사하게 원하는 타입을 넣어주면 된다.

보통 타입 변수명으로 T(Type), E(Element), K(Key), V(Value) 등 한글자로 된 이름을 많이 사용한다.

```tsx
type ExampleArrayType<T> = T[];

// 숫자 배열
const array2: ExampleArrayType<number> = [1, 2, 3, 4, 5];

// 불리언 배열
const array3: ExampleArrayType<boolean> = [true, false, true];

console.log(array2); // 출력: [1, 2, 3, 4, 5]
console.log(array3); // 출력: [true, false, true]
```

<br>

제네릭을 호출할때 반드시 꺾쇠괄호만 명시하는것이 아니다.

컴파일러가 인수를 보고 타입을 추론해준다. 따라서 생략도 가능하다.

```tsx
// 제네릭 함수
function identity<T>(arg: T): T {
  return arg;
}

// 제네릭 함수 호출
let result1 = identity("Hello"); // result1의 타입은 string로 추론됩니다.
let result2 = identity(123); // result2의 타입은 number로 추론됩니다.

console.log(result1); // 출력: Hello
console.log(result2); // 출력: 123
```

<br>

또한 특정요소 타입을 알 수 없을 때는 제니릭 타입에 기본 값을 추가할 수 있다.

```tsx
interface Person<T = string> {
  name: T;
}
```

<br>

주의할점은 함수나 클래스 등의 내부에서 제네릭을 사용할 때 어떤 타입이든 될 수 있다.

**따라서 특정 타입에서만 존재하는 인자를 참조를 하려면 안된다.**

```tsx
// 제네릭 함수
function printLength<T>(arg: T): void {
  // T 타입이 'length' 속성을 가지고 있다고 가정합니다.
  console.log(arg.length); // 오류: 'length' 속성이 'T'에 존재하지 않습니다.
}

// 문자열 배열을 전달합니다.
printLength(["apple", "banana", "orange"]);

// 숫자를 전달합니다.
printLength(123); // 오류: 'length' 속성이 'number'에 존재하지 않습니다.
```

<br>

이럴때는 length 속성을 가진 타입만을 받는다 라는 제약을 걸어주면 된다.

```tsx
interface TypeWithLength {
  length: number;
}

function exampleFunc2<T extends TypeWithLength>(arg: T): number {
  return arg.length;
}

// 예제 사용
const strLength = exampleFunc2("Hello"); // 문자열의 길이를 반환합니다.
console.log(strLength); // 출력: 5

const arrLength = exampleFunc2([1, 2, 3, 4, 5]); // 배열의 길이를 반환합니다.
console.log(arrLength); // 출력: 5
```

<br>

제네릭을 사용할때 TypeScript에서 JSX(JavaScript XML)를 사용하는 파일의 확장자는 `.tsx`이다. JSX는 React와 같은 라이브러리에서 컴포넌트를 작성할 때 사용되는 문법이다.
따라서 제네릭의 꺽쇠괄호와 태그의 꺾쇠 괄호를 혼동하므로 보통 제네릭을 사용할때 function 키워드로 선언하는 경우가 많다.

```tsx
// 에러 발생 JSX 태그로 혼동함
const exampleFunc = <T,>(arg: T): T[] => {
  return new [1, 2, 3]();
};

// function 키워드로 선언
function exampleFunc<T>(arg: T) {
  return [1, 2, 3];
}
```

<br>

다음은 제네릭 사용법에 대해 알아보자.

<br>

### 1.제네릭 호출 시그니처

제네릭(generic) 호출 시그니처는 함수를 정의할 때 **일반적인 타입이나 값을 사용하는 대신, 제네릭 타입 매개변수를 사용**하여 타입을 유연하게 정의할 수 있는 방법을 제공한다.

<br>

- 제네릭 호출 시그니처

```tsx
// 제네릭 호출 시그니처를 사용하여 배열의 첫 번째 요소를 반환하는 함수를 정의합니다.
type FirstElementFunction<T> = (arr: T[]) => T;

// 제네릭 호출 시그니처를 사용하여 숫자 배열의 첫 번째 요소를 반환하는 함수를 정의합니다.
const getFirstNumber: FirstElementFunction<number> = (arr) => {
  return arr[0];
};

// 제네릭 호출 시그니처를 사용하여 문자열 배열의 첫 번째 요소를 반환하는 함수를 정의합니다.
const getFirstString: FirstElementFunction<string> = (arr) => {
  return arr[0];
};

const numbers: number[] = [1, 2, 3, 4, 5];
const strings: string[] = ["apple", "banana", "orange"];

const firstNumber: number = getFirstNumber(numbers); // 1
const firstString: string = getFirstString(strings); // "apple"
```

<br>

- 제네릭 타입

```tsx
function getFirstNumber(arr: number[]): number {
  return arr[0];
}

function getFirstString(arr: string[]): string {
  return arr[0];
}

const numbers: number[] = [1, 2, 3, 4, 5];
const strings: string[] = ["apple", "banana", "orange"];

const firstNumber: number = getFirstNumber(numbers);
const firstString: string = getFirstString(strings);
```

<br>

**장점:**

1. **명확한 인터페이스 정의:** 제네릭 호출 시그니처를 사용하면 함수나 메서드의 인터페이스를 더 명확하게 정의할 수 있습니다. 함수가 어떤 타입을 받고 어떤 타입을 반환하는지 명시적으로 표현할 수 있다.
2. **타입 추론의 용이성:** 제네릭 호출 시그니처를 사용하면 함수를 호출할 때 매개변수의 타입을 명시적으로 지정하지 않아도 컴파일러가 타입을 추론할 수 있습니다. 이는 코드를 더 간결하게 작성할 수 있게 해준다.
3. **코드 재사용성:** 제네릭 호출 시그니처를 사용하면 여러 함수나 메서드에서 동일한 타입을 사용할 수 있습니다. 이는 코드의 재사용성을 높여준다.

<br>

### 2.제네릭을 함수처럼 사용하기

```tsx
interface Alex {
  type: human;
  race: yellow;
  name: "alex";
  age: 28;
}

interface James {
  type: human;
  race: yellow;
  name: "james";
  age: 24;
}
```

type과 race 속성은 동일하지만 name과 age 속성은 다르다.

이럴때 제네릭을 사용해서 중복을 제거할 수 있다.

```tsx
interface Person<N,A> {
	type: human,
	race: yellow,
	name: N,
	age: A,
}

interface Alex extend Person<'alex', 28>{}
interface James extend Person<'james', 24>{}
```

인터페이스 이름 바로뒤에 제네릭을 위치시켜 사용할 수 있다.

<br>

만약 타입 매개변수의 개수와 타입 인수의 개수가 일치하지 않으면 에러가 발생한다.

```tsx
interface Alex extends Person<'alex'> // 에러
```

<br>

인터페이스 뿐만이 아니라 타입별칭, 클래스, 함수도 제네릭을 가질 수 있다.

```tsx
type Person<N, A> = {
  type: human;
  race: yellow;
  name: N;
  age: A;
};

type Alex = Person<"alex", 28>;
type James = Person<"james", 24>;
```

```tsx
class Person<N, A> {
  name: N;
  age: A;
  constructor(name: N, age: A) {
    this.name = name;
    this.age = age;
  }
}
```

<br>

**주의! 함수에서는 함수 선언문이냐 표현식이냐에 따라 제네릭 표기 위치가 달라진다.**

```tsx
const personFactoryE = <N, A>(name: N, age: A) => ({
  type: human,
  race: yellow,
  name,
  age,
});

function personFactoryD<N, A>(name: N, age: A) {
  return {
    type: human,
    race: yellow,
    name,
    age,
  };
}
```

<br>

객체나 메서드에서도 제네릭을 표기할 수 있다.

```tsx
class Person<N, A> {
  name: N;
  age: A;

  constructor(name: N, age: A) {
    this.name = name;
    this.age = age;
  }
  method<B>(param: B) {}
}

interface IPerson<N, A> {
  type: "human";
  race: "yellow";
  name: N;
  age: A;
  method: <B>(param: B) => void;
}
```

<br>

기본값도 사용할 수 있다.

```tsx
interface Person<N = string, A = number> {
  type: "human";
  race: "yellow";
  name: N;
  age: A;
  method: <B>(param: B) => void;
}

type Person1 = Person;
type Person2 = Person<number>;
type Person3 = Person<boolean, number>;
```

<br>

타입스크립트는 제네릭에 직접 타입을 넣지 않아도 추론을 통해 타입을 알아 낼 수 있다.

```tsx
const personFactoryE = <N, A>(name: N, age: A) => ({
  type: "human",
  race: "yellow",
  name,
  age,
});

const alex = personFactoryE("alex", 29);
```

변수 alex는 name 변수에 ‘alex’를 age 변수에 ‘29’를 인수로 넣었다

`name : string` ,`age: number` 이므로 N은 string, A는 number가 된다.

실제로도 직접 넣지 않는 경우가 더 많다.

<br>

상수 타입 매개변수도 추가할 수 있다.

```tsx
function values<const T>(init: T[]) {
  return {
    hasValue: (value: T) => init.includes(value),
  };
}

const saveValues = values(["a", "b", "c"]);
saveValues.hasValue("x"); // '"x"' 형식의 인수는 '"a" | "b" | "c"' 형식의 매개 변수에 할당될 수 없습니다.
```

타입 매개변수앞에 const 수식어를 붙이면 타입 매개변수 T를 추론할때 as const를 붙인 값으로 추론된다.

따라서 T는 `"a" | "b" | "c"` 형식의 매개 변수만 할당 할 수 있게 된다.

<br>

### 3.제한된 제네릭

타입 매개변수에 제약을 사용할 수 있다.

**extends 문법으로 타입 매개변수의 제약을 표시하는 방법이다.**

타입의 상속을 의미하던 extends와는 사용법이 다르다.

```tsx
interface Example<A extends number, B = string> {
  a: A;
  b: B;
}

type Usecase1 = Example<string, number>; // 에러 string이 아니라 반드시 number이여야한다.
type Usecase2 = Example<1, number>;
type Usecase3 = Example<number>;
```

여기서 A를 바운드 타입 매개변수(bounded type parameters) 라고 부른다.

그리고 B는 A의 상한 한계(upper bound)라고 한다.

<br>

Usecase1 타입에서 string 타입을 넣었더니 에러가 난다. 반드시 number 이여야한다는 뜻이다.

Usecase2 타입에서는 number보다 더 구체적인 1을 넣어서 입력할 수 있다.

<br>

**타입 매개변수에 사용하는 extends는 제약을 의미한다.**

```tsx
interface Example<A, B extends A> {
  a: A;
  b: B;
}

type Usecase1 = Example<string, number>; // A 타입이 string이므로 에러 발생
type Usecase2 = Example<string, "hello">;
type Usecase3 = Example<number, 123>;
```

Usecase1 에서 A타입이 string인데 B타입은 A타입을 extends 받았으므로 number가 들어오면 에러가 발생한다.

<br>

다음과 같은 제약들이 많이 쓰인다.

```tsx
<T extends Obejct> // 모든 객체
<T extends any[]> // 모든 배열
<T extends (...args: any) => any> // 모든 함수
<T extends abstract new (...args: any) => any> // 생성자 타입
<T extends keyof any> // string | number | symbol
```

주의해야할 점이 있다. 타입 매개변수와 제약을 동일하게 생각하는 것이다

```tsx
interface V0 {
  value: any;
}

const returnV0 = <T extends V0>(): T => {
  return { value: "test" };
};

returnV0<{ value: string; another: value }>();
```

아래처럼 T가 another인 값을 가지고 있을 수 도 있기 때문에 `<value: string>`은 T가 아니다.

<br>

강박적으로 제네릭을 쓸 필요가 없다. 특히 원시값 타입만 사용한다면 대부분 제약을 걸지 않아도 되는 경우가 많다.

```tsx
interface V0 {
  value: any;
}

const returnV0 = (): V0 => {
  return { value: "test" };
};
```

<br>

또한 유니온 타입을 상속해서 선언할 수도 있다.

```tsx
function exampleFunc<T extends string | number>(arg: T): T[] {
  return [arg];
}

exampleFunc(123);
exampleFunc("hello");
```

T는 string 또는 number타입이어야 한다.

<br>

흔하게 에러가 발생하는 제네릭 추론

```tsx
function exampleFunc<T extends string>(arg: T): T[] {
  return ["1", "2", "3"];
} // 에러가 발생함

exampleFunc("hello");
// exampleFunc<"hello">(arg: "hello"): "hello"[]
```

문자열 리터럴 타입은 문자열 타입이랑 다르다.

따라서 `T`가 추론될때 문자열 리터럴 타입으로 추론될 수 가 있으므로 `string[]`과 다르다.

예를 들어 `exampleFunc('hello')`가 호출 되었을때 T가 추론되면, `'hello'`가 `T` 타입이 되고 `'hello'[]`가 된다.

따라서 return 값은 `['hello']` 가 되어야한다.

우리가 예상한 `string[]`이 아니라서 에러가 발생한다.

따라서 위의 코드를 고치려면

```tsx
function exampleFunc<T extends string>(arg: T): string[] {
  return ["1", "2", "3"];
}

or;

function exampleFunc<T extends string>(arg: T): T[] {
  return [arg];
}
```

즉 제네릭을 사용할때는 추론되어지는 T 값을 예상해서 사용해야한다.

<br>

### 4. 제네릭 맵드 타입

만약 두개의 매개변수 타입을 가지고 객체 타입으로 표현할때 주의해야한다.

예를 들어

```tsx
type Record<K, V> = { K: V };
type Test = Record<"hello", string>;
```

위의 코드가 Test 타입이 `{ “hello” : string }` 으로 동작할 것으로 예상하지만 실제로는 `{ K : string }` 으로 나타난다.

위 코드에서는 **`K`**가 반드시 문자열, 숫자열, symbol 타입이어야 한다는 제약을 추가하여야 한다. 왜나면 객체의 key에 들어가는 값은 3개의 타입만 가능하기 때문이다.

하지만 K에 아무런 제약이 없어서 'hello'로 제한하면 컴파일러는 이를 인식하지 못하고 에러를 발생시킨다.

이는 keyof any가 허용하는 타입(string, number, symbol)이 아니기 때문이다.

이말은 string, number, symbol이 아니라 ‘hello’ 문자열 리터럴로 제한한것이이 때문이다.

<br>

그리고 두번째로 여기서 **`{ K: V }`**와 같이 사용된 것은 정적인 프로퍼티 이름인 "K"에 V 값을 할당하는 것으로 해석된다. 그러나 K는 제네릭 타입으로, 여러 가지 값 중 하나를 나타내는데 사용된다. 이렇게 사용하면 실제로는 K라는 이름의 프로퍼티를 만들고 이에 V 값을 할당하는 것이 된다.

우리가 원하는것은 K값에 따라 동적으로 프로퍼티로 할당하는 것이므로 keyof 연산자를 사용해서 K가 객체의 키 타입중 하나로 제한해야한다.

<br>

```tsx
type Record<K extends string | number | symbol, V> = { [P in K]: V };
type Test = Record<"hello", string>; // { hello : string }
```

이렇게 하면 **`K`**는 `"hello"`와 같은 문자열 키의 타입을 의미하게 된다.

<br>

### 제네릭 사용 예시(API)

제네릭의 장점은 다양한 타입을 받게 해서 코드를 효율적으로 재사용한다.

실제 현업에서 가장 많이 활용되는 곳은 API 응답 값의 타입을 지정할 때다.

```tsx
// API 응답의 형태를 정의하는 인터페이스
interface ApiResponse<T> {
  data: T;
  statusCode: number;
}

// 사용자 정보를 담는 인터페이스
interface User {
  id: number;
  name: string;
  email: string;
}

// API 호출을 시뮬레이션하는 함수
function fetchUserData(): Promise<ApiResponse<User>> {
  // 실제 API 호출을 시뮬레이션하는 코드
  const fakeApiResponse: ApiResponse<User> = {
    data: {
      id: 1,
      name: "John Doe",
      email: "john@example.com",
    },
    statusCode: 200,
  };

  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(fakeApiResponse);
    }, 1000);
  });
}

// API 호출 후 받아온 응답을 처리하는 함수
async function processUserData() {
  try {
    const response = await fetchUserData();
    console.log("User Data:", response.data);
  } catch (error) {
    console.error("Error fetching user data:", error);
  }
}

// processUserData 함수 호출
processUserData();
```

API 응답 값에 따라 data를 제네릭 타입 T로 선언하고 있다.

ApiResponse 타입은 실제 API응답 값의 타입을 지정할때 사용된다.

ApiResponse을 활용해서 코드를 효율적으로 재사용할 수 있다.

하지만 굳이 필요하지 않은 곳에서 제네릭을 사용하면 오히려 좋지 않다.

<br>

## 추론(Inference)

타입 추론(Type Inference)은 TypeScript가 코드를 분석하여 변수 또는 함수의 타입을 추측하는 기능이다. 타입을 명시적으로 지정하지 않아도 TypeScript가 컴파일러가 변수 또는 함수를 초기화할 때 그 타입을 추론한다.

```tsx
// 숫자 변수
let numberVariable = 10; // TypeScript가 number 타입으로 추론합니다.

// 문자열 변수
let stringVariable = "Hello"; // TypeScript가 string 타입으로 추론합니다.

// 배열 변수
let arrayVariable = [1, 2, 3]; // TypeScript가 number[] 타입으로 추론합니다.

// 객체 변수
let objectVariable = { name: "John", age: 30 }; // TypeScript가 { name: string, age: number } 타입으로 추론합니다.

// 함수
function add(a: number, b: number) {
  return a + b;
}
let result = add(3, 5); // TypeScript가 함수의 반환 값의 타입을 number로 추론합니다.
```

코드를 간결하게 유지하고 타입 안정성을 확보할 수 있다.

하지만 매개변수에는 타입을 부여핸다 어떤값이 들어오는지 모르기 때문이다.

<br>

타입스크립트의 추론을 타입스크립트가 제대로 추론하면 그대로 쓰고, 틀리게 추론할떄만 올바른 타입을 표기하는것이 좋다.

<br>

다음과 같은 경우에는 타입 추론을 사용하는 대신 명시적으로 타입을 지정하는 것이 좋다.

1. **코드의 가독성이 떨어지는 경우**: 타입 추론을 사용하면 코드의 가독성이 떨어질 수 있다. 특히 **코드가 복잡하고 이해하기 어려운 경우에는 명시적으로 타입을 지정하여 코드를 명확하게 만드는 것이 좋다.**
2. **타입이 명확하지 않은 경우**: **타입 추론이 잘못된 결과를 내는 경우가 있다.** 특히 다양한 타입의 조합이 있는 경우에는 타입 추론을 사용하기 어려울 수 있다. 이런 경우에는 명시적으로 타입을 지정하여 정확한 타입을 사용하는 것이 좋다.
3. **코드의 안정성이 필요한 경우**: 타입 **추론은 코드의 안정성을 보장하지 않을 수 있다.** 특히 타입 추론이 잘못된 결과를 내는 경우에는 코드의 안정성이 떨어질 수 있다. 이런 경우에는 명시적으로 타입을 지정하여 코드의 안정성을 확보하는 것이 좋다.

<br>

따라서, 타입 추론을 사용할 때에는 코드의 복잡성과 안정성을 고려하여 타입 추론을 적절히 활용하는 것이 중요하다. **간단한 경우에는 타입 추론을 사용하여 코드를 간결하게 작성할 수 있지만, 복잡한 경우에는 명시적으로 타입을 지정하여 코드의 가독성과 안정성을 확보하는 것이 좋다.**

<br>

### 주의해야할 점

타입 매개변수가 두개일때 제네릭을 사용하는 것이 좋다.

무분별한 제네릭 사용은 코드 가독성을 해친다.

```jsx
function getName<Type>(name: Type): Type {
  return name;
}

getName("Alex");
```

<br>

굳이 제네릭을 사용하지 않아도 된다.

```jsx
function getName(name: string): string {
  return name;
}

getName("Alex");
```

<br>

참고

- https://www.typescriptlang.org/ko/docs/handbook/typescript-in-5-minutes.html
- https://yamoo9.gitbook.io/typescript/types/primitives
- [https://radlohead.gitbook.io/typescript-deep-dive](https://radlohead.gitbook.io/typescript-deep-dive/type-system)
- https://ui.toast.com/posts/ko_20220323

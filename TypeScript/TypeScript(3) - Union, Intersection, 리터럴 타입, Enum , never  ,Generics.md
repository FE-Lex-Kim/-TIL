- [TypeScript(3) - Union, Intersection, 리터럴 타입, Enum , never ,Generics](#typescript3---union-intersection-리터럴-타입-enum--never-generics)
  - [유니온 (Union)](#유니온-union)
  - [교차 (Intersection) 타입](#교차-intersection-타입)
  - [리터럴 타입](#리터럴-타입)
  - [Enum 타입](#enum-타입)
  - [never](#never)
  - [제네릭 함수 (Generics)](#제네릭-함수-generics)
    - [추론(Inference)](#추론inference)
    - [타입 인수 명시하기](#타입-인수-명시하기)
    - [주의해야할 점](#주의해야할-점)

# TypeScript(3) - Union, Intersection, 리터럴 타입, Enum , never ,Generics

<br>

## 유니온 (Union)

여러 타입의 합집합을 의미하는 Union이다.

```jsx
const person = (name: string | undefined, age: number | string): string => {
  return `my name is ${name}, I'm ${age}`;
};
console.log(person("alex", 29));
```

name에 string과 undefined,

age에 number와 string 모두 가능하게 하려면 파이프( | ) 를 사용해서 설정한다.

이를 유니온이라고 부른다.

<br>

## 교차 (Intersection) 타입

여러 타입의 교집합을 의미하는 Intersection 타입이다.

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

## never

nerver 절대로 발생하지 않는 값의 종류를 나타낸다.

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

<br>

## 제네릭 함수 (Generics)

함수, 타입, 클래스 등에서 내부적으로 사용할 타입을 미리 정해두지 않고 타입 변수를 사용해서 해당 위치를 비워 둔 다음에, 실제로 그 값을 사용할때 외부에서 타입 변수 자리에 타입을 지정하여 사용하는 방식을 말한다.

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

제네릭을 사용할때 TypeScript에서 JSX(JavaScript XML)를 사용하는 파일의 확장자는 **`.tsx`**입니다. JSX는 React와 같은 라이브러리에서 컴포넌트를 작성할 때 사용되는 문법입니다.
따라서 제네릭의 꺽쇠괄호와 태그의 꺾쇠 괄호를 혼동하므로 보통 제네릭을 사용할때 function 키워드로 선언하는 경우가 많다.

<br>

### 추론(Inference)

위의 예제에서 Type을 특정하지 않았다.

여기서 타입은 “추론” 되었다고 할 수 있다.

즉, TypeScript에 의해서 자동적으로 선택된 것이다.

<br>

이번에는 여러 개의 타입 매개변수때는 어떻게 하는지 알아보자.

```jsx
function map<Input, Output>(arr: Input[], func: (arg: Input) => Output): Output[] {
  return arr.map(func);
}

const parsed = map(["1", "2", "3"], (n) => parseInt(n));
console.log("parsed: ", parsed); // parsed:  [ 1, 2, 3 ]
```

`String[]` 이므로 input 타입은 string

`parseInt()`가 number 타입으로 나와서 output은 number

<br>

input 타입과 output 타입을 함수 표현식의 반환값를 통해서 추론할 수 있다.

<br>

### 타입 인수 명시하기

제네릭에서 항상 의도된 타입을 추론하지만, 항상 그렇지만은 않다.

```jsx
function combine<Type>(arr1: Type[], arr2: Type[]): Type[] {
  return arr1.concat(arr2);
}
```

<br>

아래와 같이 number, string 배열로 둘다 타입이 다르다.

```jsx
const arr = combine([1, 2, 3], ["hello"]);
// Type 'string' is not assignable to type 'number'.
```

<br>

이럴때 수동으로 타입을 명시해야한다.

```jsx
function combine<Type>(arr1: Type[], arr2: Type[]): Type[] {
  return arr1.concat(arr2);
}

const arr = (combine < number) | (string > ([1, 2, 3], ["hello"]));
```

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

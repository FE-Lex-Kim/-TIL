- [TypeScript(2) - Union, Generics](#typescript2---union-generics)
  - [유니온 (Union)](#유니온-union)
  - [제네릭 함수 (Generics)](#제네릭-함수-generics)
    - [추론(**Inference)**](#추론inference)
    - [타입 인수 명시하기](#타입-인수-명시하기)
    - [주의해야할 점](#주의해야할-점)

# TypeScript(2) - Union, Generics

여러가지 타입을 이용해서 새 타입을 작성하기 위해서는 유니언(Union)과 제네릭(Generic)이 있다.

<br>

## 유니온 (Union)

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

## 제네릭 함수 (Generics)

**입력 값이 출력 값의 타입과 관련이 있거나, 두 입력값의 타입이 서로 관련이 있는 형태의 함수를 작성하는 경우 사용할 수 있다.**

<br>

```jsx
function firstElement(arr: any[]) {
  return arr[0];
}
```

입력값과 반환값이 any 값이 된다.

함수가 입력값인 배열의 원소 타입을 반환하는 타입으로 정하면 더 좋을 것같다.

```jsx
// function firstElement(arr: any[]) {
//   return arr[0];
// }

function firstElement<Type>(arr: Type[]): Type | undefined {
  return arr[0];
}

const s = firstElement(["a", "b", "c"]);
// n은 "number" 타입
const n = firstElement([1, 2, 3]);
// u는 "undefined" 타입
const u = firstElement([]);
```

<br>

### 추론(**Inference)**

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
- [https://velog.io/@skulter/TypeScript-5.-배열과-튜플-pg99bu8g](https://velog.io/@skulter/TypeScript-5.-%EB%B0%B0%EC%97%B4%EA%B3%BC-%ED%8A%9C%ED%94%8C-pg99bu8g)

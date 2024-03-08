- [TypeScript(5) - 조건부 타입(컨디셔널 타입),함수 메서드 타이핑, 오버로딩](#typescript5---조건부-타입컨디셔널-타입함수-메서드-타이핑-오버로딩)
  - [조건부 타입(컨디셔널 타입)](#조건부-타입컨디셔널-타입)
    - [제네릭 타입 제한](#제네릭-타입-제한)
    - [조건부 타입 검사](#조건부-타입-검사)
    - [조건부 타입 분배법칙](#조건부-타입-분배법칙)
  - [함수와 메서드 타이핑](#함수와-메서드-타이핑)
    - [일반 매개변수](#일반-매개변수)
    - […나머지문법](#나머지문법)
    - [전개 문법(spread)](#전개-문법spread)
    - [구조분해할당](#구조분해할당)
  - [오버로딩](#오버로딩)

# TypeScript(5) - 조건부 타입(컨디셔널 타입),함수 메서드 타이핑, 오버로딩

<br>

## 조건부 타입(컨디셔널 타입)

조건에 따라 다른 타입이 되는 조건부 타입이 있다.

```tsx
type A1 = string;
type B1 = A1 extends string ? number : boolean; // type B1 = number;

type A2 = number;
type B2 = A2 extends string ? number : boolean; // type B2 = boolean;
```

**extends 예약어를 사용해서 삼항연산자와 같이 사용한다.**

위 예제에서 A1은 string이 맞으므로 B1 은 number type이 된다.

<br>

```tsx
interface X {
  x: number;
}

interface XY {
  x: number;
  y: number;
}

type A = XY extends X ? string : number; // type A = string
```

쉽게 이해하려면 A extends B 라고 하면

**→ A는 B가 가진 모든 타입을 가지고 있으면 true이다.**

<br>

**A extends B 가 참 or 거짓 조건**

```tsx
// true
type Result1 = "hi" extends string ? true : false;
type Result2 = string extends string ? true : false;
type Result3 = never extends string ? true : false;

type Result4 = string | never extends string ? true : false;
type Result5 = never | never extends string ? true : false;

type Result6 = string extends string | number ? true : false;
type Result7 = "hi" extends string | number ? true : false;
type Result8 = "hi" extends string | never ? true : false;
type Result9 = string extends string | never ? true : false;
type Result10 = never extends string | never ? true : false;

// false
type Result1 = string | number extends string ? true : false;
type Result2 = string | number extends never ? true : false;
type Result3 = string extends never ? true : false;
type Result4 = string extends "hi" ? true : false;
type Result5 = string | never extends never ? true : false;
```

1. **A extends B 가 항상 true가 되려면 B가 A보다 커야한다.**

   왜냐면 A를 확장한게 B이기 때문이다.

   문법도 A among B라면 → B중에 A 라고 해석 되기도 하기 때문이다.(?) → 그냥 extends 키워드 이해하기 쉽게 이렇게 이해해보자… ㅠㅠ

2. **유니온에서 never은 항상 무시한다.**

<br>

### 제네릭 타입 제한

제네릭과 extends를 함께 사용해서 제네릭으로 받는 타입을 제한할 수 있다.

따라서 개발자는 잘못된 값을 넘길 수 없기 때문에 휴먼 에러를 방지할 수 있다.

```tsx
const Alex = <T extends "Alex" | "James" | "Andrew">(name: T) => {
  type Alex = {
    name: string;
    age: number;
    hasGirlfriend: boolean;
  };

  type James = {
    name: string;
    age: number;
    hobby: string;
  };

  type Andrew = {
    name: string;
    age: number;
  };

  type NameInfo = T extends "Alex" ? Alex : T extends "James" ? James : Andrew;

  function getNameInfo(name: "Alex" | "James" | "Andrew"): NameInfo {
    if (name === "Alex") {
      return { name: "Alex", age: 29, hasGirlfriend: true } as NameInfo;
    } else if (name === "James") {
      return { name: "James", age: 24, hobby: "youtube" } as NameInfo;
    } else {
      return { name: "Andrew", age: 22 } as NameInfo;
    }
  }

  return <h1>hello</h1>;
};
export default Alex;
```

제네릭 타입 매개변수를 제한해서 “Alex”, “James”, “Andrew” 문자열 리터럴 타입만 받게했다.

getNameInfo 반환값에 대한 타입을 NameInfo 으로 했으며, 각각 제네릭 타입매개변수 T에 따라서 Alex, James, Andrew 타입을 받게 했다.

**이때 getNameInfo 함수 내부에서 조건문 if-else 문을 사용을 했는데, 조건부 에서 반환하는 값은 타입스크립트가 추론할수가 없어서 as NameInfo를 해주어야한다.**

<br>

### 조건부 타입 검사

조건부 타입은 타입검사를 위해 많이 사용한다.

```tsx
type Result = "h1" extends string ? true : false; // type Result = true
type Result2 = [1] extends [string] ? true : false; // type Result2 = false
```

hi가 string 타입인지 확인가능하고 [1]이 [string] 타입이 아님을 확인할수있다.

<br>

아래 Omit은 특정 타입인 속성을 제거하는 타입이다. 예제에서는 `boolean`을 제거하고 있다.

키가 `never` 타입이 된다면 해당 속성은 제거된다.

```tsx
type Omit<O, T> = {
  [K in keyof O as O[K] extends T ? never : K]: O[K];
};

type Result = Omit<
  {
    name: string;
    age: number;
    married: boolean;
    rich: boolean;
  },
  boolean
>;
```

여기서 사용된 표현들을 하나씩 살펴보자.

- **`keyof O`**: 객체 **`O`**의 모든 속성의 이름을 문자열로 가져온다
- **`O[K]`**: 객체 **`O`**에서 속성 **`K`**에 해당하는 값의 타입을 가져온다
- **`O[K] extends T ? never : K`**: 속성 **`K`**의 값이 타입 **`T`**를 확장한다면(**`extends T`**), 해당 속성은 제외하고 **`never`** 타입으로 지정한다. 그렇지 않으면, 속성 이름 **`K`**를 그대로 사용한다.
- **`[K in ...]: ...`**: 객체 속성의 이름을 순회하면서 해당 조건을 적용한다.
- **`as O[K] extends T ? never : K`** : 조건부 타입이 평가된 후에 그 결과를 **`never`** 또는 속성 이름 **`K`**로 변환하는 역할을 한다.

<br>

### 조건부 타입 분배법칙

다음 코드에서 string | number 을 사용해서 string[] 타입을 얻으려는 상황이다.

```tsx
type Start = string | number;
type Result = Start extends string ? Start[] : never;
```

하지만 Result는 항상 never을 만든다.

`string | number`가 `string`을 **항상** extends 할 수 없기 때문이다.

`Start`가 `string`보다 더 범위가 좁기 때문이다.

<br>

string extends Start로 바꾸시면 항상 `Start[]`이다.

**타입에서 조건은** 그 타입들의 구현체를 고려해서 **동적으로 결정되는게 아니다.**

런타임에선 어차피 타입없는 js로 돌아가기 때문에, **컴파일타임에 타입은 이미 결정된다.**

<br>

예를 들자면

```tsx
const a: Start = "a";
const b: string = a;
```

이건 가능하다. a는 이미 스트링으로 결정되어있기 때문이다.

<br>

그런데

```tsx
function fn(a: Start) {
  const b: string = a;
}
```

이건 타입에러가 뜬다.
이 관계가 충족이되어야 extends 조건이 참이된다.

<br>

따라서 아래코드를 바꾸려면

```tsx
type Start = string | number;
type Result = Start extends string ? Start[] : never;
```

다음코드로 제네릭으로 변경하면된다.

```tsx
type Start = string | number;
type Result<Key> = Key extends string ? Key[] : never;
let result: Result<Start> = ["hi"];
```

**만약 검사하려는 타입이 제네릭이면서 유니언이면 분배법칙이 실행된다.**

`Result<string | number>`는 `Result<string> | Result<number>`이 된다.

따라서 그 결과 타입이 `string[] | never` 이 되고 `never`은 사라져서 `string[]` 타입이 된다.

<br>

하지만 boolean에 분배법칙이 적용될때는 다르다.

```tsx
type Start = string | number | boolean;
type Result<Key> = Key extends string | boolean ? Key[] : never;
let n: Result<Start> = ["hi"];

// -> 분배 법칙에 따라 다음과 같이 실행된다
// let n: Result<string> = ["hi"]; // string[]
// let n: Result<number> = ["hi"]; // never
// let n: Result<false> = ["hi"]; // false[]
// let n: Result<true> = ["hi"]; // true []
// 따라서 never만 사라지고 나머지는 합쳐진다 string[] | false[] | true[]
```

`string[] | boolean[]` 이 될것이라는 예상과 달리 `string[] | false[] | true[]` 가 된다.

`boolean`은 `true | false`로 인식하기 때문이다.

<br>

분배법칙이 일어나는것을 막고싶을 수도 있다.

```tsx
// 'hi' | 3 이 string인지 검사하는 타입
type HiThree = "hi" | 3;
type IsString<T> = T extends string ? true : false;
type Result = IsString<HiThree>;
```

IsString<'hi’> | IsString<3> 이고 true | false로 결과 값이 나온다.

그러므로 합쳐져서 boolean이 된다.

<br>

**이럴때는 분배법칙이 일어나지 않게하려면 배열로 제네릭을 감싸면된다.**

```tsx
type IsString<T> = [T] extends [string] ? true : false;
type Result = IsString<"hi" | 3>;
```

**`[T]`**와 **`[string]`**을 각각 튜플로 선언하여 비교하고 있다.
그런 다음 **`Result`** 타입은 **`IsString<"hi" | 3>`**으로 설정된다.

**`IsString`** 타입에 대한 결괏값은 **`[ "hi" | 3 ]`**이며, 이는 **`string`** 타입의 튜플과 **`string`** 타입의 튜플이 서로 일치하지 않으므로 **`false`**가 된다.

`IsString[’hi’] | IsString[3]` 같이 분배법칙으로 동작하지 않는다.

<br>

하지만 주의할점이 있다

```tsx
type R<T> = T extends string ? true : false;
type RR = R<never>;
```

**`never`** 타입은 절대 발생하지 않는 타입을 나타내며, 어떤 값도 가질 수 없다. 따라서 **`R<never>`**는 무조건적으로 **`false`**가 된다. 왜냐하면 **`never`** 타입은 어떠한 조건에도 만족하지 않기 때문이다.

<br>

하지만 또 웃기게 이건 참이다.튜플을 서로 비교했기 때문이다.

```tsx
type R<T> = [T] extends [string] ? true : false;
type RR = R<never>;
```

<br>

## 함수와 메서드 타이핑

<br>

### 일반 매개변수

```tsx
function example(a: string, b?: number, c = false) {}

example("hi", 123, true);
example("hi", 123);
example("hi");
```

b 매개변수에 옵셔널 수식어가 있다. b의 값이 없어도 상관없다는 의미이다.

c는 기본값이 제공되어 있으므로 마찬가지이다.

<br>

### …나머지문법

매개변수는 …나머지 문법을 사용할 수 있다.

```tsx
function example1(a: string, ...b: number[]) {}
function example2(...a: string, b: number[]) {} // 에러
```

나머지 매개변수는 반드시 배열 또는 튜플이어야 한다.

나머지 매개변수는 항상 마지막 매개변수에 위치해야한다.

<br>

### 전개 문법(spread)

매개변수 자리에 전개 문법(spread)을 사용할 수 있다.

```tsx
// 두개다 같은 방법임
function example1(...a: [number, string, boolean]) {}
function example1(...a: [a: number, b: string, c: boolean]) {}
example1(1, 2, 3); // 에러 두번째 매개변수가 string이 여야함
example1(1, 2, 3, 4); // 4번째는 없어서 안됌
```

<br>

### 구조분해할당

구조분해할당을 타이핑하려면 다음과 같다.

```tsx
function destructuring({ props: { nested } }) {} // 두번연속 구조분해할당을 사용함
->

function destructuring({ props: { nested } }: { props: { nested: number } }) {}
```

## 오버로딩

타입스크립트에서 함수 오버로딩은 하나의 함수에 여러 함수를 정의하여 다양한 매개변수 유형을 지원하는 기능이다.

예를 들어 다음과 같이 코드를 작성하면 에러가 발생한다.

```tsx
function add(x: string | number, y: string | number): string | number {
  return x + y; //'+' 연산자를 'string | number' 및 'string | number' 형식에 적용할 수 없습니다.
}

add(1, 2);
add("1", "2");
add(1, "2");
add("1", 2);
```

x와 y 모두 string | number이 될 수 있기 때문이다.

이럴때 오버로딩이 필요하다.

<br>

동일한 함수가 다양한 타입의 입력을 처리하고 다른 동작을 수행해야 하는 경우에 오버로딩이 자주 사용될 수 있다.

오버로딩이 무엇인지는 예제를 보면 더 잘알 수 있다.

```tsx
function add(x: number, y: number): number;
function add(x: string, y: string): string;
function add(x: any, y: any) {
  return x + y;
}

add(1, 2); // 3
add("1", "2"); // 12
add(1, "2"); // 에러
add("1", 2); // 에러
```

여기서 any를 제거하면 에러가 발생해서 어쩔수 없이 넣어야한다.

<br>

타입스크립트에서 함수 오버로딩은 컴파일러가 함수 호출 시, 선언된 순서대로 우선순위가 결정된다.

즉, 함수를 호출할 때 매개변수 유형과 일치하는 첫 번째 오버로딩을 선택한다.

<br>

따라서 오버로딩의 순서는 좁은 타입부터 넓은 타입순으로 오게해야 문제가 없다.

```tsx
function example(param: string): string;
function example(param: string | null): number;
function example(param: string): string | number {
  if (param) {
    return "string";
  } else {
    return 123;
  }
}

const result = example("what"); // string
```

```tsx
function example(param: string | null): number;
function example(param: string): string;
function example(param: string): string | number {
  if (param) {
    return "string";
  } else {
    return 123;
  }
}

const result = example("what"); // number
```

순서를 바꾸면 첫번째는 string, 두번째는 number가 된다.

<br>

타입스크립트에서 오버로딩은 다음과 같은 상황에서 유용하게 사용된다.

1. **라이브러리와 프레임워크 개발**:
   - 라이브러리나 프레임워크를 개발할 때, 다양한 유형의 입력을 처리해야 할 때 오버로딩이 유용하다. 이는 사용자가 다양한 유형의 입력을 전달할 수 있도록 하여 라이브러리의 유연성을 향상시킨다.
2. **다양한 목적의 함수**:
   - 함수가 다양한 타입의 입력을 받아들이고 해당 입력에 따라 다른 동작을 수행해야 할 때 오버로딩이 유용하다. 이는 함수의 동작을 명확하게 정의하고, 코드의 가독성을 높여준다.
3. **타입 안정성 확보**:
   - 오버로딩을 사용하여 입력 타입에 따라 타입 안정성을 보장할 수 있다. 이는 잘못된 타입의 입력이 전달되는 것을 방지하고, 코드의 안정성을 높여준다.

그러나 오버로딩은 항상 필요한 것은 아니다. 간단한 함수나 메서드의 경우에는 오버로딩보다는 타입 가드나 다른 타입 추론 메커니즘을 사용하는 것이 더 간단하고 효율적일 수 있다.

따라서 프로젝트의 요구 사항과 특성에 따라 오버로딩을 사용할 빈도가 달라질 수 있다. 일반적으로 라이브러리 개발이나 다양한 유형의 입력을 처리해야 할 때 오버로딩이 자주 사용된다.
참고

<br>

- 타입스크립트 교과서를 참고해서 정리한 글입니다.

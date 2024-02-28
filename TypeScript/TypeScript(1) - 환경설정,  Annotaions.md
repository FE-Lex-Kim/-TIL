- [TypeScript(1) - 환경설정, (객체, 함수, 원시타입, 배열, any, null, undefined, void 타입 지정하기), as 키워드](#typescript1---환경설정-객체-함수-원시타입-배열-any-null-undefined-void-타입-지정하기-as-키워드)
  - [TypeScript를 사용하는 이유](#typescript를-사용하는-이유)
  - [설치방법](#설치방법)
  - [Annotations](#annotations)
    - [객체에 타입 선언하기](#객체에-타입-선언하기)
    - [함수에 타입 지정하기](#함수에-타입-지정하기)
    - [원시타입에 타입 지정하기](#원시타입에-타입-지정하기)
    - [배열에 타입 지정하기](#배열에-타입-지정하기)
  - [특별한 타입(any, null, undefined, void 타입 지정하기)](#특별한-타입any-null-undefined-void-타입-지정하기)
    - [any 타입 지정하기](#any-타입-지정하기)
    - [null, undefined 타입 지정하기](#null-undefined-타입-지정하기)
    - [void 타입 지정하기](#void-타입-지정하기)
  - [as 키워드](#as-키워드)

# TypeScript(1) - 환경설정, (객체, 함수, 원시타입, 배열, any, null, undefined, void 타입 지정하기), as 키워드

<br>

## TypeScript를 사용하는 이유

Javascript는 string, number, object, undefined 같은 원시 타입을 가지고 있지만, 전체 코드베이스 에서 일관되게 할당되어 있는지 미리 확인해 주지 않는다.

**TypeScript는 일관된 코드와 예상치 못한 값을 할당하지 않도록 도와주어 손쉬운 디버깅이 가능해진다.**

<br>

또한, Javascript로 코드를 작성하면 함수의 매개변수로 들어오는 값이 무엇인지 알기위해 검색을 해야한다.

**하지만 TypeScript를 사용해서 변수의 이름과 데이터의 자료형 까지 알 수 있게 되어 직관적이고 높은 생산성을 발휘한다.**

<br>

## 설치방법

```bash
npm install typescript --save-dev
```

<br>

## Annotations

Annotation이란 주석이란 사전적 뜻을 가지고 있다.

**TypeScript에서는 변수, 함수 등등 데이터 타입을 정의하기 위해서 TypeAnnotation 을 사용한다.**

타입에 annotaion을 선언해서 해당 선언된 타입만을 사용할 수 있다.

타입에 주석을 단다 라고 생각하면된다.

<br>

### 객체에 타입 선언하기

```jsx
let alex: Alex = {
  name: "Alex",
  age: 29,
};
```

<br>

이 객체를 명시적으로 나타개기 위해서는 `interface`로 선언한다.

```jsx
interface Alex {
  name: string;
  age: number;
}

let alex: Alex = {
  name: "Alex",
  age: 29,
};
```

이제 name과 age가 인터페이스에 맞지않는 객체를 생성하면 TypeScript는 경고를 준다.

<br>

### 함수에 타입 지정하기

함수에서 매개변수와 리턴값을 명시하는데도 사용한다.

```jsx
let user: string = "Alex";

function getUser(): user {
  //... 리턴값 명시
}

function deleteUser(user: user) {
  // ... 매개변수 명시
}
```

<br>

모든 속성을 `?` 를 사용해 옵셔널 속성으로 지정할 수 있다.

옵셔널 속성으로 타입을 정의하면 해당 속성이 존재하지 않아도 문제없이 작동한다.

```jsx
function getName(name?: string, age?: number): string {
  return name + " " + age;
}

getName("Alex");
```

<br>

### 원시타입에 타입 지정하기

```jsx
let Name: string = "alex";
let age: number = 29;
let handsome: boolean = true;

Name = "james";
Name = 29; // Error

age = 30;
age = "30"; // Error

handsome = false;
handsome = "false"; // Error
```

<br>

### 배열에 타입 지정하기

```jsx
let nameArr: string[] = ["alex", "james", "andrew"]; // Array<string> 와 같은 의미
let ageArr: number[] = [29, 30, 24]; // Array<number> 와 같은 의미

console.log(nameArr);
console.log(ageArr);
```

Array<타입> 으로 해도된다. 여기서 Array는 타입스크립트에서 내장된 타입이다.

내장된 제네릭 타입에는 다음과 같은 것들이 있다.

1. **`Array<T>`**: 배열을 나타내며, **`T`**는 배열의 요소 타입이다.
2. **`Promise<T>`**: 비동기 작업의 결과를 나타내는 프로미스를 나타내며, **`T`**는 프로미스가 이행될 때의 값의 타입이다.
3. **`ReadonlyArray<T>`**: 읽기 전용 배열을 나타내며, **`T`**는 배열의 요소 타입이다. 이 배열은 수정할 수 없다.
4. **`Map<K, V>`**: 키-값 쌍의 집합을 나타내는 맵을 나타내며, **`K`**는 키의 타입이고 **`V`**는 값의 타입이다..
5. **`Set<T>`**: 고유한 요소로 이루어진 집합을 나타내며, **`T`**는 요소의 타입이다.

<br>

## 특별한 타입(any, null, undefined, void 타입 지정하기)

<br>

### any 타입 지정하기

`any`**는 모든 타입과 호완되며 컴파일러에 포함되지 않는다.**

따라서 특정 값으로 인하여 **타입 검사 오류가 발생하는 것을 원하지 않을 때** 사용할 수 있다.

```jsx
let user: any = "alex";
user = "james";
user = 29;

let age: number = 29;
user = age;
```

any는 코드의 특정라인에 문제가 없다고 판단되면 굳이 TypeScript를 사용하지 않고 긴타입을 새로 정의 하고 싶지 않을때 사용하면 유용하다.

또한

자바스크립트 프로젝트를 타입스크립트 프로젝트로 변경하는 전략으로 사용하는 경우도 많다.

<br>

하지만 타입체크 기능이 작동하지 않기 때문에 타입스크립트 장점을 활용할 수 없다.

따라서 기본적으로는 any를 사용하지 않는것이 좋다.

<br>

### null, undefined 타입 지정하기

`null`과 `undefined`는 `strictNullChecks` 옵션의 설정에 따라 달라진다.

1. `strictNullChecks` 가 설정되지 않았을때

   null 또는 undefined는 any와 동일한 방식으로 동작한다.

   하지만 null 을 검사하지 않으므로 버그의 주요원인이 될 수 있어 `strictNullChecks` 을 설정하는 것이 좋다.

2. `strictNullChecks` 가 설정 되었을 때

   null 또는 undefined인 값인 프로퍼티 거나 매개변수를 검사를 해야한다.

   ```jsx
   function doSomething(x: string | undefined) {
     if (x === undefined) {
       // 아무 것도 하지 않는다
     } else {
       console.log("Hello, " + x.toUpperCase());
     }
   }
   ```

<br>

### void 타입 지정하기

`:void`는 함수에서 아무것도 리턴하는 타입이 없을 경우 사용한다.

```jsx
function log(message): void {
  console.log(message);
}
```

<br>

```tsx
const func = (): void => 1;

console.log(func()); // 1
```

이것은 TypeScript에서 반환값의 타입이 명시적으로 void로 지정되어 있더라도 함수가 실제로 값을 반환할 수 있다는 것을 보여준다.

이것은 TypeScript에서 반환 타입이 void라고 명시했다 하더라도, 함수가 반환하는 값의 유효성을 체크하지 않는다는 것을 의미한다.

따라서 다음과 같이 함수 전체의 타입을 표기해야한다.

```tsx
type Func = () => void;
const func: Func = () => 1;
```

- 사용자가 함수의 반환값을 사용하지 못하도록 제한한다.
- 반환값을 사용하지 않는 콜백함수를 타이핑 할때 사용한다.

<br>

## as 키워드

이 연산자는 변수의 타입을 개발자가 **명시적으로 지정하거나 변환할 때 사용된다.**

1. **타입 단언(Type Assertion)**: 변수의 타입을 개발자가 명시적으로 지정한다. 이는 개발자가 해당 변수가 특정한 타입임을 확신할 때 사용된다. 예를 들어, **`variable as Type`**와 같이 사용됩니다.
2. **타입 변환(Type Conversion)**: 변수의 값을 다른 타입으로 변환한다. 이는 주로 호환되지 않는 타입 간 변환 시 사용된다.

```tsx
interface Person {
  name: string;
  age: number;
}

let obj = {};
obj.name = "alex"; // 에러
obj.age = 29; // 에러
```

위의 코드는 에러가 발생한다. obj를 빈 객체로 초기화 했기 때문이다. 해당 객체는 어떤 속성이 들어갈지 알 수 없기 떄문에 이후 추가되는 속성들은 모두 있어서는 안될 속성으로 간주한것이다. 이문제를 해결하려면 선언하는 시점에 속성을 정의하거나 변수 타입을 Person으로 정의하면된다.

```tsx
interface Person {
  name: string;
  age: number;
}

let obj: Person = {
  name: "alex",
  age: 29,
};
```

<br>

하지만 이런 방법이 아니더라도 에러를 해결할 수 있다.

```tsx
interface Person {
  name: string;
  age: number;
}

let obj = {} as Person;

obj.name = "alex";
obj.age = 29;
```

변수를 선언할 때 빈 객체로 선언했지만, Person 인터페이스의 속성이라고 타입스크립트에게 말해준다.

이렇게 as를 사용하면 타입스크립트가 알기 어려운 타입에 대해 힌트를 제공할 수 있다.

또한 선언하는 시점에 name과 age 속성을 정의하지 않고 추후에 정의할 수 있는 유연함도 가질수 있다.

<br>

하지만 as를 남용하면 실행 시점의 에러에 취약해 질 수 있다.

따라서 가급적 as를 사용하기보다 타입스크립트가 추론하게 하거나 타이핑을 하는 것이 좋다.

<br>

참고

- https://www.typescriptlang.org/ko/docs/handbook/typescript-in-5-minutes.html
- https://www.elancer.co.kr/blog/view?seq=183
- [https://radlohead.gitbook.io/typescript-deep-dive](https://radlohead.gitbook.io/typescript-deep-dive/type-system)
- [https://velog.io/@skulter/TypeScript-5.-배열과-튜플-pg99bu8g](https://velog.io/@skulter/TypeScript-5.-%EB%B0%B0%EC%97%B4%EA%B3%BC-%ED%8A%9C%ED%94%8C-pg99bu8g)
- [https://joshua1988.github.io/ts/guide/type-inference.html#문맥상의-타이핑-contextual-typing](https://joshua1988.github.io/ts/guide/type-inference.html#%EB%AC%B8%EB%A7%A5%EC%83%81%EC%9D%98-%ED%83%80%EC%9D%B4%ED%95%91-contextual-typing)
- 쉽게 시작하는 타입스크립트를 참고했습니다.

- [TypeScript(1) - 환경설정, Annotaions](#typescript1---환경설정-annotaions)
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

# TypeScript(1) - 환경설정, Annotaions

<br>

## TypeScript를 사용하는 이유

Javascript는 string, number, objectg, undefined 같은 원시 타입을 가지고 있지만, 전체 코드베이스 에서 일관되게 할당되어 있는지 미리 확인해 주지 않는다.

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

**any는 코드의 특정라인에 문제가 없다고 판단되면 굳이 TypeScript를 사용하지 않고 긴타입을 새로 정의 하고 싶지 않을때 사용하면 유용하다.**

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

참고

- https://www.typescriptlang.org/ko/docs/handbook/typescript-in-5-minutes.html
- https://www.elancer.co.kr/blog/view?seq=183
- [https://radlohead.gitbook.io/typescript-deep-dive](https://radlohead.gitbook.io/typescript-deep-dive/type-system)
- [https://velog.io/@skulter/TypeScript-5.-배열과-튜플-pg99bu8g](https://velog.io/@skulter/TypeScript-5.-%EB%B0%B0%EC%97%B4%EA%B3%BC-%ED%8A%9C%ED%94%8C-pg99bu8g)

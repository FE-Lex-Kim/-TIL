# type

<br>

## any

<br>

any는 어떤 타입이 와도 작업을 할수있게해준다.

any는 꼭 필요한 상황이 아니라면 사용하지 않는게 좋다.

<br>

타입스크립트는 any를 최후의 보루라 가급적 사용하지 않는것을 추천한다.

<br>

```tsx
let a: any = 111;
let b: any = ["danget"];
let c = a + b;
```

c는 에러가 발생할것 같지만 any는 어떤값이 오더라도 에러가 발생하지 않는다.

<br>

## unknown

unknown은 어떤 타입이 와도 상관없지만

정제된이후에는 그 타입으로 변하게 된다.

<br>

이점이 any와 다르게된다.

<br>

## Boolean

<br>

boolean 타입은 true, false 두개의 값을 갖는다.

이탑으로는 비교연산과 반전연산만 가능하다.

<br>

값이 boolean 타입인지 알려주는 방법은 4가지가 있다.

<br>

1. 어떤 값이 boolean 인지 타입스크립트가 추정하게한다. `let a = true`
2. 어떤 값이 특정 booleand 타입스크립트가 추론하게 한다. `const c = true`
3. 값이 boolean임을 명시적으로 타입스크립트에게 알린다. `let d: boolean = true`
4. 값이 특정 boolean임을 명시적으로 타입스크립트에 알린다. `let e:true = true`

<br>

Boolean은 어노테이션을 대부분 사용하지 않고 타입스크립트가 추론하게하여 Boolean 타입을 지정하게한다.

<br>

즉, 1번과 2번 방법으로 보통 사용한다.

<br>

**let과 const를 사용하여 타입스크립트가 자동으로 변수의 타입을 리터럴로 추론한다.**

<br>

## number

<br>

보통 boolean과 마찬가지로 타입스크립트가 값이 number임을 추론하게 한다. `const c = 5`

<br>

## bigint

<br>

자바스크립트에서 정수로 만 사용하는 타입으로 정의 할수있게 추가한 타입이다.

<br>

마찬가지로 bigint도 타입을 추론하게 만드는게 좋다.

<br>

```tsx
let a = 1234n;
```

<br>

## String

<br>

마찬가지로 보통 타입스크립트가 추론하게 선언하게 만든다.

```tsx
let a = "hello";
```

<br>

## 객체

<br>

객체는 보통 어노테이션을 사용하여 타입을 정하지 않는다.

`let a: object = { b: 'x'}`

<br>

자바스크립트에서는 객체가 배열, 클래스, 함수 등등 객체로 부터 시작하기 때문에 직접 어노테이션을 하지 않는다.

```tsx
let a: { b: number } = {
  b: 12,
}; // {b: number}
```

<br>

interface 을 사용해서도 타입을 정해줄 수 있다.

```tsx
interface a {
  id: number;
  content: string;
  completed: boolean;
}

let a: a;

a = {
  id: 1,
  content: "Alex",
  completed: true,
};
```

<br>

### 인덱스 시그니처

[key: T] : U 같은 문법을 인덱스 시그니처 라고 부른다.

<br>

키 타입을 정할수있게 하고 그에따라 값도 따라서 정의를 하면

첫번째로 키의 타입을 정의할수있다

두번째로 값의 타입을 정의할수있다.

세번째로 객체에 다양한 프로퍼티를 정의할때도 타입이 그 타입만 가능할수있게 여러가지 프로퍼트를 정의해도된다.

<br>

readonly 는 한정자를 이용해서 읽기전용으로 정의 할 수 있다.

<br>

빈객체를 어노테이션한다면 `let danger: {}` 을 하게된다면 null 과 undefined를 제외한 모든 타입이 빈 객체 타입에 할당할 수 있게 되므로, 개발자가 예측하지 못한 타입이 들어갈수도 있으므로 최대한 빈객체는 피하는 것이 좋다.

<br>

### 타입 별칭

<br>

미리 타입을 변수에 할당하고 그에따라 재사용이 가능하게 타입을 선언해준다.

```tsx
type Age = number;
type Person = {
  name: string;
  age: Age;
};
```

<br>

따라서 Age 을 재사용하여 타입을 정의 할 수 있다.

<br>

### 유니온과 인터섹션 타입

<br>

A와 B에 있는 합집합을 합친 결과(유니온)를 하면 A와 B의 타입을 선택적으로 사용할수있게 된다.

```tsx
type Cat = { name: string; purrs: boolean };
type Dog = { name: string; barks: boolean; wags: boolean };
type CatOrDogBoth = Cat | Dog;
type CatAndDog = Cat & Dog;
```

<br>

합집합(유니온)을 한다면 `type CatAndDog = Cat & Dog` 두 타입중 하나는 반드시 쓰고 다른 하나의 타입은 추가적으로 사용할 수 있다.

```tsx
//Cat
let a: CatOrDogBoth = {
  name: "Alex",
  purrs: true,
};

// Dog만 가능
a = {
  name: "Alex",
  barks: true,
  wags: true,
};

a = {
  name: "Alex",
  barks: true,
  purrs: true,
  wags: true,
};
```

<br>

A와 B에는 교집합(인터섹션)을 하면 A와 B의 모든 타입을 사용가능하다.

```tsx
let b: CatAndDog = {
  name: "Alex",
  barks: true,
  purrs: true,
  wags: true,
};
```

<br>

## 배열

<br>

타입을 추론할수있게 배열의 요소들이 하나의 타입을 같게 해주어야한다.

```tsx
let c: string[] = ["a"];
let h: number[] = [];
```

<br>

c배열의 요소에는 항상 string타입들만 와야한다.

h배열의 요소는 number타입들만 와야한다.

<br>

빈배열을 추론하게끔 하면 배열의 요소들이 any타입으로 추론되어진다.

```tsx
let a = []; // any[]
```

<br>

**보통 배열은 모든 요소들이 같은 타입을 갖도록 설계해야한다.**

**그렇지 않으면 타입스크립트에 배열에 관한 작업이 안전한지 확인해야하므로 추가작업을 해야한다. 따라서 위의 c,h 같은 예제를 따라야한다.**

<br>

### 튜플

배열의 길이를 지정가능하고 인덱스의 타입이 알려진 배열이다.

정확한 배열의 요소들의 타입을 정할수있게된다.

```tsx
let a: [number] = [1];
let b: [string, string, number] = ["Alex", "James", 27];
```

<br>

😁최소 한개의 요소를 갖는 배열

```tsx
let frineds: [string, ...string[]] = ["javi", "Andrea", "Bowen"];
```

<br>

### 읽기 전용 배열과 튜플

<br>

타입스크립트는 readonly 배열 타입을 기본으로 지원하여 불변 배열을 만들 수 있다.

읽기전용은 일반배열과 같지만 내용을 갱신 할 수 없다는 점만 다르다.

<br>

**😄어떻게 읽기전용을 만들 수 있을까?**

명시적 타입 어노테이션으로 만들 수 있다.

<br>

불변으로 만들기때문에 push, splice 등등과 같은 내용을 바꾸는 메서드 대신에 slice, concat 등등 과 같은 원본을 바꾸지 않는 메서드를 사용해야한다.

```tsx
let as: readonly number[] = [1, 2, 3];
```

<br>

### null, undefined, viod, nerver

<br>

타입스크립트와 자바스크립트느 null, undefined는 두가지 값으로 값이 없음을 표현한다.

<br>

undefined는 값이 아직 정의 되어지지 않았다는것을 의미하고

null은 값이 없음을 명시적으로 의미한다.

<br>

더 세밀하게 분류하는 특별한 용도의 void와 never가 있다.

void는 명시적으로 아무것도 반환하지 않은 함수의 반환타입을 의미한다.

never는 절대 반환하지 않는(예외 던지기, 계속 끊임없이 실행되는 while문) 함수 타입을 가리킨다.

```tsx
// number 또는 null을 반환하는 함수
function a(x: number) {
  if (x === 1) {
    return x;
  }
  return null;
}

//undefined를 반환하는 함수
function b() {
  return undefined;
}

// void를 반환하는 함수
function c() {
  let a = 10 + 10;
}

// never를 반환하는 함수
function e() {
  while (true) {
    dosomething();
  }
}
```

<br>

참고

- 이 글은 [타입스크립트 프로그래밍 책](http://www.yes24.com/Product/Goods/90265564?OzSrank=3)을 참고하여 정리한 글 입니다.

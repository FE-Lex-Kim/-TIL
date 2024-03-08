- [TypeScript(7) - 타입 확장, 타입 좁히기](#typescript7---타입-확장-타입-좁히기)
  - [타입 확장](#타입-확장)
    - [1. 유니온 타입](#1-유니온-타입)
    - [2. 교차타입](#2-교차타입)
    - [3. extends와 교차타입](#3-extends와-교차타입)
    - [4. 확장 적용하기](#4-확장-적용하기)
  - [타입 좁히기](#타입-좁히기)
    - [1. 원시타입 좁히기(typeof 연산자)](#1-원시타입-좁히기typeof-연산자)
    - [2. 인스턴스 객체 좁히기(instanceof 연산자)](#2-인스턴스-객체-좁히기instanceof-연산자)
    - [3. 객체의 속성이 있는지 없는지(in 연산자)](#3-객체의-속성이-있는지-없는지in-연산자)
    - [4. 배열 좁히기(Array.isArray)](#4-배열-좁히기arrayisarray)
    - [5. 사용자 정의 타입 가드 함수(is 연산자)](#5-사용자-정의-타입-가드-함수is-연산자)
    - [6. 브랜드 속성 사용](#6-브랜드-속성-사용)
    - [7. Exhaustive Checking](#7-exhaustive-checking)

# TypeScript(7) - 타입 확장, 타입 좁히기

<br>

## 타입 확장

타입 확장은 기존타입을 사용해서 새로운 타입을 정의하는 것을 말한다.

타입스크립트는 interface, type 키워드로 타입을 정의하고

extends, 교차타입(&), 유니 온 타입( | )을 사용해서 타입을 확장한다.

<br>

### 1. 유니온 타입

유니온 타입은 2개 이상의 타입을 조합하여 사용하는 방법이다.

집합으로보면 합집합이다.

```tsx
type Union = A | B;
```

A 와 B 타입의 모든 값이 Union 타입의 값이 된다.

주의해야할 점은 유니온 타입으로 선언된 값은 유니온 타입에 포함된 모든 타입이 **공통으로 갖고 있는 속성에만 접근할** 수 있다.

```tsx
interface Alex {
  name: "alex";
  handsome: true;
}
interface James {
  name: "james";
}

function IsHandsome(person: Alex | James) {
  return person.handsome;
}
```

IsHandsome은 Alex와 James의 유니온 타입 값을 person이라는 인자로 받고 있다.

person.handsome을 호출하고 있는데 Alex에만 존재하는 속성이다.

James에는 없는 속성이므로 에러가 발생한다.

따라서 person의 유니온 타입은 Alex 또는 James 타입에 해당할 뿐이지 Alex 이면서 James 인것은 아니다.

<br>

### 2. 교차타입

교차 타입도 기존의 타입을 합쳐서 하나의 타입을 만드는 것이다.

```tsx
interface Alex {
  name: string;
  handsome: true;
}
interface James {
  name: string;
}

type AlexAndJames = Alex & James;
function getNameHandsome(person: AlexAndJames) {
  console.log(person.name);
  console.log(person.handsome);
}
```

AlexAndJames 타입은 Alex와 James 타입을 합쳐 모든 속성을 가진 타입이 된다.

<br>

```tsx
interface Mango {
  yellow: true;
}
interface Apple {
  red: true;
}

type MangoApple = Mango & Apple; // Mago & Apple 이라고 타입이 뜬다.
function IsYellowRed(fruit: MangoApple) {
  console.log(fruit.yellow);
  console.log(fruit.red);
}
```

MagoApple 타입은 Mago와 Apple의 교집합이라서 우리가 예상한 공집합인 never이 아니다.

Mango와 Apple의 속성을 모두 포함한 타입이 된다.

왜냐하면 Mago와 Apple은 객체형식의 타입이므로 MangoApple의 타입은 Mango와 Apple의 객체형식의 타입안에서 교집합을 실행하는 것이 아닌, Mango & Apple 그자체로 뜨는 타입이 된다.

따라서 Mango & Apple을 모두 만족하는 값의 타입인 `{ yellow : true, red: true}` 가 된다.

<br>

여기서 주의해야할점은 아래와 같은 코드이다.

```tsx
type type1 = number | string;
type type2 = string | boolean;

type type3 = type1 & type2; // string
```

위의 MagoApple과 다르게 type1, type2 는 객체 형식의 타입이 아니라 그자체로 타입이다.

따라서 typ1과 type2 의 교집합을 직접 실행하므로 type3는 type1과 type2를 모두 만족하는 타입인 string 이 된다.

```tsx
type type1 = number;
type type2 = string;

type type3 = type1 & type2; // never
```

위의 코드는 number과 string 모두 만족하는 타입이 없어 공집합인 never이 된다.

<br>

### 3. extends와 교차타입

extends 키워드를 사용해서 교차타입을 작성할 수도 있다.

```tsx
interface Mango {
  name: string;
  yellow: true;
}

interface MangoApple extends Mango {
  red: true;
}
```

MagoApple 타입은 Mago을 확장함으로써 Mango의 속성을 모두 포함하고 있다.

<br>

이를 extends가 아닌 교차타입( & )으로 작성하면 다음과 같다.

```tsx
interface Mango {
  name: string;
  yellow: true;
}

type MangoApple = {
  red: true;
} & Mango;
```

interface는 교차타입( & )을 사용할 수 없고 extends로 확장한다.

type은 extends를 사용할 수 없고 교차타입( & ) 으로 확장한다.

<br>

두개 모두 확장한다는 개념은 같지만 100프로 같지는 않다.

먼저 interface extends의 경우를 보자

```tsx
interface Mango {
  red: false;
}

interface MangoApple extends Mango {
  red: true;
} // 에러 발생
```

MangoApple은 Mango와 교집합이 되는 속성이 없어서 호환되지 않다는 에러가 발생한다.

<br>

다음은 type의 교차타입의 경우를 보자

```tsx
type Mango = {
  red: false;
};

type MangoApple = Mango & {
  red: true;
};
```

interface의 extends는 에러가 발생하지만 type의 교차타입은 에러가 발생하지 않는다.

이때 MangoApple의 속성 타입은 never이 된다.

왜냐하면 새롭게 추가되는 `{ red : true }` 속성은 미리 알 수 없기 때문에 선언 시 에러가 발생하지 않는다. 하지만 red 라는 같은 속성에 대해 서로 호환되지 않아 결국 never 타입이 된다.

<br>

### 4. 확장 적용하기

기존에 있던 서버 응답 값의 타입이 UserInfo라고 해보자.

```tsx
interface UserInfos {
  name: string;
  email: string;
}
```

<br>

이때 서버에서 응답값에 추가적으로 VIP, VVIP라는 정보가 추가적으로 들어갔다고 해보자.

추가적으로 정보가 들어왔으므로 기존 타입을 수정해야한다.

타입 수정 방법으로 2가지를 생각해 볼 수 있다.

<br>

**첫번째 방법 : 하나의 타입에 여러 속성을 추가할 때**

```tsx
interface UserInfos {
  name: string;
  email: string;
  isVIP?: boolean; // 추가
  isVVIP?: boolean; // 추가
}
```

<br>

아래는 각각 서버에서 새롭게 추가된 정보와 함께 받아온 응답값을 새롭게 수정한 타입으로 변경한 방법이다.

```tsx
  // 각 배열은 서버에서 받아온 응답값이라고 가정
  const userInfos: UserInfos[] = [
    {
      name: "Alice",
      email: "alice@example.com",
    },
    {
      name: "Bob",
      email: "bob@example.com",
    },
  ];

  const VIPUserInfos: UserInfos[] = [
    {
      name: "Eve",
      email: "eve@example.com",
      isVIP: true,
    },
    {
      name: "Frank",
      email: "frank@example.com",
      isVIP: true,
    },
  ];

  const VVIPUserInfos: UserInfos[] = [
    {
      name: "Harry",
      email: "harry@example.com",
      isVVIP: true,
    },
    {
      name: "Ivy",
      email: "ivy@example.com",
      isVVIP: true,
    },
    {
      name: "Jack",
      email: "jack@example.com",
      isVVIP: true,
    },
  ];

userInfos: UserInfos[]; // ok
VIPUserInfos: UserInfos[]; // ok
VVIPUserInfos: UserInfos[]; // ok
```

모든 응답값에 `UserInfos[]`라는 타입을 지정했다.

<br>

하지만 다음과 같은 문제가 발생할 수 있다.

```tsx
VIPUserInfos.map((info) => info.isVVIP); // 실행단계에서 에러
```

VIPUserInfos는 UserInfos의 타입 원소를 갖기 때문에 isVVIP에 접근할 수 있다.

따라서 코드 작성단계에서 에러가 발생하지 않는다.

하지만 실행단계에서 isVVIP 속성을 가지고 있지 않으므로 에러가 발생한다.

<br>

**두번째 방법 : 타입을 확장하는 방식**

각 배열의 타입에 맞게 명확하게 규정할 수 있다.

```tsx
  interface UserInfos {
    name: string;
    email: string;
  }

  interface VIPUserInfo extends UserInfos {
    VIP: boolean;
  }

  interface VVIPUserInfo extends UserInfos {
    VVIP: boolean;
  }

userInfos: UserInfos[] // OK

VIPUserInfos: UserInfos[] // NOT OK
VIPUserInfos: VIPUserInfos[] // OK

VVIPUserInfos: UserInfos[] // NOT OK
VVIPUserInfos: VVIPUserInfos[] // OK
```

<br>

이를 바탕으로 VIPUserInfos 배열의 원소 내 속성에 접근하면, 프로그램을 실행하지 않고도 코드 작성단계에서 타입이 잘못됬음을 미리 알 수 있다.

```tsx
VIPUserInfos.map((info) => info.isVVIP); // 타입 에러
```

**결과적으로 주어진 타입에 무분별하게 속성을 추가하여 사용하는것 보다 타입을 확장해서 사용하는 것이 좋다.**

적절한 네이밍을 사용해서 타입의 의도를 명확하게 표현할 수도 있고

코드작성단계에서 미리 버그도 예방할 수 있다.

<br>

## 타입 좁히기

타입 좁히기는 타입 범위를 더 작은 범위로 좁혀나가는 과정을 말한다.

더 정확하고 명시적인 타입 추론을 할 수 있고 복잡한 타입을 작은 범위로 축소하여 타입 안정성을 높일 수 있다.

<br>

### 1. 원시타입 좁히기(typeof 연산자)

typeof 연산자를 사용하면 원시 타입에 대해 추론할 수 있다.

**해당 좁히기(분기) 내에서는 typeof A === B 를 할시 A의 타입이 B로 추론된다.**

하지만 typeof 연산자는 null과 배열 타입 등이 object 타입으로 판별되는 복잡한 타입을 검증하기에는 한계가 있다.

**따라서 typeof 연산자는 주로 원시 타입을 좁히는 용도로만 사용하는 것이 좋다.**

- string
- number
- boolean
- undefined
- object
- function
- bigint
- symbol

```tsx
function getName(params: string) {
  if (typeof params === "string") {
    return "alex";
  }

  return "hi";
}
```

<br>

### 2. 인스턴스 객체 좁히기(instanceof 연산자)

instanceof 연산자는 인스턴스화된 객체 타입을 판별하는 타입 가드로 사용할 수 있다.

객체 instanceof 생성자 함수

<br>

우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 true로 평가되고, 그렇지 않은 경우에는 false로 평가된다.

```tsx
class A {}
class B {}

function processValue(value: A | B) {
  if (value instanceof A) {
    value;
  } else {
    value;
  }
}
```

<br>

### 3. 객체의 속성이 있는지 없는지(in 연산자)

in 연산자는 객체에 속성이 있는지 확인한후 true 또는 false를 반환한다.

```tsx
const Alex = (props: { name: string; age?: number }) => {
  if ("age" in props) return <h1>alex is props.age</h1>;
  return <h1>Hi props.name</h1>;
};
```

자바스크립트의 in 연산자는 런타임의 값만 검사하지만 타입스크립트에서는 객체 타입에 속성이 존재하는지를 검사한다.

<br>

### 4. 배열 좁히기(Array.isArray)

Array.isArray로 타입을 좁힐 수 있다.

```tsx
function processValue(value: string | number[]) {
  if (Array.isArray(value)) {
    value; //(parameter) value: number[]
  } else {
    value; //(parameter) value: string
  }
}
```

<br>

### 5. 사용자 정의 타입 가드 함수(is 연산자)

지금까지는 기존 자바스크립트 사용하여 좁혀짐을 처리했지만, 때로는 코드 전체에서 유형이 변경되는 방식을 보다 직접적으로 제어하고 싶을 때가 있다.

타입 프레디케이트(Type Predicates)는 타입스크립트에서 특정 값이 특정 타입인지를 판별하는 함수이다.

이 함수는 **`value is Type`** 형태로 정의되며, 일반적으로 타입 가드(Type Guards)와 함께 사용된다.

<br>

사용자가 타입 가드를 정의하려면, 반환 타입이 type predicate인 함수를 정의하기만 하면 된다.

```tsx
function isString(value: any): value is string {
  return typeof value === "string";
}
```

위 코드에서 **`isString`** 함수는 주어진 값이 **`string`** 타입인지를 판별하는 타입 프레디케이트이다.

이를 사용하는 함수에서 **`isString`**이 **`true`**를 반환하는 경우 **TypeScript는 해당 값이 `string` 타입임을 추론하게 된다.**

<br>

```tsx
function isString(value: string | number): value is string {
  return typeof value === "string";
}

function stringOrNumber(value: string | number) {
  if (isString(value)) {
    // 가독성이 좋다.
    console.log("값은 string 입니다.");
    value;
  } else {
    console.log("값은 number 입니다.");
    value;
  }
}
```

위 코드에서 `isString(value)`가 만약 `true`를 반환한다면, if문의 블록안에서는 `value` 타입이 `string`으로 추론하게 된다.

따라서 자연스럽게 `else` 블록에서는 `string`이 아닌 `number`로 추론하게 된다.

<br>

**이렇게 타입 프레디케이트를 사용하면 가독성이 좋아진다.**

만약 타입 좁히기를 직접 처리하게 되면 가독성이 안좋아 질 수 있다.

```tsx
const isRequiredAndNumberAndNotZero = (value: any): value is number => {
  return isNumber(value) && isNotZero(value) && isRequired(value);
}

const isRequired = (value: any): value is any => {
  return value !== null && value !== undefined;
}

const isNumber = (value: any): value is number => {
  return typeof value === "number";
}

const isNotZero = (value: number | string): value is number => {
  return Number(value) !== 0;
}

type StringOrNumber = string | number;

const data: StringOrNumber = await fetchData();

//type predicate 사용한 type guard. 아 data는 저런 데이터겠군 하고 바로 유추가능하다.
if(isRequiredAndNumberAndNotZero(data){
   console.log(data + 10);
} else {
   console.log(data + "십");
}

//type predicate를 사용하지 않은 type guard. 가독성이 좋지 않다.
if(data !== null || data !== undefined){
  if(typeof data === "number"){
    if(data !== 0){
      console.log(data + 10);
    }
  }
} else {
  console.log(data + "십");
}
```

<br>

하지만 is 연산자를 사용할때는 타입을 잘못 적을 가능성이 생긴다.

아래코드는 is 연산자를 잘못 사용해서 타입 좁히기가 반대로 되어버렸다.

```tsx
function isString(value: any): value is number {
  return typeof value === "string";
}

function stringOrNumber(value: string | number) {
  if (isString(value)) {
    console.log("값은 string 입니다.");
    value; // (parameter) value: number
  } else {
    console.log("값은 number 입니다.");
    value; // (parameter) value: string
  }
}
```

<br>

### 6. 브랜드 속성 사용

브랜드 속성을 사용하면 더 쉽게 구분할 수 있다.

```tsx
interface X {
  __type: "X";
  width: number;
  height: number;
}
interface Y {
  __type: "Y";
  length: number;
  center: number;
}

function processValue(value: X | Y) {
  if (value.__type === "X") {
    value;
  } else {
    value;
  }
}
```

<br>

응용하기 예시

```tsx
// 객체 타입 속성
type Object1 = {
  name: string;
  age: number;
  hobby: string;
};

type Object2 = {
  name: string;
  age: number;
  skills: string[];
};

type ObjectArrType = Object1 | Object2;

// 객체요소 배열
const arrayOfObjects: ObjectArrType[] = [
  {
    name: "Charlie",
    age: 19,
    skills: ["JavaScript", "TypeScript"],
  },
  {
    name: "Charlie",
    age: 19,
    hobby: "Reading",
  },
];
```

<br>

여기서 새로운 객체가 arrayofobjects에 생겼다고 해보자.

이 객체는 Object1, Object2의 모든 속성을 다 가지고 있다.

```tsx
const arrayOfObjects: ObjectArrType[] = [
  //...
  {
    name: "Charlie",
    age: 20,
    hobby: "Reading",
    skills: ["JavaScript", "TypeScript"],
  },
];
```

이때는 Object1 과 Object2 타입과 다르므로 에러가 발생해야한다.

하지만 이코드를 작성했을때는 자바스크립트는 에러를 뱉지 않는다.

타입 에러가 발생하지 않는다면 무수한 에러 객체가 생길 가능성이 있다.

<br>

따라서 객체 타입을 서로 호환되지 않도록 만들어주어야한다.

브랜드 속성을 사용하면 된다.

```tsx
// 객체 타입 속성
type Object1 = {
  __type: "object1";
  name: string;
  age: number;
  hobby: string;
};

type Object2 = {
  __type: "object2";
  name: string;
  age: number;
  skills: string[];
};

type ObjectArrType = Object1 | Object2;

// 객체요소 배열
const arrayOfObjects: ObjectArrType[] = [
  {
    __type: "object1",
    name: "Charlie",
    age: 19,
    skills: ["JavaScript", "TypeScript"],
  },
  {
    __type: "object2",
    name: "Charlie",
    age: 19,
    hobby: "Reading",
  },
  {
    __type: "object1",
    name: "Charlie",
    age: 20,
    hobby: "Reading",
    skills: ["JavaScript", "TypeScript"],
  },
];
```

이제는 브랜드 속성을 사용해서 에러 객체에 대해 에러가 발생하는 것을 확인할 수 있다.

<br>

### 7. Exhaustive Checking

Exhaustive 사전적으로 철저함, 완벽함을 의미한다.

따라서 Exhaustive Checking는 철저하게 모든 타입을 검사하는 것을 말하는 패러다임이다.

<br>

분기처리(타입 조건문)를 필요하다고 생각되는 부분만 분기처리를 해서 코드를 작성할 수 있다.

하지만 모든 케이스에 대해 분기처리를 해야만 유지보수 측면에서 안전하다고 생각되는 상황이 생긴다.

이때 Exhaustive Checking을 통해 모든 케이스에 대한 타입 검사를 강제할 수 있다.

<br>

아래 예시를 보자

```tsx
interface X {
  __type: "X";
  width: number;
  height: number;
}
interface Y {
  __type: "Y";
  length: number;
  center: number;
}

function processValue(value: X | Y) {
  if (value.__type === "X") return value + "X";
  if (value.__type === "Y") return value + "Y";
  return value;
}
```

X, Y 타입에 대해 분기를 걸어서 확실하게 타입 검사를 하고 있다.

만약 새로운 Z 타입이 생겼다고 해보자

```tsx
...
interface Z {
  __type: "Z";
  length: number;
  center: number;
}

function processValue(value: X | Y) {
  if (value.__type === "X") return value + "X";
  if (value.__type === "Y") return value + "Y";
  else {
	  exhaustiveCheck(value); // Error : Argument of type 'Y' is not assign
														// able to parameter of type 'never'
	  return value;
	}
}
```

이때 당연히 Z 타입에 대해 분기를 걸어주어야 한다. 하지만 깜빡하고 모든 케이스에 대한 분기를 처리해주지 않았을때 컴파일 타입 에러가 발생하게 해주는것이 Exhaustive Checking 이다.

`exhaustiveCheck(value)` 은 파라미터로 어떠한 값을 받을수없는 never 타입을 선언하고 있다.

따라서 만약 모든 분기를 지나치고 else 문에 들어오면 강제로 타입 에러가 발생하게 된다.

이렇게 철저하게 분기 처리가 필요하다면 Exhaustive Checking 패턴을 활용하는게 좋다.

<br>

참고

- 타입스크립트 교과서 책을 정리한 글입니다.
- 우아한 타입스크립트 책을 정리한 글입니다.

- [TypeScript(4) - 맵드 타입(Mapped Types), 템플릿 리터럴 타입(Template Literal Types),then 반환값 타입,객체간 대입](#typescript4---맵드-타입mapped-types-템플릿-리터럴-타입template-literal-typesthen-반환값-타입객체간-대입)
  - [맵드 타입(Mapped Types)](#맵드-타입mapped-types)
  - [템플릿 리터럴 타입(Template Literal Types)](#템플릿-리터럴-타입template-literal-types)
  - [then 반환값 타입](#then-반환값-타입)
  - [객체간 대입](#객체간-대입)

# TypeScript(4) - 맵드 타입(Mapped Types), 템플릿 리터럴 타입(Template Literal Types),then 반환값 타입,객체간 대입

<br>

## 맵드 타입(Mapped Types)

TypeScript의 매핑된 타입은 기존 타입의 속성을 변환하여 새로운 타입을 생성할 수 있는 강력한 기능이다.

이를 통해 객체 타입의 키를 반복하고 해당 키를 기반으로 새 타입을 생성할 수 있다.

이는 모든 properties을 optional, readonly 또는 nullable로 만드는 등 기존 타입의 변형을 만드는 데 특히 유용할 수 있다.

다음은 TypeScript에서 매핑된 타입을 사용하는 방법에 대한 예시이다.

```tsx
type Person = {
  name: string;
  age: number;
  email: string;
};

type PartialPerson = { [K in keyof Person]?: Person[K] };

const partialPerson: PartialPerson = {
  name: "John",
};
```

이 예에서는  `name` ,  `age` ,  `email` 이라는 세 가지 속성을 사용하여  `Person`  타입 정의한다.

그런 다음에 맵된핑 유형을 사용하여 'PartialPerson'이라는 새 타입 만든다.

매핑된 타입은  `Person`  타입의 각 키( `K` )를 반복하고 속성 타입 뒤에  `?` 를 추가하여 각 속성을 선택 사항으로 만든다.

마지막으로 '`name`' 속성에만 값이 할당되는 'PartialPerson' 타입의 'partialPerson' 변수 만든다.

매핑된 타입 표현식  `{ [K in keyof Person]?: Person[K] }` 을 분석한다.

- `[K in keyof Person]` : 이 부분은  `Person`  타입의 모든 속성 이름의 통합을 생성하는  `keyof`  연산자를 사용하여  `Person`  타입의 각 키( `K` )를 반복한다.
- `?: Person[K]` : 매핑된 타입의 각 속성의 타입을 지정한다.  `Person[K]` 를 사용하여  `Person`  타입에서 해당 속성의 타입을 검색한다.

따라서 매핑된 타입  `{ [K in keyof Person]?: Person[K] }` 는 본질적으로 원래  `Person`  타입의 각 속성이 새로운 타입을 생성한다.

<br>

맵드 타입에서는 수식을 더해주는 것 뿐만아니라 제거할 수도 있다.

기존에 타입에 존재하던 `readonly`, `?` 앞에 `-` 을 붙여주면 해당 수식어를 제거한 타입 선언을 할 수 있다.

```tsx
type Person = {
  readonly name: string;
  age?: number;
};

type MutablePerson = {
  -readonly [K in keyof Person]-?: Person[K];
};

type ResultType = MutablePerson; // { name : string, age: number}
```

다음과 같이 `readonly`와 `?` 가 사라졌다.

<br>

맵드 타입을 사용하는 예시.

```tsx
const personInfo = {
  name: "John",
  age: 30,
  isActive: true,
  hobbies: ["Reading", "Traveling"],
  address: { city: "New York", postalCode: "10001" },
  greet: (name) => console.log(`Hello, ${name}!`),
  createdAt: new Date(),
  metadata: { key: "value" },
  description: "Optional description",
  nullableValue: null,
};

// 불필요한 반복이 발생한다.
type PersonInfoStore = {
  name: {
    a: () => void;
    b: any;
    c: boolean;
  };
  age: {
    a: () => void;
    b: any;
    c: boolean;
  };
  isActive: {
    a: () => void;
    b: any;
    c: boolean;
  };
  // ...
};

// 맵드 타입으로 효율적으로 타입을 선언할 수 있다.
type PersonInfoID = keyof typeof personInfo;
type PersonInfoStore = {
  [K in PersonInfoID]: {
    a: () => void;
    b: any;
    c: boolean;
  };
};
```

<br>

as 키워드를 사용해서 키를 재지정 할 수 있다.

위의 예시에서 PersonInfoStore의 키 이름에 PersonInfo의 키 이름을 그대로 쓰고 싶은 경우가 있을 수 도 있고, 모든 키에 ForModal 이라는 공통된 처리를 적용해서 새로운 키를 지정하고 싶을 수 도 있다. 이때 as 키워드를 사용하면 된다.

```tsx
// 맵드 타입으로 효율적으로 타입을 선언할 수 있다.
type PersonInfoStore = {
  [K in PersonInfoID as `${K}ForModal`]: {
    a: () => void;
    b: any;
    c: boolean;
  };
};

// nameForModal: {
//   a: () => void;
//   b: any;
//   c: boolean;
// };
// ageForModal: {
//     a: () => void;
//     b: any;
//     c: boolean;
// };
```

<br>

## 템플릿 리터럴 타입(Template Literal Types)

템플릿 리터럴 타입은 TypeScript 4.1부터 도입된 새로운 기능이다. 이를 사용하면 문자열 템플릿 리터럴을 기반으로 타입을 정의할 수 있습니다. 다음은 간단한 예제입니다:

```tsx
type Color = "red" | "blue" | "green";
type Quantity = 1 | 2 | 3 | 4;

type Card = `${Color}-${Quantity}`;

const card1: Card = "red-2"; // Valid
const card2: Card = "blue-4"; // Valid
// const card3: Card = 'green'; // Error: Type '"green"' is not assignable to type 'Card'.
// const card4: Card = 'yellow-3'; // Error: Type '"yellow-3"' is not assignable to type 'Card'.
```

- `Color` 는 문자열 리터럴 유니온 타입으로 'red', 'blue', 'green' 중 하나를 가질 수 있다.
- `Quantity` 는 숫자 리터럴 유니온 타입으로 1, 2, 3, 4 중 하나를 가질 수 있다.
- `Card` 는 템플릿 리터럴 타입을 사용하여  `${Color}-${Quantity}`  형식의 문자열을 나타내는 타입이다.즉, Color와 Quantity가 결합된 문자열 형태를 가진다.
- `card1` 과  `card2` 는  `Card`  타입에 해당하는 유효한 값입니다.
- `card3` 과  `card4` 는  `Card`  타입에 맞지 않는 값이므로 컴파일 시에 에러가 발생한다.

이렇게 새로운 문자열 리터럴 유니온 타입을 만들 수 있다.

<br>

템플릿 리터럴 타입은 문자열 리터럴을 조합하여 새로운 문자열 타입을 생성하는 방법을 제공한다. 이는 문자열 리터럴과 유사한 문법을 사용하여 타입을 정의할 수 있는 방법이다.

```tsx
type Greeting = "Hello, " | "Hola, " | "Bonjour, ";
type Language = "English" | "Spanish" | "French";

type GreetingMessage<T extends Language> = `${Greeting}Welcome to TypeScript in ${T}`;

// 예시 사용
const englishMessage: GreetingMessage<"English"> = "Hello, Welcome to TypeScript in English";
const spanishMessage: GreetingMessage<"Spanish"> = "Hola, Welcome to TypeScript in Spanish";
const frenchMessage: GreetingMessage<"French"> = "Bonjour, Welcome to TypeScript in French";
```

<br>

다음은 에러가 발생한 코드이다.

```tsx
type Template = `template ${string}`;
let str: Template = `template `;
str = "template hello";
str = "template 123";
str = "template"; // 에러
```

마지막 str에만 에러가 발생한다. template 문자열 뒤에 띄어쓰기가 없기 때문이다. 문자열 변수를 엄격하게 관리할 수 있다.

<br>

문자열 조합해서 표현할때 편리하다.

예를 들어 City로 서울, 수원, 부산, Vehicle로 차, 자전거, 도보 가 있는 상황이다.

이들을 조합해서 지역:이동수단으로 타이핑을 하고 싶다

조합은 9가지 가 나오게 된다. 서울 : 차, 서울 : 자전거, 서울 : 도보, 수원 : 차, 수원 : 자전거 …… 부산 : 도보

이럴때 템플릿 리터럴로 편리하게 사용할 수 있다.

```tsx
type City = "seoul" | "suwon" | "busan";
type Vehicle = "car" | "bike" | "walk";
type ID = `${City} : ${Vehicle}`;
const id: ID = "seoul : walk";
```

<br>

## then 반환값 타입

제네릭을 사용하면 해당 **`then`** 메서드에서 반환되는 값의 타입을 명시적으로 지정할 수 있다. 이는 TypeScript에서 타입 안정성을 보장하는데 도움이 된다.

```tsx
fetchData()
  .then<number>((data) => {
    // 데이터 처리 로직
    return data * 2; // 반환값의 타입을 명시적으로 number로 설정
  })
  .then<string>((result) => {
    // 결과 처리 로직
    return result.toString(); // 반환값의 타입을 명시적으로 string으로 설정
  })
  .then((finalResult) => {
    // 최종 결과 처리 로직
    // 여기서 반환값의 타입은 이전 `then` 메서드에서 설정된 타입으로 추론됨
  });
```

위 예제에서 각 **`then`** 메서드 앞에 제네릭을 사용하여 해당 메서드에서 반환되는 값의 타입을 명시적으로 지정했다. 이렇게 함으로써 TypeScript는 각 단계에서 예상되는 타입을 추론하고, 코드의 타입 안정성을 유지할 수 있다.

<br>

## 객체간 대입

```tsx
interface Person {
  name: string;
  age: number;
}

interface Employee {
  name: string;
  age: number;
  position: string;
}

let person: Person = { name: "John", age: 30 };
let employee: Employee = { name: "Alice", age: 25, position: "Manager" };

person = employee; // 가능
employee = person; // 불가능
```

이는 **`Employee`** 인터페이스가 **`position`** 속성을 요구하기 때문에 **`Person`** 객체에는 해당 속성이 없어서 호환되지 않기 때문이다.

<br>

Person은 Employee 보다 넓은 타입이다.

**좁은 타입은 넓은 타입에 대입할 수 있고 넓은 타입은 좁은 타입에 대입할 수 없다.**

Employee가 Person보다 넓다고 착각할 수 도 있다.

하지만 Employee가 코드의 양이 많고 A보다 차지하는 줄의 폭이 넓기 때문이다.

**즉, 코드의 양과 줄 수가 더 많은 이유는 그만큼 더 구체적으로 적었기 때문에 더 좁은 타입이라고 생각하면 된다.**

<br>

교집합인 &, 합집합인 | 이 있다면, | 가 더 넓은 영역에 속한다.

```tsx
interface A {
    name: string;
    city: string;
  }

  interface B {
    age: number;
    city: string;
  }

  let c;

  const target1: A & B = c : A | B; // 에러
  const target2: A = c : A | B; // 에러
  const target3: B = c : A | B; // 에러
```

A | B가 A & B 보다 더 큰 영영이다.( A & B )가 더 구체적이므로 더 좁은 영역이라고 생각하면된다.

A | B가 A 보다 더 큰영역이다.(A | B ) 는 A를 포함하고 또 B까지도 포함하기 때문에 더 크다.

A | B도 B 보다 더 큰영역이다.

<br>

튜플은 배열보다 좁은 타입이다. 튜플은 배열에 대입이 가능하지만, 배열은 튜플에 대입할 수 없다.

```tsx
// 튜플 타입 정의
type TupleType = [string, number];

// 배열 타입 정의
type ArrayType = string[];

// 튜플 변수
let tuple: TupleType = ["apple", 3];

// 배열 변수
let array: ArrayType = ["banana", "orange", "grape"];

// 튜플에 배열 대입 시도
tuple = array; // 실패: 배열 타입은 튜플 타입보다 넓은 범위를 가지므로 대입할 수 없습니다.

// 배열에 튜플 대입 시도
array = tuple; // 성공: 튜플 타입은 배열 타입보다 좁은 범위를 가지므로 대입할 수 있습니다.
```

<br>

배열이나 튜플에 readonly 수식어를 붙일 수 있다. readonly 수식어가 붙은 배열이 더 넓은 타입이다.

<br>

참고

- 우아한 타입스크립트 with 리액트를 참고한 글입니다.

- [TypeScript(4) - 맵드 타입(Mapped Types), 템플릿 리터럴 타입(Template Literal Types)](#typescript4---맵드-타입mapped-types-템플릿-리터럴-타입template-literal-types)
  - [맵드 타입(Mapped Types)](#맵드-타입mapped-types)
  - [템플릿 리터럴 타입(Template Literal Types)](#템플릿-리터럴-타입template-literal-types)

# TypeScript(4) - 맵드 타입(Mapped Types), 템플릿 리터럴 타입(Template Literal Types)

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

참고

- 우아한 타입스크립트 with 리액트를 참고한 글입니다.

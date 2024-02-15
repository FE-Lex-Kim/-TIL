- [TypeScript(2) - 타입 앨리어스, 인터페이스](#typescript2---타입-앨리어스-인터페이스)
  - [타입 앨리어스](#타입-앨리어스)
  - [인터페이스](#인터페이스)
  - [인덱스 접근 타입](#인덱스-접근-타입)

# TypeScript(2) - 타입 앨리어스, 인터페이스

## 타입 앨리어스

지금까지는 변수나 인수의 정의 시에 직접 인라인으로 표기하는 방법이였다.

같은 타입을 여러 번 사용할 때 재사용하기 좋지 안흥ㄴ, 코드 기술이 복잡해지는 것이 문제이다.

<br>

타입 앨러이스(type alias)는 타입 지정의 별명을 붙이는 기능이다.

그 이름을 참조해서 같은 타입을 여러 차례 재사용할 수 있다.

**타입명은 일반적으로 대문자로 시작한다.**

```tsx
type Name = string;
```

<br>

다음과 같이 객체 타입도 타입 앨리어스를 붙여 사용할 수 있다.

```tsx
type Inform = {
  name: string;
  age: number;
};

function getPerson({ name, age }: Inform) {
  console.log(`my name is ${name}, I'm ${age}`);
}

getPerson({ name: "alex", age: 29 });
```

<br>

함수 자체의 타입도 타입 앨리어스로 정의할 수 있다.

**타입 앨리어스를 사용함으로써 매개변수 라인에 가독성이 좋아진다.**

```tsx
type Formatter = (firstName: string) => string;

function printName(firstName: string, formatter: Formatter) {
  console.log(formatter(firstName));
}
```

<br>

객체의 키 이름을 명시하지 않고 타입 앨리어스를 정의할 수 있다.

```tsx
type Label = {
  [key: string]: string;
};

const labels: Label = {
  topTitle: "톱 페이지 입니다",
  topTitle2: "톱 페이지 입니다2",
  topTitle3: "톱 페이지 입니다3",
};

// 값 부분의 타입이 맞지않아 에러
const foo: Label = {
  message: 200,
};
```

<br>

## 인터페이스

인터페이스는 타입 앨리어스와 비슷한 기능이지만, 보다 확장성이 높다.

보통 클래스와 함께 많이 사용한다.

```tsx
interface Inform {
  name: string;
  age: number;
}

function getPerson({ name, age }: Inform) {
  console.log(`my name is ${name}, I'm ${age}`);
}

getPerson({ name: "alex", age: 29 });
```

<br>

나중에 city를 추가하는 것처럼 인터페이스를 확장할 수 있다.

타입 앨리어스는 같은 이름으로 타입을 재정의할 수 없다.

```tsx
interface Inform {
  name: string;
  age: number;
}

function getPerson({ name, age, city }: Inform) {
  if (!city) city = "Korea";

  console.log(`my name is ${name}, I'm ${age}, I live in ${city}`);
}
interface Inform {
  city?: string;
}

getPerson({ name: "alex", age: 29, city: "Bucheon" });
```

<br>

또한 인터페이스는 extends를 사용해 다른 인터페이스를 확장할 수 있다.

```tsx
interface Colorful {
  color: string;
}

interface Circle {
  radius: number;
}

interface ColorfulCircle extends Colorful, Circle {}

const cc: ColorfulCircle = {
  color: "빨강",
  radius: 10,
};
```

<br>

객체 타입을 정의할 때 인터페이스와 타입 앨리어스는 모두 사용할 수 있다.

상속에 관한 세세한 기능의 큰차이는 없이 거의 비슷한 기능을 갖는다.

<br>

단 타입스크립트 설계 사상을 고려했을때 2가지 기능은 다른점이 있다.

1. 인터페이스는 매치하는 타입이라도 그 값이외에 **다른 필드나 메서드가 있음을 전제로 한다.(확장성이 더 좋음으로)**
2. 타입 앨리어스는 **객체의 타입 자체를 의미한다.(확장성이 좋지 않음으로)**

<br>

따라서 인터페이스는 객체 그 자체가 아니라 **클래스나 객체의 일부 속성,** 일부 작동을 정의할때 적합하다.

<br>

## 인덱스 접근 타입

특정한 속성에 **연동되게** 타입을 만들고 싶다면 다음과 같이 작성하면된다.

```tsx
type Animal = {
  name: string;
};

type N1 = Animal[name];
type N2 = Animal[name];
type N3 = ANimal.name; // 오류 객체.속성 꼴의 방식은 사용할 수 없다.
```

객체.속성 꼴의 방식은 사용할 수 없기때문에 N3 는 오류가 난다.

<br>

keyof 연산자와 인덱스 접근 타입을 활용해 키의 타입과 값의 타입을 구할 수 있다.

```tsx
const obj = {
  hello: "world",
  name: Alex,
  age: 28,
};

type keys = keyof typeof obj; // type Keys = 'hello' | 'name' | 'age'
type values = (typeof obj)[keys]; // type Value = string | number
```

<br>

참고

- https://www.typescriptlang.org/ko/docs/handbook/typescript-in-5-minutes.html
- https://www.elancer.co.kr/blog/view?seq=183
- [https://radlohead.gitbook.io/typescript-deep-dive](https://radlohead.gitbook.io/typescript-deep-dive/type-system)
- [https://joshua1988.github.io/ts/guide/type-inference.html#문맥상의-타이핑-contextual-typing](https://joshua1988.github.io/ts/guide/type-inference.html#%EB%AC%B8%EB%A7%A5%EC%83%81%EC%9D%98-%ED%83%80%EC%9D%B4%ED%95%91-contextual-typing)

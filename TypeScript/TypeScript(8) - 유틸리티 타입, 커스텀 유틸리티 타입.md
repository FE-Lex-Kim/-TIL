- [TypeScript(8) - 유틸리티 타입, 커스텀 유틸리티 타입](#typescript8---유틸리티-타입-커스텀-유틸리티-타입)
  - [유틸리티 타입](#유틸리티-타입)
  - [커스텀 유틸리티 타입](#커스텀-유틸리티-타입)
    - [1. 브랜드 타입](#1-브랜드-타입)
    - [2. PickOne 유틸리티 구현](#2-pickone-유틸리티-구현)
    - [1. One](#1-one)
    - [2. Exclude](#2-exclude)
    - [3. One \& ExcludeOne;](#3-one--excludeone)

# TypeScript(8) - 유틸리티 타입, 커스텀 유틸리티 타입

## 유틸리티 타입

1. Partial: 제네릭 타입 T의 모든 속성을 선택적으로 만든다.
2. Required: 제네릭 타입 T의 모든 속성을 필수 속성으로 만든다.
3. Readonly: 제네릭 타입 T의 모든 속성을 읽기 전용으로 만든다.
4. Record<K, T>: 제네릭 타입 K를 키로, 제네릭 타입 T를 값으로 하는 객체를 나타낸다.
   - `Record<K, T>`는 객체를 생성할 때 사용된다.
   - `Record<”Apple” | “Banana”, string>`를 사용하면 `{Apple : string, Banana : string }` 타입이 생성된다.
   - `Record<"Apple" | "Banana", string | number>` 를 사용하면 `{Apple: string | number,  Banana: string | number}` 타입이 생성된다.
5. Pick<T, K>: 제네릭 타입 T에서 특정 속성만을 선택하여 새로운 타입을 생성한다.
6. Omit<T, K>: 제네릭 타입 T에서 특정 속성을 제외하여 새로운 타입을 생성한다.
7. Exclude<T, U>: 제네릭 타입 T에서 U 타입을 제외한 타입을 생성한다.
   - `Exclude<T, U>`는 T에서 U와 일치하는 모든 타입을 제외한 나머지 타입을 나타낸다.
   - `Exclude<string | number | boolean, number>`를 사용하면 `string | boolean` 타입만 남는다.
   - `Exclude<T, U>`는 항상 유니온 타입을 반환하며, T에서 U와 일치하는 모든 타입을 제외하기 때문에 유용하게 사용된다.
8. Extract<T, U>: 제네릭 타입 T에서 U 타입과 일치하는 모든 타입을 추출하여 새로운 타입을 생성한다.
9. NonNullable: 제네릭 타입 T에서 null 또는 undefined를 제외한 모든 타입을 생성한다.
10. ReturnType: 함수 타입 T의 반환 타입을 추출한다.
11. Parameters: 함수 타입 T의 매개변수 타입을 추출한다.
12. ConstructorParameters: 생성자 함수 타입 T의 매개변수 타입을 추출한다.
13. InstanceType: 클래스 타입 T의 인스턴스 타입을 추출한다.

<br>

## 커스텀 유틸리티 타입

유틸리티 타입으로만 만들수 없는 더 복잡한 유틸리티 함수를 커스텀으로 제작해서 사용하면 된다.

타입스크립트에서 서로다른 2개 이상의 객체를 유니온 타입으로 받을때 타입검사가 제대로 진행되지 않는 이슈가 있다. 이 문제를 해결하기 위한 PickOne이라는 커스텀 유틸리티 타입을 구현해보자.

예시코드

```tsx
type Person1 = {
  name: string;
};

type Person2 = {
  city: string;
};

function comparePerson(Person: Person1 | Person2) {
  ...
}

comparePerson({ name: "alex", city: 'Bucheon' });
```

위에서 Person1과 Person2를 비교해서 해당하는 하나의 타입이 무엇인지 검사해야한다.

하지만 name, age, city 같이 각각 타입 객체별로 들어있지 않는 속성이 있는데도 에러가 발생하지 않는다.

그 이유는 유니온은 합집합 이기 때문이다. 따라서 Person1과 Person2 속성이 모두 포함되어도 합집합이기 때문에 에러가 발생하지 않는다.

이방법을 해결하기 위해서는 두가지 방법이 대표적으로 있다

1. 브랜드 타입
2. PickOne 유틸리티 함수

<br>

### 1. 브랜드 타입

브랜드 타입은 이전에 정리했던 글처럼 각 객체별로 고유한 속성 가지게 하면된다.

```tsx
type Person1 = {
  __type: "alex";
  name: string;
};

type Person2 = {
  __type: "james";
  city: string;
};
```

이렇게 하면 \_\_type을 하나한 다 넣어주어야 한다는 불편함이 생긴다.

<br>

### 2. PickOne 유틸리티 구현

Person1 일때는 city 속성을, Person2일때는 name 속성을 받지 못하게 해야한다.

자신의 객체 안에 속해있지 않는 속성은 옵셔널 + undefined 값으로 지정하는 방법이 있다.

값에 undefined 값을 넣지 않는 이상 타입 에러가 발생할 것이다.

예를 들어

```tsx
{name : string, city? : undefined} | {name? : undefined, city : string}
```

<br>

구현방법

1. `type One<T>` 만들기
2. `type ExcludeOne<T>` 만들기

### 1. One<T>

```tsx
type One<T> = { [K in keyof T]: Record<K, T[K]> }[keyof T];
```

1. `[K in keyof T]` 는 T 객체에서 각 키들을 K로 의미한다.
2. `Record<K, T[K]>` 는 객체의 키 K와 키에 대한 값 `T[K]`를 1 : 1로 객체 타입으로 만든다.
3. `[K in keyof T]: Record<K, T[K]>` 는 키 K에 대한 값으로 `Record<K, T[K]>`로 만들어진 타입이 된다.
4. `[keyof T]` 는 만들어진 객체 `{ [K in keyof T]: Record<K, T[K]> }`에서 T의 키 속성으로 접근해 값만을 가져온다.

예시

```tsx
type Obj1 = {
  name: string;
  age: number;
  city: string;
};

// {name: string} | {ages: number} | {city: string}
type Obj2 = One<Obj1>;
```

<br>

### 2. Exclude<T>

```tsx
type ExcludeOne<T> = {
  [K in keyof T]: Partial<Record<Exclude<keyof T, K>, undefined>>;
}[keyof T];
```

1. `[K in keyof T]` 는 T 객체에서 각 키들을 K로 의미한다.
2. `Exclude<keyof T, K>`는 객체 T의 키들 중에서 K만 뺀 나머지 객체 타입을 말한다 이 값을 A라고 하자.
3. `Record<A, undefined>` 는 각 A를 키로 하고 그에 대한 값을 undefined로 한다. 이 값을 B로 하자.
4. `Partial<B>` 는 객체 B타입을 모두 옵셔널로 만든다. 이값을 C로 하자.
5. `{[K in keyof T]: C}[keyof T]` 는 새롭게 만들어진 `{[K in keyof T]: C}` 의 C만을 접근해 값만 가져온다.

```tsx
type Obj1 = {
  name: string;
  age: number;
  city: string;
};

// { age?: undefined; city?: undefined; }
// | { name?: undefined; city?: undefined; }
// | { name?: undefined; age?: undefined; }
type Obj2 = ExcludeOne<Obj1>;
```

<br>

### 3. One<A> & ExcludeOne<B>;

```tsx
type Obj1 = {
  name: string;
  age: number;
};
type Obj2 = {
  city: string;
};

type Mix = One<Obj1> & ExcludeOne<Obj2>;

// 밑에와 같다.
type Mix = ({ name: string } | { age: number }) & { city?: undefined };
// { name: string; city?: undefined; }| { age: number; city?: undefined; }
```

<br>

참고

- 우아한 타입스크립트 책을 정리한 글입니다.

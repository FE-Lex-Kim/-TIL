- [TypeScript(7) - satisfies, 타입 확장](#typescript7---satisfies-타입-확장)
  - [satisfies](#satisfies)
  - [타입 확장](#타입-확장)
    - [1. 유니온 타입](#1-유니온-타입)
    - [2. 교차타입](#2-교차타입)
    - [3. extends와 교차타입](#3-extends와-교차타입)
    - [4. 확장 적용하기](#4-확장-적용하기)

# TypeScript(7) - satisfies, 타입 확장

<br>

## satisfies

타입 추론을 그대로 활용하면서 추가로 타입 검사를 하고 싶을때 사용한다.

아래의 코드는 일부로 uinverse의 속성 키중 sirius 대신 siriius로 오타를 냈다.

그리고 굳이 타이핑을 하지않고 추론을 통해 타입을 정했다.

```tsx
const universe = {
  sun: "star",
  sriius: "star",
  earth: { type: "planet", parent: "sun" },
};
```

이럴때 타입 추론이 아니라 타입을 정할때 sriius의 오타를 잡아낼 수 있는 방법이 있다.

<br>

```tsx
const universe: {
  [key in "sun" | "sirius" | "earth"]: { type: string; parent: string } | string;
} = {
  sun: "star",
  sriius: "star",
  earth: { type: "planet", parent: "sun" },
};
```

위와 같이 인덱스 시그니처를 사용해 타이핑 해서 siriius의 오타를 잡을 수 있다.

<br>

다만 속성 값을 사용할 때가 문제이다. earth의 타입이 객체라는 것을 제대로 잡아내지 못한다.

```tsx
universe.earth.type; // <--- 에러가 발생한다.
```

속성 값의 타입을 객체와 문자열의 유니언으로 표기했기 때문에 earth가 문자열 일 수 도 있다고 생각하는것이다.

<br>

이럴때 처음처럼 타입을 추론하는 이점을 사용하고 sriius에서 오타가 났음을 알리는 방법이 있다.

satisfies 연산자를 사용하면 된다.

```tsx
const universe = {
  sun: "star",
  sriius: "star",
  earth: { type: "planet", parent: "sun" },
} satisfies {
  [key in "sun" | "sirius" | "earth"]: { type: string; parent: string } | string;
};
```

이러면 universe 타입은 타입 추론된것을 그대로 사용하면서, 각각 속성들은 satisefies에 적은 타입으로 **다시 검사 한다.**

여기서 sririus 오타가 발견된다.

또한 `universe.earth.type` 도 에러없이 쓸 수 있다.

<br>

## 타입 확장

타입 확장은 기존타입을 사용해서 새로운 타입을 정의하는 것을 말한다.

타입스크립트는 interface, type 키워드로 타입을 정의하고

extends, 교차타입(&), 유니온 타입( | )을 사용해서 타입을 확장한다.

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

참고

- 타입스크립트 교과서 책을 정리한 글입니다.
- 우아한 타입스크립트 책을 정리한 글입니다.

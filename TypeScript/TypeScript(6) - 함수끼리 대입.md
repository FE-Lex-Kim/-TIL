- [TypeScript(6) - 함수끼리 대입](#typescript6---함수끼리-대입)
  - [함수끼리 대입](#함수끼리-대입)
    - [반환값의 경우](#반환값의-경우)
    - [매개변수의 경우](#매개변수의-경우)
    - [메서드의 경우](#메서드의-경우)

# TypeScript(6) - 함수끼리 대입

<br>

## 함수끼리 대입

함수끼리 대입하는 방법은 공변성과, 반공변성, 이변성에 대해 알아야한다.

공변성 : A → B 일때 T<A> → T<B>인 경우

반공변성 : A → B 일때 T<B> → T<A> 인경우

이번성 : A → B 일때 T<A> → T<B>도 되고 T<B> → T<A> 되는 경우

<br>

여기서 A와 B는 반환값을 의미하고 T<a>, T<b>는 함수라고 생각하면 된다.

예시를 보면 더 쉽다.

<br>

### 반환값의 경우

```tsx
function a(x: string): string {}
type B = (x: string) => string | number;

let b: B = a; // a를 b에 할당. b가 a보다 더 크므로 할당 가능
```

함수 a`를 타입 `b` 에 할당할 수 있는 이유는  `b` 의 반환 타입이  `a` 의 반환 타입보다 더 포괄적인 넓은 범위를 가지고 있기 때문이다.

여기서 a → b 이고 T<a> → T<b> 를 할당할 수 있으므로 공변성을 갖는다고 할 수 있다.

**항상 반환값은 공변성을 가지고 있다고 보면된다. a → b(b가 크면 항상 a를 대입할 수 있다.)**

<br>

### 매개변수의 경우

strict 옵션이 true 인경우 반공변성을 가진다.

```tsx
function a(x: string | number): number {}
type B = (x: string) => number;

let b: B = a; // 매개변수 a가 b의 매개변수보다 큰데도 대입할 수 있다. 반공변성
```

여기서 반환값은 같고 매개변수의 타입만 다르다.

a함수의 매개변수는 string | number이고 b함수의 매개변수는 string이다.

즉, b → a 인 상황이지만, 여기서 매개변수는 반공변성을 가진다고 했으므로 T<a> → T<b> 이 된다.

반대는 에러가 발생한다.(T<b> → T<a>는 에러가 발생함)

<br>

**결론은 매개변수는 반공변성을 가진다.**

하지만 주의할 점으로, 여기서 strict 옵션을 false로 해제하면 T<b> → T<a>는 에러가 발생하지 않는다.

즉, strict 옵션이 아닐때는 이변성을 가진다.

<br>

### 메서드의 경우

객체의 메서드인 경우는 메서드를 타이핑 하는 방식에 따라 다르다.

```tsx
interface SayMethod {
  say(a: string | number): string;
}

interface SayFunction {
  say: (a: string | number) => string;
}

interface SayCall {
  say: {
    (a: string | number): string;
  };
}

const sayFunc = (a: string) => "hello";
const MyAddingMethod: SayMethod = {
  say: sayFunc, // 이변성
};
const MyAddingFunction: SayFunction = {
  say: sayFunc, // 에러 -> 반공변성
};
const MyAddingCall: SayCall = {
  say: sayFunc, // 에러 -> 반공변성
};
```

기본적으로 sayFunc 함수의 타입은 `(a: string) ⇒ string` 이라서 매개변수 반공변성으로 인해 `(a: string | number) ⇒ string` 에 대입할 수 없다.

```tsx
const obj = {
  a: B,
};
```

**보통 여기서 a 메서드를 B타입에 대입한다라고 한다.**

**B타입을 a 메서드에 대입한다가 아니다.**

<br>

참고

- 타입스크립트 교과서를 정리한 글입니다.

# Symbol 타입

**심볼(symbol)은 ES6에서 도입된 7번째 데이터 타입으로 변경 불가능한 원시 타입의 값이다.**

심볼 값은 **다른 값과 중복되지 않는 유일무이한 값이다.**

따라서 주로 이름의 충돌 위험이 없는 **유일한 프로퍼티 키를 만들기 위해 사용한다.**

- 참고로 **프로퍼티 키로 사용할 수 있는 값은 문자열과 심볼** 값만 가능하다.

## 1. Symbol 생성

심볼은 Symbol 함수를 호출해서 생성한다.

```jsx
const mySymbol = Symbol();
console.log(typeof mySymbol); // symbol
```

Symbol 함수에 인수를 문자열로 전달할 수 있다.

심볼 값에 대한 디버깅용 설명용으로 사용된다.

```jsx
// 심벌 값에 대한 설명이 같더라도 유일무이한 심벌 값을 생성한다.
const mySymbol1 = Symbol("mySymbol");
const mySymbol2 = Symbol("mySymbol");

console.log(mySymbol1 === mySymbol2); // false
```

## 2. Symbol.for, Symbol.keyFor 메서드

### Symbol.for

`Symbol.for(문자열)`로 인수로 전달받은 **문자열을 키로 사용한다.**

**문자열에 해당하는 키와 일치하는 심벌 값을 검색한다.**

- **검색에 성공하면 검색된 심벌 값을 반환한다.**
- 검색에 실패하면 새로운 심벌 값을 생성해서, **인수로 전달받은 문자열을 키로한 심벌값을 반환한다.**

**그럼 `Symbol.for`을언제 사용할까?**

일반적인 Symbol 함수를 호출해서 생성하면 **키를 설정하지 않아, 심볼 레지스터리에서 키를 저장할 수 없다.**

**따라서 심벌 레지스트리에 등록되어 관리되지 않는다.**

하지만 `**Symbol.for`을 사용하면 키를 등록할 수 있으므로, 심벌 레지스트리에 등록해 관리가능하다.\*\*

따라서 애플리케이션 전역에서 심벌 값이 중복되지 않는 유일무이한 심벌 값을 생성할 수 있다.

### Symbol.keyFor

Symbol.keyFor을 통해 심벌 레지스트리에서 **심벌의 키를 추출할 수 있다.**

```jsx
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for("mySymbol");
// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
Symbol.keyFor(s1); // -> mySymbol

// Symbol 함수를 호출하여 생성한 심벌 값은 전역 심벌 레지스트리에 등록되어 관리되지 않는다.
const s2 = Symbol("foo");
// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
Symbol.keyFor(s2); // -> undefined
```

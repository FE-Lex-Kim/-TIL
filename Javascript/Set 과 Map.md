# Set 과 Map

- 출처 [모던 자바스크립트 Deep Dive](http://www.yes24.com/Product/Goods/92742567?OzSrank=1)을 보고 정리한 내용입니다.

<br>

# 1. Set

<br>

**Set 객체는 중복되지 않는 유일한 값들의 집합(set)이다.**

Set 객체는 **배열**과 유사하지만 차이가 있다.

<br>

## 1.1. Set 객체의 생성

<br>

Set 객체는 Set 생성자 함수로 생성한다. 

Set 생성자 함수에 인수를 전달하지 않으면 빈 Set 객체가 생성된다.

```jsx
const set = new Set();
console.log(set); // Set(0) {}
```

<br>

**Set 생성자 함수는 이터러블을 인수로 전달받아 Set 객체를 생성한다.** 

**이때 이터러블의 중복된 값은 Set 객체에 요소로 저장되지 않는다.**

```jsx
const set1 = new Set([1, 2, 3, 3]);
console.log(set1); // Set(3) {1, 2, 3}

const set2 = new Set('hello');
console.log(set2); // Set(4) {"h", "e", "l", "o"}
```

<br>

중복을 허용하지 않는 Set 객체의 특성을 활용하여 배열에서 중복된 요소를 제거할 수 있다.

```jsx
// 배열의 중복 요소 제거
const uniq = array => array.filter((v, i, self) => self.indexOf(v) === i);
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]

// Set을 사용한 배열의 중복 요소 제거
const uniq = array => [...new Set(array)];
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]
```

<br>

## 1.2. 요소 개수 확인

<br>

Set 객체의 요소 개수를 확인할 때는 Set.prototype.size 프로퍼티를 사용한다.

```jsx
const { size } = new Set([1, 2, 3, 3]);
console.log(size); // 3
```

<br>

size 프로퍼티는 setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티다. 따라서 size 프로퍼티에 숫자를 할당하여 Set 객체의 요소 개수를 변경할 수 없다.

```jsx
const set = new Set([1, 2, 3]);

console.log(Object.getOwnPropertyDescriptor(Set.prototype, 'size'));
// {set: undefined, enumerable: false, configurable: true, get: ƒ}

set.size = 10; // 무시된다.
console.log(set.size); // 3
```

<br>

## 1.3. 요소 추가

<br>

Set 객체에 요소를 추가할 때는 Set.prototype.add 메서드를 사용한다.

```jsx
const set = new Set();
console.log(set); // Set(0) {}

set.add(1);
console.log(set); // Set(1) {1}
```

<br>

add 메서드는 새로운 요소가 추가된 Set 객체를 반환한다. 

따라서 add 메서드를 호출한 후에 add 메서드를 연속적으로 호출(method chaining)할 수 있다.

```jsx
const set = new Set();

set.add(1).add(2);
console.log(set); // Set(2) {1, 2}
```

<br>

Set 객체는 객체나 배열과 같이 자바스크립트의 모든 값을 요소로 저장할 수 있다.

```jsx
const set = new Set();

set
  .add(1)
  .add('a')
  .add(true)
  .add(undefined)
  .add(null)
  .add({})
  .add([]);

console.log(set); // Set(7) {1, "a", true, undefined, null, {}, []}
```

<br>

## 1.4. 요소 존재 여부 확인

<br>

Set 객체에 특정 요소가 존재하는지 확인하려면 Set.prototype.has 메서드를 사용한다.

```jsx
const set = new Set([1, 2, 3]);

console.log(set.has(2)); // true
console.log(set.has(4)); // false
```

<br>

## 1.5. 요소 삭제

<br>

Set 객체의 특정 요소를 삭제하려면 Set.prototype.delete 메서드를 사용한다.

```jsx
const set = new Set([1, 2, 3]);

// 요소 2를 삭제한다.
set.delete(2);
console.log(set); // Set(2) {1, 3}

// 요소 1을 삭제한다.
set.delete(1);
console.log(set); // Set(1) {3}
```

<br>

delete 메서드는 삭제 성공 여부를 나타내는 불리언 값을 반환한다.

```jsx
const set = new Set([1, 2, 3]);

// delete는 불리언 값을 반환한다.
set.delete(1).delete(2); // TypeError: set.delete(...).delete is not a function
```

<br>

## 1.6. 요소 일괄 삭제

<br>

Set 객체의 모든 요소를 일괄 삭제하려면 Set.prototype.clear 메서드를 사용한다.

clear 메서드는 언제나 undefined를 반환한다.

```jsx
const set = new Set([1, 2, 3]);

set.clear();
console.log(set); // Set(0) {}
```

<br>

## 1.7. 요소 순회

<br>

Set 객체의 요소를 순회하려면 Set.prototype.forEach 메서드를 사용한다.

<br>

- 첫 번재 인수 : 현재 순회 중인 요소값
- 두 번재 인수 : 현재 순회 중인 요소값
- 세 번재 인수 : 현재 순회 중인 Set 객체 자체

<br>

```jsx
const set = new Set([1, 2, 3]);

set.forEach((v, v2, set) => console.log(v, v2, set));
/*
1 1 Set(3) {1, 2, 3}
2 2 Set(3) {1, 2, 3}
3 3 Set(3) {1, 2, 3}
*/

<br>
```

**Set 객체는 이터러블이다.**

for…of 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링 할당의 대상이 될 수도 있다.

```jsx
const set = new Set([1, 2, 3]);

// Set 객체는 Set.prototype의 Symbol.iterator 메서드를 상속받는 이터러블이다.
console.log(Symbol.iterator in set); // true

// 이터러블인 Set 객체는 for...of 문으로 순회할 수 있다.
for (const value of set) {
  console.log(value); // 1 2 3
}

// 이터러블인 Set 객체는 스프레드 문법의 대상이 될 수 있다.
console.log([...set]); // [1, 2, 3]

// 이터러블인 Set 객체는 배열 디스트럭처링 할당의 대상이 될 수 있다.
const [a, ...rest] = [...set];
console.log(a, rest); // 1, [2, 3]
```

<br>

## 1.8. 집합 연산

<br>

### 1.8.1. 교집합

<br>

**방법1**

```jsx
Set.prototype.intersection = function (set) {
  const result = new Set();

  for (const value of set) {
    // 2개의 set의 요소가 공통되는 요소이면 교집합의 대상이다.
    if (this.has(value)) result.add(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 교집합
console.log(setA.intersection(setB)); // Set(2) {2, 4}
// setB와 setA의 교집합
console.log(setB.intersection(setA)); // Set(2) {2, 4}
```

<br>

**방법2**

```jsx
Set.prototype.intersection = function (set) {
  return new Set([...this].filter(v => set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 교집합
console.log(setA.intersection(setB)); // Set(2) {2, 4}
// setB와 setA의 교집합
console.log(setB.intersection(setA)); // Set(2) {2, 4}
```

<br>

### 1.8.2. 합집합

<br>

**방법1**

```jsx
Set.prototype.union = function (set) {
  // this(Set 객체)를 복사
  const result = new Set(this);

  for (const value of set) {
    // 합집합은 2개의 Set 객체의 모든 요소로 구성된 집합이다. 중복된 요소는 포함되지 않는다.
    result.add(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 합집합
console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
// setB와 setA의 합집합
console.log(setB.union(setA)); // Set(4) {2, 4, 1, 3}
```

<br>

**방법2**

```jsx
Set.prototype.union = function (set) {
  return new Set([...this, ...set]);
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 합집합
console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
// setB와 setA의 합집합
console.log(setB.union(setA)); // Set(4) {2, 4, 1, 3}
```

<br>

### 1.8.3. 차집합

<br>

**방법1**

```jsx
Set.prototype.difference = function (set) {
  // this(Set 객체)를 복사
  const result = new Set(this);

  for (const value of set) {
    // 차집합은 어느 한쪽 집합에는 존재하지만 다른 한쪽 집합에는 존재하지 않는 요소로 구성된 집합이다.
    result.delete(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA에 대한 setB의 차집합
console.log(setA.difference(setB)); // Set(2) {1, 3}
// setB에 대한 setA의 차집합
console.log(setB.difference(setA)); // Set(0) {}
```

<br>

**방법2**

```jsx
Set.prototype.difference = function (set) {
  return new Set([...this].filter(v => !set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA에 대한 setB의 차집합
console.log(setA.difference(setB)); // Set(2) {1, 3}
// setB에 대한 setA의 차집합
console.log(setB.difference(setA)); // Set(0) {}
```

<br>

### 1.8.4. 부분 집합과 상위 집합

<br>

**방법1**

```jsx
// this가 subset의 상위 집합인지 확인한다.
Set.prototype.isSuperset = function (subset) {
  for (const value of subset) {
    // superset의 모든 요소가 subset의 모든 요소를 포함하는지 확인
    if (!this.has(value)) return false;
  }

  return true;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA가 setB의 상위 집합인지 확인한다.
console.log(setA.isSuperset(setB)); // true
// setB가 setA의 상위 집합인지 확인한다.
console.log(setB.isSuperset(setA)); // false
```

<br>

**방법2**

```jsx
// this가 subset의 상위 집합인지 확인한다.
Set.prototype.isSuperset = function (subset) {
  const supersetArr = [...this];
  return [...subset].every(v => supersetArr.includes(v));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA가 setB의 상위 집합인지 확인한다.
console.log(setA.isSuperset(setB)); // true
// setB가 setA의 상위 집합인지 확인한다.
console.log(setB.isSuperset(setA)); // false
```

<br>

# 2. Map

<br>

**Map 객체는 키와 값의 쌍으로 이루어진 컬렉션이다.**

Map 객체는 **객체**와 유사하지만 차이가 있다.

<br>

## 2.1. Map 객체의 생성

<br>

Map 객체는 Map 생성자 함수로 생성한다.

```jsx
const map = new Map();
console.log(map); // Map(0) {}
```

<br>

Map 생성자 함수는 이터러블을 인수로 전달받아 Map 객체를 생성한다.

이때 인수로 전달되는 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성되어야 한다.

```jsx
const map1 = new Map([['key1', 'value1'], ['key2', 'value2']]);
console.log(map1); // Map(2) {"key1" => "value1", "key2" => "value2"}

const map2 = new Map([1, 2]); // TypeError: Iterator value 1 is not an entry object
```

<br>

중복된 키는 Map 객체에 요소로 저장되지 않는다.

```jsx
const map = new Map([['key1', 'value1'], ['key1', 'value2']]);
console.log(map); // Map(1) {"key1" => "value1"}
```

<br>

## 2.2. 요소 개수 확인

<br>

Map 객체의 요소 개수를 확인할 때는 Map.prototype.size 프로퍼티를 사용한다.

```jsx
const { size } = new Map([['key1', 'value1'], ['key2', 'value2']]);
console.log(size); // 2
```

<br>

size 프로퍼티는 setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티다. 

따라서 size 프로퍼티에 숫자를 할당하여 Map 객체의 요소 개수를 변경할 수 없다.

```jsx
const map = new Map([['key1', 'value1'], ['key2', 'value2']]);

console.log(Object.getOwnPropertyDescriptor(Map.prototype, 'size'));
// {set: undefined, enumerable: false, configurable: true, get: ƒ}

map.size = 10; // 무시된다.
console.log(map.size); // 2
```

<br>

## 2.3. 요소 추가

<br>

Map 객체에 요소를 추가할 때는 Map.prototype.set 메서드를 사용한다.

```jsx
const map = new Map();
console.log(map); // Map(0) {}

map.set('key1', 'value1');
console.log(map); // Map(1) {"key1" => "value1"}
```

<br>

set 메서드는 새로운 요소가 추가된 Map 객체를 반환한다.

따라서 set 메서드를 호출한 후에 set 메서드를 연속적으로 호출(method chaining)할 수 있다.

```jsx
const map = new Map();

map
  .set('key1', 'value1')
  .set('key2', 'value2');

console.log(map); // Map(2) {"key1" => "value1", "key2" => "value2"}
```

<br>

**중요!**

객체는 문자열 또는 심벌 값만 키로 사용할 수 있다. 

하지만 Map 객체는 키 타입에 제한이 없다. 따라서 객체를 포함한 모든 값을 키로 사용할 수 있다. 

이는 Map 객체와 일반 객체의 가장 두드러지는 차이점이다.

```jsx
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

// 객체도 키로 사용할 수 있다.
map
  .set(lee, 'developer')
  .set(kim, 'designer');

console.log(map);
// Map(2) { {name: "Lee"} => "developer", {name: "Kim"} => "designer" }
```

<br>

## 2.4. 요소 취득

<br>

Map 객체에서 특정 요소를 취득하려면 Map.prototype.get 메서드를 사용한다. 

get 메서드의 인수로 키를 전달하면 Map 객체에서 인수로 전달한 키를 갖는 값을 반환한다.

Map 객체에서 인수로 전달한 키를 갖는 요소가 존재하지 않으면 undefined를 반환한다.

```jsx
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

map
  .set(lee, 'developer')
  .set(kim, 'designer');

console.log(map.get(lee)); // developer
console.log(map.get('key')); // undefined
```

<br>

## 2.5. 요소 존재 여부 확인

<br>

Map 객체에 특정 요소가 존재하는지 확인하려면 Map.prototype.has 메서드를 사용한다.

```jsx
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

console.log(map.has(lee)); // true
console.log(map.has('key')); // false
```

<br>

## 2.6. 요소 삭제

<br>

Map 객체의 요소를 삭제하려면 Map.prototype.delete 메서드를 사용한다.

delete 메서드는 삭제 성공 여부를 나타내는 불리언 값을 반환한다.

```jsx
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.delete(kim);
console.log(map); // Map(1) { {name: "Lee"} => "developer" }
```

<br>

delete 메서드는 삭제 성공 여부를 나타내는 불리언 값을 반환한다. 

따라서 set 메서드와 달리 연속적으로 호출(method chaining)할 수 없다.

```jsx
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.delete(lee).delete(kim); // TypeError: map.delete(...).delete is not a function
```

<br>

## 2.7. 요소 일괄 삭제

<br>

Map 객체의 요소를 일괄 삭제할 때는 Map.prototype.clear 메서드를 사용한다.

clear 메서드는 언제나 undefined를 반환한다.

```jsx
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.clear();
console.log(map); // Map(0) {}
```

<br>

## 2.8. 요소 순회

<br>

Map 객체의 요소를 순회하려면 Map.prototype.forEach 메서드를 사용한다.

<br>

- 첫 번재 인수 : 현재 순회 중인 요소값
- 두 번재 인수 : 현재 순회 중인 요소키
- 세 번재 인수 : 현재 순회 중인 Map 객체 자체

<br>

```jsx
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.forEach((v, k, map) => console.log(v, k, map));
/*
developer {name: "Lee"} Map(2) {
  {name: "Lee"} => "developer",
  {name: "Kim"} => "designer"
}
designer {name: "Kim"} Map(2) {
  {name: "Lee"} => "developer",
  {name: "Kim"} => "designer"
}
*/
```

<br>

**Map 객체는 이터러블이다.** 

따라서 for…of 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링 할당의 대상이 될 수도 있다.

```jsx
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

// Map 객체는 Map.prototype의 Symbol.iterator 메서드를 상속받는 이터러블이다.
console.log(Symbol.iterator in map); // true

// 이터러블인 Map 객체는 for...of 문으로 순회할 수 있다.
for (const entry of map) {
  console.log(entry); // [{name: "Lee"}, "developer"]  [{name: "Kim"}, "designer"]
}

// 이터러블인 Map 객체는 스프레드 문법의 대상이 될 수 있다.
console.log([...map]);
// [[{name: "Lee"}, "developer"], [{name: "Kim"}, "designer"]]

// 이터러블인 Map 객체는 배열 디스트럭처링 할당의 대상이 될 수 있다.
const [a, b] = map;
console.log(a, b); // [{name: "Lee"}, "developer"]  [{name: "Kim"}, "designer"]
```

<br>

Map 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드를 제공한다.

```jsx
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

// Map.prototype.keys는 Map 객체에서 요소키를 값으로 갖는 이터레이터를 반환한다.
for (const key of map.keys()) {
  console.log(key); // {name: "Lee"} {name: "Kim"}
}

// Map.prototype.values는 Map 객체에서 요소값을 값으로 갖는 이터레이터를 반환한다.
for (const value of map.values()) {
  console.log(value); // developer designer
}

// Map.prototype.entries는 Map 객체에서 요소키와 요소값을 값으로 갖는 이터레이터를 반환한다.
for (const entry of map.entries()) {
  console.log(entry); // [{name: "Lee"}, "developer"]  [{name: "Kim"}, "designer"]
}
```
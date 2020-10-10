# RegExp

<br>

## 1. 정규 표현식이란?

<br>

정규 표현식(regular expression, regexp)은 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어(formal language)다.

<br>

정규 표현식은 문자열을 대상으로 패턴 매칭 기능을 제공한다.

패턴 매칭 기능이란 특정 패턴과 일치하는 문자열을 검색하거나 추출 또는 치환할 수 있는 기능을 말한다.

```jsx
// 사용자로부터 입력받은 휴대폰 전화번호
const tel = '010-1234-567팔';

// 정규 표현식 리터럴로 휴대폰 전화번호 패턴을 정의한다.
const regExp = /^\d{3}-\d{4}-\d{4}$/;

// tel이 휴대폰 전화번호 패턴에 매칭하는지 테스트(확인)한다.
regExp.test(tel); // -> false
```

<br>

## 2. 정규 표현식의 생성

<br>

정규 표현식 객체(RegExp 객체)를 생성하기 위해서는 

- 정규 표현식 리터럴
- RegExp 생성자 함수를 사용할 수 있다.

<br>

일반적인 방법은 정규 표현식 리터럴을 사용한다.

<br>

정규 표현식 리터럴은 패턴과 플래그로 구성된다.

```jsx
const target = 'Is this all there is?';

// 패턴: is
// 플래그: i => 대소문자를 구별하지 않고 검색한다.
const regexp = /is/i;

// test 메서드는 target 문자열에 대해 정규표현식 regexp의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.
regexp.test(target); // -> true
```

<br>

생성자 함수를 사용하여 RegExp 객체를 생성할 수도 있다.

```jsx
const target = 'Is this all there is?';

const regexp = new RegExp(/is/i); // ES6
// const regexp = new RegExp(/is/, 'i');
// const regexp = new RegExp('is', 'i');

regexp.test(target); // -> true
```

<br>

## 3. RegExp 메서드

<br>

### 3.1. RegExp.prototype.exec

exec 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 배열로 반환한다.

```jsx
const target = 'Is this all there is?';
const regExp = /is/;

regExp.exec(target); // -> ["is", index: 5, input: "Is this all there is?", groups: undefined]
```

<br>

### 3.2. RegExp.prototype.test

test 메서드는 문자열에서 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.

```jsx
const target = 'Is this all there is?';
const regExp = /is/;

regExp.test(target); // -> true
```

<br>

### 3.3. String.prototype.match

String 표준 빌트인 객체가 제공하는 match 메서드는 문자열과 정규 표현식과의 매칭 정보를 배열로 반환한다.

```jsx
const target = 'Is this all there is?';
const regExp = /is/;

target.match(regExp); // -> ["is", index: 5, input: "Is this all there is?", groups: undefined]
```

<br>

**exec 메서드와 차이점**

String.prototype.match 메서드는 g 플래그가 지정되면 모든 매칭 결과를 배열로 반환한다.

```jsx
const target = 'Is this all there is?';
const regExp = /is/g;

target.match(regExp); // -> ["is", "is"]
```

<br>

## 4. 플래그

<br>

패턴과 함께 정규 표현식을 구성하는 플래그는 정규 표현식의 검색 방식을 설정하기 위해 사용한다.

<br>

**i플래그**

대소문자를 구별하지 않고 패턴을 검색한다.

<br>

**g플래그**

대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다.

<br>

**m플래그**

문자열의 행이 바뀌더라도 패턴 검색을 계속한다.

<br>

어떠한 플래그를 사용하지 않은 경우 대소문자를 구별해서 패턴을 검색한다.

문자열에 패턴 검색 매칭 대상이 1개 이상 존재해도 첫 번째 매칭한 대상만 검색하고 종료한다.

```jsx
const target = 'Is this all there is?';

// target 문자열에서 is 문자열을 대소문자를 구별하여 한 번만 검색한다.
target.match(/is/);
// -> ["is", index: 5, input: "Is this all there is?", groups: undefined]

// target 문자열에서 is 문자열을 대소문자를 구별하지 않고 한 번만 검색한다.
target.match(/is/i);
// -> ["Is", index: 0, input: "Is this all there is?", groups: undefined]

// target 문자열에서 is 문자열을 대소문자를 구별하여 전역 검색한다.
target.match(/is/g);
// -> ["is", "is"]

// target 문자열에서 is 문자열을 대소문자를 구별하지 않고 전역 검색한다.
target.match(/is/ig);
// -> ["Is", "is", "is"]
```

<br>

## 5. 패턴

<br>

정규 표현식은 패턴과 플래그로 구성된다.

패턴은 문자열의 일정한 규칙을 표현하기 위해 사용하며, 정규 표현식의 플래그는 정규 표현식의 검색 방식을 설정하기 위해 사용한다.

<br>

패턴은 /으로 열고 닫으며 문자열의 따옴표는 생략한다.

<br>

### 5.1. 문자열 검색

<br>

정규 표현식의 패턴에 문자 또는 문자열을 지정하면 검색 대상 문자열에서 패턴으로 지정한 문자 또는 문자열을 검색한다.

```jsx
const target = 'Is this all there is?';

// 'is' 문자열과 매치하는 패턴. 플래그가 생략되었으므로 대소문자를 구별한다.
const regExp = /is/;

// target과 정규 표현식이 매치하는지 테스트한다.
regExp.test(target); // -> true

// target과 정규 표현식의 매칭 결과를 구한다.
target.match(regExp);
// -> ["is", index: 5, input: "Is this all there is?", groups: undefined]
```

<br>

대소문자를 구별하지 않고 검색하려면 플래그 i를 사용한다.

```jsx
const target = 'Is this all there is?';

// 'is' 문자열과 매치하는 패턴. 플래그 i를 추가하면 대소문자를 구별하지 않는다.
const regExp = /is/i;

target.match(regExp);
// -> ["Is", index: 0, input: "Is this all there is?", groups: undefined]
```

<br>

대상 문자열 내에서 패턴과 매치하는 모든 문자열을 전역 검색하려면 플래그 g를 사용한다.

```jsx
const target = 'Is this all there is?';

// 'is' 문자열과 매치하는 패턴.
// 플래그 g를 추가하면 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다.
const regExp = /is/ig;

target.match(regExp); // -> ["Is", "is", "is"]
```

<br>

### 5.2. 임의의 문자열 검색

<br>

.은 임의의 문자 한 개를 의미한다. 문자의 내용은 무엇이든 상관없다.

```jsx
const target = 'Is this all there is?';

// 임의의 3자리 문자열을 대소문자를 구별하여 전역 검색한다.
const regExp = /.../g;

target.match(regExp); // -> ["Is ", "thi", "s a", "ll ", "the", "re ", "is?"]
```

<br>

### 5.3. 반복 검색

<br>

{m,n}은 앞선 패턴이 최소 m번, 최대 n번 반복되는 문자열을 의미한다.

```jsx
const target = 'A AA B BB Aa Bb AAA';

// 'A'가 최소 1번, 최대 2번 반복되는 문자열을 전역 검색한다.
const regExp = /A{1,2}/g;

target.match(regExp); // -> ["A", "AA", "A", "AA", "A"]
```

<br>

{n}은 앞선 패턴이 n번 반복되는 문자열을 의미한다. 즉, {n}은 {n, n}과 같다.

```jsx
const target = 'A AA B BB Aa Bb AAA';

// 'A'가 2번 반복되는 문자열을 전역 검색한다.
const regExp = /A{2}/g;

target.match(regExp); // -> ["AA", "AA"]
```

<br>

{n,}은 앞선 패턴이 최소 n번 이상 반복되는 문자열을 의미한다.

```jsx
const target = 'A AA B BB Aa Bb AAA';

// 'A'가 최소 2번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /A{2,}/g;

target.match(regExp); // -> ["AA", "AAA"]
```

<br>

+는 앞선 패턴이 최소 한 번 이상 반복되는 문자열을 의미한다. 즉, +는 {1,}과 같다.

```jsx
const target = 'A AA B BB Aa Bb AAA';

// 'A'가 최소 한 번 이상 반복되는 문자열('A, 'AA', 'AAA', ...)을 전역 검색한다.
const regExp = /A+/g;

target.match(regExp); // -> ["A", "AA", "A", "AAA"]
```

<br>

?는 앞선 패턴이 최대 한 번(0번 포함) 이상 반복되는 문자열을 의미한다. 즉, ?는 {0,1}과 같다.

```jsx
const target = 'color colour';

// 'colo' 다음 'u'가 최대 한 번(0번 포함) 이상 반복되고 'r'이 이어지는 문자열 'color', 'colour'를 전역 검색한다.
const regExp = /colou?r/g;

target.match(regExp); // -> ["color", "colour"]
```

<br>

### 5.4. OR 검색

<br>

`|`은 or의 의미를 갖는다.

```jsx
const target = 'A AA B BB Aa Bb';

// 'A' 또는 'B'를 전역 검색한다.
const regExp = /A|B/g;

target.match(regExp); // -> ["A", "A", "A", "B", "B", "B", "A", "B"]
```

<br>

분해되지 않은 단어 레벨로 검색하기 위해서는 +를 같이 사용한다.

```jsx
const target = 'A AA B BB Aa Bb';

// 'A' 또는 'B'가 한 번 이상 반복되는 문자열을 전역 검색한다.
// 'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ...
const regExp = /A+|B+/g;

target.match(regExp); // -> ["A", "AA", "B", "BB", "A", "B"]
```

<br>

위 예제는 패턴을 or로 한 번 이상 반복하는 것인데 이를 간단히 표현하면 다음과 같다. 

`[]` 내의 문자는 or로 동작한다.

```jsx
const target = 'A AA B BB Aa Bb';

// 'A' 또는 'B'가 한 번 이상 반복되는 문자열을 전역 검색한다.
// 'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ...
const regExp = /[AB]+/g;

target.match(regExp); // -> ["A", "AA", "B", "BB", "A", "B"]
```

<br>

범위를 지정하려면 `[]` 내에 -를 사용한다.

```jsx
const target = 'A AA BB ZZ Aa Bb';

// 'A' ~ 'Z'가 한 번 이상 반복되는 문자열을 전역 검색한다.
// 'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ... ~ 또는 'Z', 'ZZ', 'ZZZ', ...
const regExp = /[A-Z]+/g;

target.match(regExp); // -> ["A", "AA", "BB", "ZZ", "A", "B"]
```

<br>

**대소문자를 구별하지 않고 알파벳을 검색하는 방법**

```jsx
const target = 'AA BB Aa Bb 12';

// 'A' ~ 'Z' 또는 'a' ~ 'z'가 한 번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /[A-Za-z]+/g;

target.match(regExp); // -> ["AA", "BB", "Aa", "Bb"]
```

<br>

**숫자를 검색하는 방법**

```jsx
const target = 'AA BB 12,345';

// '0' ~ '9'가 한 번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /[0-9]+/g;

target.match(regExp); // -> ["12", "345"]
```

<br>

```jsx
const target = 'AA BB 12,345';

// '0' ~ '9' 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /[0-9,]+/g;

target.match(regExp); // -> ["12,345"]
```

<br>

`\d`는 숫자를 의미한다. 즉, `\d`는 [0-9]와 같다.

`\D`는 숫자가 아닌 문자를 의미한다.

```jsx
const target = 'AA BB 12,345';

// '0' ~ '9' 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
let regExp = /[\d,]+/g;

target.match(regExp); // -> ["12,345"]

// '0' ~ '9'가 아닌 문자(숫자가 아닌 문자) 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
regExp = /[\D,]+/g;

target.match(regExp); // -> ["AA BB ", ","]
```

<br>

`\w`는 알파벳, 숫자, 언더스코어를 의미한다. 즉, `\w`는 `[A-Za-z0-9_]`와 같다. `\W`는 `\w`와 반대로 동작한다.

```jsx
const target = 'Aa Bb 12,345 _$%&';

// 알파벳, 숫자, 언더스코어, ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
let regExp = /[\w,]+/g;

target.match(regExp); // -> ["Aa", "Bb", "12,345", "_"]

// 알파벳, 숫자, 언더스코어가 아닌 문자 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
regExp = /[\W,]+/g;

target.match(regExp); // -> [" ", " ", ",", " $%&"]
```

<br>

### 5.5. NOT 검색

<br>

`[...]` 내의 `^`은 not의 의미를 갖는다.

```jsx
const target = 'AA BB Aa Bb 12';

// 숫자를 제외한 문자열을 전역 검색한다.
const regExp = /[^0-9]+/g;

target.match(regExp); // -> ["AA BB ", " Aa Bb"]
```

<br>

### 5.6. 시작 위치로 검색

<br>

[...] 밖의 ^은 문자열의 시작을 의미한다.

```jsx
const target = 'https://poiemaweb.com';

// 'https'로 시작하는지 검사한다.
const regExp = /^https/;

regExp.test(target); // -> true
```

<br>

### 5.7. 마지막 위치로 검색

<br>

`$`는 문자열의 마지막을 의미한다.

```jsx
const target = 'https://poiemaweb.com';

// 'com'으로 끝나는지 검사한다.
const regExp = /com$/;

regExp.test(target); // -> true
```

<br>

## 6. 자주 사용하는 정규표현식

<br>

### 6.1. 특정 단어로 시작하는지 검사

<br>

`[...]` 바깥의 `^`은 문자열의 시작을 의미한다.

?은 앞선 패턴이 최대 한 번(0번 포함) 이상 반복되는지를 의미한다.

```jsx
const url = 'https://example.com';

// 'http://' 또는 'https://'로 시작하는지 검사한다.
/^https?:\/\//.test(url); // -> true
```

<br>

똑같이 동작

```jsx
/^(http|https):\/\//.test(url); // -> true
```

<br>

### 6.2. 특정 단어로 끝나는지 검사

<br>

검색 대상 문자열이 ‘html’로 끝나는지 검사한다. $는 문자열의 마지막을 의미한다.

```jsx
const fileName = 'index.html';

// 'html'로 끝나는지 검사한다.
/html$/.test(fileName); // -> true
```

<br>

### 6.3. 숫자로만 이루어진 문자열인지 검사

<br>

검색 대상 문자열이 숫자로만 이루어진 문자열인지 검사한다.

[...] 바깥의 ^은 문자열의 시작을, $는 문자열의 마지막을 의미한다. \d는 숫자를 의미하고 +는 앞선 패턴이 최소 한 번 이상 반복되는 문자열을 의미한다.

```jsx
const target = '12345';

// 숫자로만 이루어진 문자열인지 검사한다.
/^\d+$/.test(target); // -> true
```

<br>

### 6.4. 하나 이상의 공백으로 시작하는지 검사

<br>

검색 대상 문자열이 하나 이상의 공백으로 시작하는지 검사한다.

`\s`는 여러 가지 공백 문자(스페이스, 탭 등)를 의미한다. 즉, `\s`는 `[\t\r\n\v\f]`와 같은 의미다.

```jsx
const target = ' Hi!';

// 하나 이상의 공백으로 시작하는지 검사한다.
/^[\s]+/.test(target); // -> true
```

<br>

### 6.5. 아이디로 사용 가능한지 검사

<br>

검색 대상 문자열이 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4 ~10자리인지 검사한다.

{4,10}은 최소 4번, 최대 10번 반복되는 문자열을 의미한다.

```jsx
const id = 'abc123';

// 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4 ~10자리인지 검사한다.
/^[A-Za-z0-9]{4,10}$/.test(id); // -> true
```

<br>

### 6.6. 메일 주소 형식에 맞는지 검사

<br>

검색 대상 문자열이 메일 주소 형식에 맞는지 검사한다.

```jsx
const email = 'ungmo2@gmail.com';

/^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.test(email); // -> true
```

<br>

### 6.7. 핸드폰 번호 형식에 맞는지 검사

<br>

검색 대상 문자열이 핸드폰 번호 형식에 맞는지 검사한다.

```jsx
const cellphone = '010-1234-5678';

/^\d{3}-\d{3,4}-\d{4}$/.test(cellphone); // -> true
```

<br>

### 6.8. 특수 문자 포함 여부 검사

<br>

검색 대상 문자열에 특수 문자가 포함되어 있는지 검사한다. 특수 문자는 A-Za-z0-9 이외의 문자다.

```jsx
const target = 'abc#123';

// A-Za-z0-9 이외의 문자가 있는지 검사한다.
(/[^A-Za-z0-9]/gi).test(target); // -> true
```
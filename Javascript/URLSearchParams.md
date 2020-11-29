# URLSearchParams

<br>

URLSearchParams 인터페이스는 URL의 쿼리 문자열에 대해 작업할 수 있는 유틸리티 메서드를 정의한다.

<br>

URL의 쿼리에 특정 키/값 을 추가, 삭제, 반환 등등을 할수있다.

<br>

## URLSearchParams()

<br>

URLSearchParams() 생성자는 새로운 URLSearchParams 객체를 생성하고 반환한다.

<br>

전역 생성자함수로 URLSearchParams  객체 인스턴스를 생성해준다.

```jsx
var URLSearchParams = new URLSearchParams(init);
```

<br>

예제

```jsx
// url 생성자에 전달된 주소를 url.search를 통해 params라는 변수로 검색합니다.
var url = new URL('https://example.com?foo=1&bar=2'); 
var params = new URLSearchParams(url.search);

// 문자열 리터럴을 전달합니다. 
var params2 = new URLSearchParams("foo=1&bar=2");
var params2a = new URLSearchParams("?foo=1&bar=2"); 

// 일련의 쌍으로 전달합니다.
var params3 = new URLSearchParams([["foo", "1"], ["bar", "2"]]);

// 레코드로 전달합니다.
var params4 = new URLSearchParams({"foo": "1", "bar": "2"});
```

<br>

URL.search는 URL의 쿼리를 쉽게 가져오게한다.

```jsx
const url = new URL('https://developer.mozilla.org/en-US/docs/Web/API/URL/search?q=123');
console.log(url.search); // Logs "?q=123"
```

<br>

## URLSearchParams.append()

append() 메서드는 구체적인 키와 값을 쿼리문자열에 넣어준다.

```jsx
URLSearchParams.append(name, value)
```

<br>

예제

```jsx
let url = new URL('https://example.com?foo=1&bar=2');
let params = new URLSearchParams(url.search.slice(1));

//Add a second foo parameter.
params.append('foo', 4);
//Query string is now: 'foo=1&bar=2&foo=4'
```

<br>

## URLSearchParams.delete()

<br>

URL의 특정 쿼리문자열에서 값을 지워준다.

지워주고싶은 키를 매개변수에 넣어준다.

<br>

중복되는 키가 있으면 모든 키와 값을 지워준다.

```jsx
URLSearchParams.delete(name)
```

<br>

예제

```jsx
let url = new URL('https://example.com?foo=1&bar=2&foo=3');
let params = new URLSearchParams(url.search);

// Delete the foo parameter.
params.delete('foo'); //Query string is now: 'bar=2'
```

<br>

### URLSearchParams.get()

<br>

주어진 검색 매개변수에 연결된 첫 번째 값을 반환한다.

원하는 키의 값을 가져오기위해 매개변수에 키를 넣어준다.

```jsx
URLSearchParams.get(name)
```

<br>

예제

```jsx
// https://example.com/?name=Jonathan&age=18
let params = new URLSearchParams(document.location.search.substring(1));
let name = params.get("name"); // is the string "Jonathan"
let age = parseInt(params.get("age"), 10); // is the number 18
let address = params.get("address"); // null
```

<br>

### URLSearchParams.getAll()

<br>

쿼리문자열에 동일한 키의 모든 값을 반환한다.

```jsx
URLSearchParams.getAll(name)
```

<br>

예제

```jsx
let url = new URL('https://example.com?foo=1&bar=2'); 
let params = new URLSearchParams(url.search.slice(1)); 

//Add a second foo parameter. 
params.append('foo', 4);

console.log(params.getAll('foo')) //Prints ["1","4"].
```
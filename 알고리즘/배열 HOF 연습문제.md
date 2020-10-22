# 배열 HOF 연습 문제

<br>

## 1. html 생성

<br>

아래 배열을 사용하여 html을 생성하는 함수를 작성하라.

<br>

```jsx
const todos = [
  { id: 3, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 1, content: 'Javascript', completed: false }
];

function render() {
  let html = '';

  todos.forEach(todo => {
html +=
`<li id="${todo.id}">
  <label><input type="checkbox"${todo.completed ? 'checked' : ''}>${todo.content}</label>
</li> \n`
  });

  return html;
}

console.log(render());
/*
<li id="3">
  <label><input type="checkbox">HTML</label>
</li>
<li id="2">
  <label><input type="checkbox" checked>CSS</label>
</li>
<li id="1">
  <label><input type="checkbox">Javascript</label>
</li>
*/
```

<br>

## 2. 특정 프로퍼티 값 추출

<br>

요소의 프로퍼티(id, content, completed)를 문자열 인수로 전달하면 todos의 각 요소 중, 해당 프로퍼티의 값만을 추출한 배열을 반환하는 함수를 작성하라.

<br>

단, for 문이나 Array#forEach는 사용하지 않도록 하자.

<br>

```jsx
const todos = [
  { id: 3, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 1, content: 'Javascript', completed: false }
];

function getValues(key) {
  return todos.map(todo => todo[key]);
}

console.log(getValues('id')); // [3, 2, 1]
console.log(getValues('content')); // ['HTML', 'CSS', 'Javascript']
console.log(getValues('completed')); // [false, true, false]
```

<br>

## 3. 프로퍼티 정렬

<br>

요소의 프로퍼티(id, content, completed)를 문자열 인수로 전달하면 todos의 요소를 정렬하는 함수를 작성하라.

<br>

단, todos는 변경되지 않도록 하자.

<br>

참고: [Array.prototype.sort](https://poiemaweb.com/fastcampus/array#91-arrayprototypesort)

<br>

```jsx
const todos = [
  { id: 3, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 1, content: 'Javascript', completed: false }
];

function sortBy(key) {
  return todos.sort((todo1, todo2) => todo1[key] > todo2[key]? 1 : (todo1[key] < todo2[key] ? -1 : 0));
}

console.log(sortBy('id'));
/*
[
  { id: 1, content: 'Javascript', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 3, content: 'HTML', completed: false }
]
*/
console.log(sortBy('content'));
/*
[
  { id: 2, content: 'CSS', completed: true },
  { id: 3, content: 'HTML', completed: false },
  { id: 1, content: 'Javascript', completed: false }
]
*/
console.log(sortBy('completed'));
/*
[
  { id: 3, content: 'HTML', completed: false },
  { id: 1, content: 'Javascript', completed: false },
  { id: 2, content: 'CSS', completed: true }
]
*/
```

<br>

## 4. 새로운 요소 추가

<br>

새로운 요소(예를 들어 { id: 4, content: 'Test', completed: false })를 인수로 전달하면 todos의 선두에 새로운 요소를 추가하는 함수를 작성하라. 단, Array#push는 사용하지 않도록 하자.

<br>

```jsx
let todos = [
  { id: 3, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 1, content: 'Javascript', completed: false }
];

function addTodo(newTodo) {
  todos = [newTodo, ...todos];
}

addTodo({ id: 4, content: 'Test', completed: false });

console.log(todos);
/*
[
  { id: 4, content: 'Test', completed: false },
  { id: 3, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 1, content: 'Javascript', completed: false }
]
*/
```

<br>

## 5. 특정 요소 삭제

<br>

todos에서 삭제할 요소의 id를 수로 전달하면 해당 요소를 삭제하는 함수를 작성하라.

<br>

```jsx
let todos = [
  { id: 3, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 1, content: 'Javascript', completed: false }
];

function removeTodo(id) {
  todos = todos.filter(todo => todo.id !== 2);
}

removeTodo(2);

console.log(todos);
/*
[
  { id: 3, content: 'HTML', completed: false },
  { id: 1, content: 'Javascript', completed: false }
]
*/
```

<br>

## 6. 특정 요소의 프로퍼티 값 반전

<br>

todos에서 대상 요소의 id를 인수로 전달하면 해당 요소의 completed 프로퍼티 값을 반전하는 함수를 작성하라.

<br>

hint) 기존 객체의 특정 프로퍼티를 변경/추가하여 새로운 객체를 생성하려면 [Object.assign](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) 또는 [스프레드 문법](https://poiemaweb.com/fastcampus/spread-syntax#3-%EA%B0%9D%EC%B2%B4-%EB%A6%AC%ED%84%B0%EB%9F%B4-%EB%82%B4%EB%B6%80%EC%97%90%EC%84%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EA%B2%BD%EC%9A%B0)을 사용한다.

<br>

```jsx
let todos = [
  { id: 3, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 1, content: 'Javascript', completed: false }
];

function toggleCompletedById(id) {
  todos.forEach((todo, i)  => {
    todo.id === id ? todos[i].completed = !todo.completed : '';
  });
}

toggleCompletedById(1);

console.log(todos);
/*
[
  { id: 3, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: false },
  { id: 1, content: 'Javascript', completed: false }
]
*/
```

<br>

## 7. 모든 요소의 completed 프로퍼티 값을 true로 설정

<br>

todos의 모든 요소의 completed 프로퍼티 값을 true로 설정하는 함수를 작성하라.

<br>

hint) 기존 객체의 특정 프로퍼티를 변경/추가하여 새로운 객체를 생성하려면 [Object.assign](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) 또는 [스프레드 문법](https://poiemaweb.com/fastcampus/spread-syntax#3-%EA%B0%9D%EC%B2%B4-%EB%A6%AC%ED%84%B0%EB%9F%B4-%EB%82%B4%EB%B6%80%EC%97%90%EC%84%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EA%B2%BD%EC%9A%B0)을 사용한다.

<br>

```jsx
let todos = [
  { id: 3, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 1, content: 'Javascript', completed: false }
];

function toggleCompletedAll() {
  todos.forEach((todo, i , todos)=> {
    todo.completed ? '' : todos[i].completed = true;
  });
}

toggleCompletedAll();

console.log(todos);
/*
[
  { id: 3, content: 'HTML', completed: true },
  { id: 2, content: 'CSS', completed: true },
  { id: 1, content: 'Javascript', completed: true }
]
*/
```

<br>

## 8. completed 프로퍼티의 값이 true인 요소의 갯수 구하기

<br>

todos에서 완료(completed: true)한 할일의 갯수를 구하는 함수를 작성하라.

<br>

단, for 문, Array#forEach는 사용하지 않도록 하자.

<br>

```jsx
let todos = [
  { id: 3, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 1, content: 'Javascript', completed: false }
];

function countCompletedTodos() {
  return todos.filter(todo => todo.completed).length;
}

console.log(countCompletedTodos()); // 1
```

<br>

## 9. id 프로퍼티의 값 중에서 최대값 구하기

<br>

todos의 id 프로퍼티의 값 중에서 최대값을 구하는 함수를 작성하라.

<br>

단, for 문, Array#forEach는 사용하지 않도록 하자.

<br>

```jsx
let todos = [
  { id: 3, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 1, content: 'Javascript', completed: false }
];

function getMaxId() {
  return todos.reduce((acc, cur) => acc > cur.id ? acc : cur.id, 0)
}

console.log(getMaxId()); // 3
```
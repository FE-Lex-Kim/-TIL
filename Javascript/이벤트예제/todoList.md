# TodoList

## Javascript
```js
// State
let todos = [];

const $todos = document.querySelector('.todos');
const $inputTodo = document.querySelector('.input-todo');
const $completeAll = document.querySelector('.complete-all');
const $completedTodos = document.querySelector('.completed-todos');
const $activeTodos = document.querySelector('.active-todos');
const $active = document.getElementById('active');
const $completed = document.getElementById('completed');
const $all = document.getElementById('all');
const $btn = document.querySelector('.btn');
const $nav = document.querySelector('.nav');

// completed 현황판
const changeCompletedNow = () => {
  $completedTodos.textContent = `${todos.filter(todo => todo.completed).length}`;
  $activeTodos.textContent = `${todos.filter(todo => !todo.completed).length}`;
};

const render = (tap = todos) => {
  $todos.innerHTML = tap.map(({ id, content, completed }) => `
  <li id="${id}" class="todo-item">
    <input id="ck-${id}" class="checkbox" ${completed ? 'checked' : ''} type="checkbox">
    <label for="ck-${id}">${content}</label>
    <i class="remove-todo far fa-times-circle"></i>
  </li>`).join('');
  changeCompletedNow();
};

const fetchTodos = () => {
  todos = [
    { id: 1, content: 'Javascript', completed: false },
    { id: 2, content: 'HTML', completed: false },
    { id: 3, content: 'CSS', completed: true }
  ];

  render();
};

const addTodo = () => {
  todos = [{ id: Math.max(...todos.map(todo => todo.id)) + 1, content: $inputTodo.value, completed: false }, ...todos];
  render();
  $inputTodo.value = '';
};

window.onload = fetchTodos;


const chekckAcitveCompleted = () => {
  if ($active.classList.contains('active')) {
    active();
  } else if ($completed.classList.contains('active')) {
    complete();
  }
};

// 인풋창에 투두 추가
$inputTodo.onkeypress = e => {
  if (e.key !== 'Enter' || $inputTodo.value === '') return;
  addTodo();
  chekckAcitveCompleted();
};

// 실시간 체크시 acitve 바꾸기
const active = () => {
  const actived = todos.filter(todo => !todo.completed);
  render(actived);
};

// 실시간 체크시 completed 바꾸기
const complete = () => {
  const completed = todos.filter(todo => todo.completed);
  render(completed);
};

// 체크박스 클릭이벤트
$todos.onchange = e => {
  todos = todos.map(todo => todo.id === +e.target.parentNode.id ? {...todo, completed : !todo.completed} : todo);
  render();
  chekckAcitveCompleted();
};

// 전체 체크박스 클릭 이벤트
$completeAll.onchange = e => {
  todos = todos.map(todo => e.target.checked ? { ...todo, completed : true } : { ...todo, completed : false });
  render();
  chekckAcitveCompleted();
};

// 삭제이벤트
$todos.onclick = e => {
  if (e.target.classList.contains('remove-todo')) {
    todos = todos.filter(todo => todo.id !== +e.target.parentNode.id);
    render();
  }
};

const changeBackgroundColor = (e) => [...$nav.children].forEach(todo => {
  todo.classList.toggle('active', todo.id === e.target.id);
});

// 탭리스트
$active.onclick = (e) => {
  active();
  // console.log(active);
  changeBackgroundColor(e);
};

$completed.onclick = (e) => {
  complete();
  changeBackgroundColor(e);
};

$all.onclick = (e) => {
  e.target.classList.add('active');
  changeBackgroundColor(e);
  render();
};

// 체크 된요소들 삭제
$btn.onclick = () => {
  todos = todos.filter(todo => !todo.completed);
  render();
};

## CSS

```css
@import url('https://fonts.googleapis.com/css?family=Roboto:100,300,400,700|Noto+Sans+KR');
@import url('https://use.fontawesome.com/releases/v5.5.0/css/all.css');

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Roboto', 'Noto Sans KR', sans-serif;
  font-size: 0.9em;
  color: #58666e;
  background-color: #f0f3f4;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.container {
  max-width: 750px;
  min-width: 450px;
  margin: 0 auto;
  padding: 15px;
}

.title {
  /* margin: 10px 0; */
  font-size: 4.5em;
  font-weight: 100;
  text-align: center;
  color: #23b7e5;
}

.ver {
  font-weight: 100;
  text-align: center;
  color: #23b7e5;
  margin-bottom: 30px;
}

/* .input-todo  */
.input-todo {
  display: block;
  width: 100%;
  height: 45px;
  padding: 10px 16px;
  font-size: 18px;
  line-height: 1.3333333;
  color: #555;
  border: 1px solid #ccc;
  border-color: #e7ecee;
  border-radius: 6px;
  outline: none;
  transition: border-color ease-in-out .15s,box-shadow ease-in-out .15s;
}

.input-todo:focus {
  border-color: #23b7e5;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(102, 175, 233, 0.6);
}

.input-todo::-webkit-input-placeholder {
  color: #999;
}

/* .nav */
.nav {
  display: flex;
  margin: 15px;
  list-style: none;
}

.nav > li {
  padding: 4px 10px;
  border-radius: 4px;
  cursor: pointer;
}

.nav > li.active {
  color: #fff;
  background-color: #23b7e5;
}

.todos {
  margin-top: 20px;
}

/* .todo-item */
.todo-item {
  position: relative;
  /* display: block; */
  height: 50px;
  padding: 10px 15px;
  margin-bottom: -1px;
  background-color: #fff;
  border: 1px solid #ddd;
  border-color: #e7ecee;
  list-style: none;
}

.todo-item:first-child {
  border-top-left-radius: 4px;
  border-top-right-radius: 4px;
}

.todo-item:last-child {
  border-bottom-left-radius: 4px;
  border-bottom-right-radius: 4px;
}

/*
  .checkbox
  .checkbox 바로 뒤에 위치한 label의 before와 after를 사용해
  .checkbox의 외부 박스와 내부 박스를 생성한다.

  <input class="checkbox" type="checkbox" id="myId">
  <label for="myId">Content</label>
*/

.checkbox {
  display: none;
}

.checkbox + label {
  position: absolute; /* 부모 위치를 기준으로 */
  top: 50%;
  left: 15px;
  transform: translate3d(0, -50%, 0);
  display: inline-block;
  width: 90%;
  line-height: 2em;
  padding-left: 35px;
  cursor: pointer;
  user-select: none;
}

.checkbox + label:before {
  content: "";
  position: absolute;
  top: 50%;
  left: 0;
  transform: translate3d(0, -50%, 0);
  width: 20px;
  height: 20px;
  background-color: #fff;
  border: 1px solid #cfdadd;
}

.checkbox:checked + label:after {
  content: "";
  position: absolute;
  top: 50%;
  left: 6px;
  transform: translate3d(0, -50%, 0);
  width: 10px;
  height: 10px;
  background-color: #23b7e5;
}

/* .remove-todo button */
.remove-todo {
  display: none;
  position: absolute;
  top: 50%;
  right: 10px;
  cursor: pointer;
  transform: translate3d(0, -50%, 0);
}

/* todo-item이 호버 상태이면 삭제 버튼을 활성화 */
.todo-item:hover > .remove-todo {
  display: block;
}

footer {
  display: flex;
  justify-content: space-between;
  margin: 20px 0;
}

.complete-all, .clear-completed {
  position: relative;
  flex-basis: 50%;
}

.clear-completed {
  text-align: right;
  padding-right: 15px;
}

.btn {
  padding: 1px 5px;
  font-size: .8em;
  line-height: 1.5;
  border-radius: 3px;
  outline: none;
  color: #333;
  background-color: #fff;
  border-color: #ccc;
  cursor: pointer;
}

.btn:hover {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}

```

## HTML
---
```
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Todos 3.0</title>
  <link href="css/style.css" rel="stylesheet">
  <script defer src="js/app.js"></script>
</head>
<body>
  <div class="container">
    <h1 class="title">Todos</h1>
    <div class="ver">3.0</div>

    <input class="input-todo" placeholder="What needs to be done?" autofocus>
    <ul class="nav">
      <li id="all" class="active">All</li>
      <li id="active">Active</li>
      <li id="completed">Completed</li>
    </ul>
    <ul class="todos">
      <!-- <li id="myId" class="todo-item">
        <input id="ck-myId" class="checkbox" type="checkbox">
        <label for="ck-myId">HTML</label>
        <i class="remove-todo far fa-times-circle"></i>
      </li> -->
    </ul>
    <footer>
      <div class="complete-all">
        <input class="checkbox" type="checkbox" id="ck-complete-all">
        <label for="ck-complete-all">Mark all as complete</label>
      </div>
      <div class="clear-completed">
        <button class="btn">Clear completed (<span class="completed-todos">0</span>)</button>
        <strong class="active-todos">0</strong> items left
      </div>
    </footer>
  </div>
</body>
</html>

```

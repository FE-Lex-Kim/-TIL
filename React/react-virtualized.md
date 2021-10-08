# react-virtualized

<br>

컴포넌트에서 스크롤되기전에 보이지 앟는 컴포넌트는 렌더링 하지 않고 크기만 차지 하게끔 할 수 있다.

스크롤 되면 해당 컴포넌트는 자연스럽게 렌더링 시켜준다.

<br>

**설치방법**

```bash
npm i react-virtualized
```

<br>

예제 코드)

```jsx
import React, { useCallback } from "react";
import TodoListItem from "./TodoListItem";
import { List } from "react-virtualized";
import "./TodoList.scss";

const TodoList = ({ todos, onRemove, modify }) => {
  const rowRenderer = useCallback(
    ({ index, key, style }) => {
      const todo = todos[index];
      return (
        <TodoListItem
          todo={todo}
          key={key}
          onRemove={onRemove}
          onToggle={modify}
          style={style}
        />
      );
    },
    [onRemove, modify, todos]
  );
  return (
    <List
      className="TodoList"
      width={512} // 전체 크기
      height={513} // 전체 높이
      rowCount={todos.length} // 항목 개수
      rowHeight={57} // 항목 높이
      rowRenderer={rowRenderer} // 항목을 렌더링할 때 쓰는 함수
      list={todos} // 배열
      style={{ outline: "none" }} // List에 기본 적용되는 outline 스타일 제거
    />
  );
};

export default React.memo(TodoList);
```

<br>

List 컴포넌트를 사용하기위해 rowRenderer라는 함수를 작성해준다.

이함수는 react-virtualized의 List컴포넌트에서 TodoItem을 렌더링 할 때 사용한다.

<br>

이함수를 List 컴포넌트의 props로 전달해주어야한다.

이함수의 파라미터에는 index, key, style값을 객체 타입으로 받아와서 사용한다.

<br>

**반드시!**

List 컴포넌트를 사용할떄는

1. 해당 리스트의 전체 크기
2. 해당 항목의 높이
3. 각 항목을 렌더링할때 사용하는 함수
4. 렌더링하는 상태의 배열을 props로 넣어주어야한다.

<br>

이컴포넌트가 전달받은 props를 통해 자동으로 최적화 해준다.

<br>

**각 항목에 해당하는 컴포넌트는 div로 한번 감싸주고 그 div에 props로 받아온 style을 넣어주어야한다.**

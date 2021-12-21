- [React Query(8) - Optimistic Updates, Axios Interceptor](#react-query8---optimistic-updates-axios-interceptor)
  - [Optimistic Updates](#optimistic-updates)

<br>

# React Query(8) - Optimistic Updates, Axios Interceptor

## Optimistic Updates

**서버의 데이터를 변경하기전에, 화면을 먼저 업데이트 한다.**

**네트워크 요청, 응답 시간을 기다리지 않아, 사용자 경험이 더 나아진다.**

<br>

**만약 데이터 요청이 실패하더라도, 요청하기전 화면으로 되돌린다.**

<br>

어떻게 구현하는지 코드를 보고 알아보자.

```jsx
const queryClient = useQueryClient();

useMutation(updateTodo, {
  // mutate가 호출되었을때, onMutate 메서드가 실행된다.
  // onMutate는 데이터 호출하기전에 실행된다.
  // 현재 코드에 들어간 인자인 newTodo는 mutate를 호출할때 들어가는 인수 값이다.
  onMutate: async (newTodo) => {
    // queryClient.cancelQueries는 비동기적으로 실행되므로,
    // aysnc, await으로 동기적으로 실행시켜준다.
    // queryClient.cancelQueries은 'todos' query에 발생하는 refetch를 취소시켜준다.
    // setQueryData을 할때, refetch를 취소시켜준다.
    await queryClient.cancelQueries("todos");

    // 새로운 데이터를 추가하기전에, 현재 query를 변수에 저장한다.
    const previousTodos = queryClient.getQueryData("todos");

    // 'todos' query에 새로운 데이터를 추가시켜준다.
    queryClient.setQueryData("todos", (oldData) => {
      return {
        ...oldData,
        // id 값은 반드시, uuid 패키지를 사용해 랜덤 id를 넣어주도록 하자.
        data: [...oldData.data, { id: oldData?.data.length + 1, ...newData }],
      };
    });

    // 새로운 데이터를 추가하기전, 현재 query를 반환해준다.
    // error 발생시, 이 변수를 사용해서 다시 업데이트된 화면을 롤백 시켜준다.
    return { previousTodos };
  },

  // 만약 mutation이 실패했다면, onMutate에 반환된 이전 query를 사용해서 롤백 시켜준다.
  onError: (err, newTodo, context) => {
    // 두 번째 인수를 업데이트 함수를 전달하지 않으면, 해당 값으로 업데이트한다.
    queryClient.setQueryData("todos", context.previousTodos);
  },

  // Error, Success에 상관없이 항상 refetch 시킨다.
  onSettled: () => {
    queryClient.invalidateQueries("todos");
  },
});
```

<br>

참고

- [https://react-query.tanstack.com/guides/optimistic-updates](https://react-query.tanstack.com/guides/optimistic-updates)
- [https://www.youtube.com/watch?v=rnN5ng6aoAc&list=PLC3y8-rFHvwjTELCrPrcZlo6blLBUspd2&index=24](https://www.youtube.com/watch?v=rnN5ng6aoAc&list=PLC3y8-rFHvwjTELCrPrcZlo6blLBUspd2&index=24)

- [React Query(8) - Handling Mutation Response, Optimistic Updates](#react-query8---handling-mutation-response-optimistic-updates)
  - [Handling Mutation Response](#handling-mutation-response)
  - [Optimistic Updates](#optimistic-updates)

<br>

# React Query(8) - Handling Mutation Response, Optimistic Updates

- 서버의 데이터를 추가, 삭제, 수정을 하고 효율적으로 화면을 업데이트 하는 2가지 방식에 대해 알아보자.

<br>

## Handling Mutation Response

<br>

**서버 데이터를 추가, 삭제, 수정을 할때**, refetch를 하는 방법으로 화면의 보여지는 데이터를 수정했다.

즉, refetch를 통해 query를 새롭게 업데이트 시켜줬다.

<br>

**하지만, 일반적으로 이런 경우에는 서버로 부터 응답을 받을때, 변경된 새로운 서버 데이터를 같이 반환해준다.**

네트워크 요청을 응답 받을때, 이미 새로운 데이터를 가져왔으므로, 이후 **불필요한 네트워크 요청을 할 필요가 없다.**

**이런 경우를 위한 기능으로 `queryClient.setQueryData`을 통해 응답받은 즉시 query 업데이트를 해줄 수 있다.**

<br>

**`queryClient.setQueryData` 는 원하는 query의 data를 추가하거나, 수정할 수 있다.**

**응답 받은 데이터를 `queryClient.setQueryData` 를 사용해서 데이터를 추가해, 즉시 query를 업데이트 시킨다.**

```jsx
const { data } = useQuery("colors", fetchFavoriteColor);
const { mutate, isLoading, isError, error } = useMutation(addFavoriteColor, {
  onSuccess: (newData) => {
    queryClient.setQueryData("colors", (oldQueryData) => {
      return {
        ...oldQueryData,
        data: [...oldQueryData.data, newData.data],
      };
    });
  },
});

mutate(color);
```

<br>

**onSuccess 메서드 인자로 새롭게 응답받은 서버 데이터를 받을 수 있다.**

```jsx
onSuccess: (newData) => {
  console.log(newData);
};
```

<Image alt="Handling Mutation Response" src="../Images/React%20Query(7)/React%20Query(7)-8.png" width=500 />

<br>

**`queryClient.setQueryData` 는 두 개의 인수를 받는다.**

1. 첫 번째 인수는 **queryKey를 넣어준다.**
   - **queryKey에 해당하는 query가 변경된다.**
     ```jsx
     queryClient.setQueryData("colors", ()=> ...)
     ```
2. 두 번째 인수는 **query data를 변경하는 함수를 전달한다.**

   - **함수가 반환한 값이, 첫 번째 인수로 전달된 query의 값으로 변경된다.**
   - **해당 함수는 아직 변경되기전, query 값이 인자로 들어간다.**

   ```jsx
   (oldQueryData) => {
       return {
         ...oldQueryData,
         data: [...oldQueryData.data, newData.data], // 새롭게 받은 데이터를 추가해준다.
       };
   });
   ```

  <Image alt="Handling Mutation Response" src="../Images/React%20Query(7)/React%20Query(7)-9.png" width=500 />

<br>

이제 데이터를 응답 받은후, 불필요한 네트워크 요청 없이 바로 query를 수정해 화면을 업데이트 할 수 있다.

<br>

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

- [https://react-query.tanstack.com/guides/updates-from-mutation-responses](https://react-query.tanstack.com/guides/updates-from-mutation-responses)
- [https://react-query.tanstack.com/guides/optimistic-updates](https://react-query.tanstack.com/guides/optimistic-updates)
- [https://www.youtube.com/watch?v=rnN5ng6aoAc&list=PLC3y8-rFHvwjTELCrPrcZlo6blLBUspd2&index=24](https://www.youtube.com/watch?v=rnN5ng6aoAc&list=PLC3y8-rFHvwjTELCrPrcZlo6blLBUspd2&index=24)

# Jest 비동기 코드 테스트

Jest는 테스트 중인 코드가 완료된 시점을 알아야 다른 테스트로 넘어갈 수 있다.

비동기 코드를 테스트 하는 방법은 3가지가 있다.

1. 콜백
2. promise
3. async / await

<br>

## 콜백

fetchData가 콜백함수를 호출해서 데이터를 불러오는 함수라고 하자.

이때 일반적인 테스팅을 하면 동작하지 않는다.

```jsx
// Don't do this!
test("the data is peanut butter", () => {
  function callback(data) {
    expect(data).toBe("peanut butter");
  }

  fetchData(callback);
});
```

<br>

```jsx
test("the data is peanut butter", (done) => {
  function callback(data) {
    try {
      expect(data).toBe("peanut butter");
      done();
    } catch (error) {
      done(error);
    }
  }

  fetchData(callback);
});
```

`done` 파라미터를 받아준다.

Jest는 테스트를 완료하기전에 `done` 콜백함수가 호출될때까지 기다리다려준다.

<br>

## Promise

마찬가지로 일반적인 Promise를 사용한 테스트를 하면 동작하지 않는다.

Promise의 비동기 코드 테스트 방법은 2가지이다.

<br>

### 내부에서 테스트

첫번째는 `then` 메서드의 내부에서 해당 data에 대한 테스트를 진행하고 그에대한 값을 `return` 문에 반환하면 된다.

```jsx
test("the data is peanut butter", () => {
  return fetchData().then((data) => {
    expect(data).toBe("peanut butter");
  });
});
```

<br>

마찬가지로 `catch` 메서드 내부에서도 테스트를 진행하고 반드시 `return`문에 반환해야한다.

```jsx
test("the fetch fails with an error", () => {
  expect.assertions(1);
  return fetchData().catch((e) => expect(e).toMatch("error"));
});
```

<br>

### 외부에서 테스트

두번째는 `.resolves` 또는 `.rejects`비교 메서드를 사용하면된다.

promise가 처리될때까지 기다리다가 그에 대한 값을 비교 메서드로 테스트를 진행한다.

만약 promise가 rejected된다면 테스트는 자동적으로 실패하게 된다.

```jsx
test("the data is peanut butter", () => {
  return expect(fetchData()).resolves.toBe("peanut butter");
});
```

<br>

`.rejescts` 메서드는 만약 promise가 rejected 될것을 예상하는 테스트일 경우에 사용한다.

```jsx
test("the fetch fails with an error", () => {
  return expect(fetchData()).rejects.toMatch("error");
});
```

<br>

## Async/Await

`Async/Await` 키워드를 사용하는것은 굉장히 심플하다.

기존에 `Async/Await`를 사용하면 동기적으로 처리하게 되어있다.

<br>

따라서 해당 키워드를 사용한후 테스트를 진행하면된다.

```jsx
test("the data is peanut butter", async () => {
  const data = await fetchData();
  expect(data).toBe("peanut butter");
});

test("the fetch fails with an error", async () => {
  expect.assertions(1);
  try {
    await fetchData();
  } catch (e) {
    expect(e).toMatch("error");
  }
});
```

<br>

참고

- [https://jestjs.io/docs/getting-started](https://jestjs.io/docs/getting-started)
- [https://jestjs.io/docs/using-matchers](https://jestjs.io/docs/using-matchers)
- 패스트캠퍼스 한 번에 끝내는 프론트엔드 개발 초격자 패키지 online 강의

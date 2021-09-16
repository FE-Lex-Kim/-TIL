# 테스트 도구 Jest

<br>

사람이 자신의 지식을 바탕으로 프로그래밍을 하다보면 의도치 않게 실수로 인해 에러가 나거나 올바르지 않은 방향의 프로그래밍을 할 수 도 있다.

<br>

사람의 감으로 코딩을 하는 것이 아닌 테스트 코드를 통해서 코딩을 하면 명확하고 정확하게 진행할 수 있다.

또한 테스트 코드는 그 자체로 명세가 되어서 어떻게 동작하는지에 대해 설명하기 수월해진다.

<br>

테스트 도구중에 Facebook이 개발한 테스트 도구 Jest를 사용해보자.

<br>

## 환경설정

설치 방법(npm 기준)

```bash
npm install -D jest
```

당연히 테스트 도구이므로 개발 전용에 저장해준다.

<br>

package.json에 script의 test 프로퍼티 값을 `jest`로 변경해준다.

```bash
{
  "scripts": {
    "test": "jest"
  }
}
```

환경설정 끝.

이제 사용법에대해 알아보자

<br>

## 사용하기

테스트를 진행하는 파일을 만들어주어야한다.

파일 네이밍은 [파일이름].test.js 라고 만들어준다.

<br>

example.test.js

```jsx
function sum(a, b) {
  return a + b;
}

test("sum 1 + 2 to equal 3", () => {
  expect(sum(1, 2)).toBe(3);
});
```

test함수의 첫번째 인수는 해당 테스트에 대한 설명을 적어준다.

두번째 인수는 콜백함수안에 테스트할 값을 넣어준다.

<br>

콜백함수 내부에서 expect 함수를 사용해서 테스트할 값을 인수로 전달한다.

이후 마침표 연산자와 jest에서 제공하는 비교 메서드 내부에 결과값을 넣어 사용하면 비교해준다.

ex) toBe() : 같은지 비교

<br>

[다양한 비교 메서드 알아보기](https://jestjs.io/docs/expect)

<br>

이후 실행하는 방법은 `npm run test` 을 적어주면된다.

출력창에 다음과 같은 내용이 나온다.

```jsx
PASS ./example.test.js
  ✓ sum 1 + 2 to equal 3 (1 ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        0.128 s, estimated 1 s
Ran all test suites.
```

<br>

만약 테스트 값이 틀리다면

```jsx
FAIL ./example.test.js
  ✕ sum 1 + 2 to equal 3 (1 ms)

  ● sum 1 + 2 to equal 3

    expect(received).toBe(expected) // Object.is equality

    Expected: 4
    Received: 3

      4 |
      5 | test("sum 1 + 2 to equal 3", () => {
    > 6 |   expect(sum(1, 2)).toBe(4);
        |                     ^
      7 | });
      8 |

      at Object.<anonymous> (example.test.js:6:21)

Test Suites: 1 failed, 1 total
Tests:       1 failed, 1 total
Snapshots:   0 total
Time:        0.212 s, estimated 1 s
Ran all test suites.
```

<br>

Expected 는 테스트에서 원하는 결과값이다.

Recevied는 테스트를 진행해서 나온 결과 값이다.

<br>

### describe

describe는 `test` 들을 묶어서 어떠한 테스트인지에 대해 설명하는 메서드이다.

<br>

또한 `test`에 대한 alias인 `it`을 제공해준다.

```jsx
function sum(a, b) {
  return a + b;
}

describe("expect test", () => {
  it("sum 1 + 2 to equal 3", () => {
    expect(sum(1, 2)).toBe(3);
  });

  it("object assignment", () => {
    const data = { one: 1 };
    data["two"] = 2;
    expect(data).toEqual({ one: 1, two: 2 });
  });

  it("toHaveLength", () => {
    expect("Hi galaxy").toHaveLength(9);
  });
});
```

위의 예시에서 두번째 예제 코드에서 `toBe`를 사용하게 되었을때, 객체끼리 비교하게 된다면 참조값을 비교하므로 같지 않다고 나온다.

<br>

따라서 객체끼리 비교한다면 `toEqual`을 사용해서 내부의 모든 값을 재귀적으로 탐색해 확인한다.

<br>

참고

- [https://jestjs.io/docs/getting-started](https://jestjs.io/docs/getting-started)
- [https://jestjs.io/docs/using-matchers](https://jestjs.io/docs/using-matchers)
- 패스트캠퍼스 한 번에 끝내는 프론트엔드 개발 초격자 패키지 online 강의

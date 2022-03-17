# 함수형 컴포넌트 Error boundaries

클래스형 컴포넌트에서는 Error boundaries이 있어서 에러가 발생시 에러 페이지를 보여준다.

React는 함수형 컴포넌트에서는 아직 Error boundaries를 만들지 않았다고 한다.

<br>

그래서 함수형 컴포넌트에서는 \***\*react-error-boundary 라이브러리를 사용해서 에러 페이지를 보여줄 수 있다.\*\***

```bash
npm i react-error-boundary
```

<br>

사용

```jsx
import { ErrorBoundary } from "react-error-boundary";

function ErrorFallback({ error, resetErrorBoundary }) {
  return (
    <div role="alert">
      <p>Something went wrong:</p>
      <pre>{error.message}</pre>
      <button onClick={resetErrorBoundary}>Try again</button>
    </div>
  );
}

const ui = (
  <ErrorBoundary
    FallbackComponent={ErrorFallback}
    onReset={() => {
      // reset the state of your app so the error doesn't happen again
    }}
  >
    <ComponentThatMayError />
  </ErrorBoundary>
);
```

<br>

참고

- [https://dev.to/edemagbenyo/handle-errors-in-react-components-like-a-pro-l7l](https://dev.to/edemagbenyo/handle-errors-in-react-components-like-a-pro-l7l)
- [https://www.npmjs.com/package/react-error-boundary](https://www.npmjs.com/package/react-error-boundary)

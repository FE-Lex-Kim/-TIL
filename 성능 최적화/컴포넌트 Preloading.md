# 컴포넌트 Preloading

code splitting을 통해 다른 페이지를 들어갈때, 해당 페이지를 그 시점에 로드 한다.

최초 렌더링 속도는 향상되겠지만, **다른 페이지로 넘어갈때, 로드하는 시간이 추가적으로 걸린다는 점이 있다.**

<br>

다른 페이지로 넘어갈때, 실행 프로세스

1. 페이지 로드(네트워크)
2. 로드한 페이지의 JS Evaluate(메인 스레드)
3. JS 실행(메인 스레드)

<br>

1,2번을 실행하는 시간이 걸리는 속도를 줄이기 위해선 어떻게 해야할까?

다른 페이지로 넘어가기 전 즉, 3번이 되기전에 Preloading(미리 로드)을 하면된다.

<br>

## Preloading 타이밍

Preloading을 언제 할지 생각해야한다.

<br>

보통 두 가지 경우때 Preloading 한다.

1. 다른 페이지로 넘어가는 **버튼 호버(hover)시**
2. 최초 로드이후, **모든 컴포넌트 마운트 끝난뒤**

<br>

### 1. 마우스 호버시

`onMouseEnter` 이벤트를 통해 preloading을 실행한다.

다른 페이지로 넘어가기 위해, **버튼에 호버한뒤 클릭하기 까지 걸리는 시간이 보통 0.2 ~ 0.5초 걸린다고 한다.**

<br>

0.2 ~ 0.5초 정도는 페이지를 로드하기 까지 **큰 도움이 되는 시간이다.**

<br>

```jsx
**const LazyImageModal = lazy(() => import("./components/ImageModal"));**

function App() {
  const [showModal, setShowModal] = useState(false);

  **function handleMouseEnter(params) { // <---------------------- 1.
    const temp = import("./components/ImageModal");
  }**

  return (
    <div className="App">
      <ButtonModal
        onClick={() => {
          setShowModal(true);
        }}
        **onMouseEnter={handleMouseEnter} // <---------------------- 1.**
      >
        올림픽 사진 보기
      </ButtonModal>
      <Suspense fallback={null}>
        {showModal ? (
          **<LazyImageModal // <---------------------- 2.
            closeModal={() => {
              setShowModal(false);
            }}
          />**
        ) : null}
      </Suspense>
    </div>
  );
}
```

<br>

프로세스

1. 버튼 호버시 동적 import를 해준다.
2. LazyImageModal이 preload되어서 바로 나타난다.

<br>

만약 페이지의 사이즈가 커서 로드하는데 **1초이상 걸리면 어떻게 해야할까?**

마찬가지로 버튼을 호버할때, 로드한다고 해도 시간이 **꾀걸려서 사용감이 떨어질 것이다.**

<br>

따라서 호버시가 아니라 최초 로드 이후, **모든 컴포넌트 마운트 이후에 Preloading을 해주면 좋다.**

<br>

### 2. 최초 로드이후, 모든 컴포넌트 마운트 이후

모든 컴포넌트 마운트 이후 진행하는 것이라 **useEffect를 사용하면 된다.**

```jsx
**const LazyImageModal = lazy(() => import("./components/ImageModal")); // <----------------------**

function App() {
  const [showModal, setShowModal] = useState(false);

  **useEffect(() => {
    const temp = import("./components/ImageModal");// <---------------------- 1.
  });**

  return (
    <div className="App">
      <ButtonModal
        onClick={() => {
          setShowModal(true);
        }}
      >
        올림픽 사진 보기
      </ButtonModal>
      <Suspense fallback={null}>
        {showModal ? (
          <**LazyImageModal // <---------------------- 1.**
            closeModal={() => {
              setShowModal(false);
            }}
          />
        ) : null}
      </Suspense>
    </div>
  );
}
```

<br>

프로세스

1. 컴포넌트 내부의 모든 로직이 실행된후
2. useEffect가 실행되어 ImageModal 컴포넌트를 import를 한다.

<br>

만약 여러개의 페이지를 최초에 preloading 한다면, facotry 패턴을 사용하면 좋다.

<br>

```jsx
const ReactPreloadComponent = (importComponent) => {
  const Component = lazy(importComponent);
  Component.preload = importComponent;

  return Component;
};

const LazyImageModal = ReactPreloadComponent(() =>
  import("./components/ImageModal")
);

function App() {
  const [showModal, setShowModal] = useState(false);

  useEffect(() => {
    LazyImageModal.preload();
  });

  return (
    <div className="App">
      <ButtonModal
        onClick={() => {
          setShowModal(true);
        }}
      >
        올림픽 사진 보기
      </ButtonModal>
      <Suspense fallback={null}>
        {showModal ? (
          <LazyImageModal
            closeModal={() => {
              setShowModal(false);
            }}
          />
        ) : null}
      </Suspense>
    </div>
  );
}

export default App;
```

<br>

`ReactPreloadComponent` 를 사용해서 앞으로 preload할 컴포넌트를 호출할땐 `component.preload()` 를 하면 하면된다.

<br>

## **react-lazy-with-preload**

preload을 할때 보일러 플레이트를 사용하지 않아도 되는 library가 있다.

**[react-lazy-with-preload](https://www.npmjs.com/package/react-lazy-with-preload)**

<br>

before

```jsx
Before: import { lazy, Suspense } from "react";
const OtherComponent = lazy(() => import("./OtherComponent"));
```

<br>

after

```jsx
import { Suspense } from "react";
import lazy from "react-lazy-with-preload";
const OtherComponent = lazy(() => import("./OtherComponent"));

// ...
OtherComponent.preload();
```

<br>

참고

- 인프런 프론트엔드 최적화 강의
- [https://blog.maximeheckel.com/posts/preloading-views-with-react/](https://blog.maximeheckel.com/posts/preloading-views-with-react/)
- [https://www.npmjs.com/package/react-lazy-with-preload](https://www.npmjs.com/package/react-lazy-with-preload)

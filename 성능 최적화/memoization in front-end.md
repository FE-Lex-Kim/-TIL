# memoization in front-end

<br>

**빈번하게 발생되는 복잡한 로직일때, 메모이제이션 기법으로 캐싱을 하는 방법을 사용하면 렌더링 성능에 향상된다.**

<br>

물론 처음에는 캐싱을 하지 않은 상태이기 때문에 느릴 수 있다.

하지만 같은 동작을 반복하는 경우일때 상당히 빠르게 동작한다.

<br>

## memoization cache 구현

<br>

```jsx
const cache = {};
export function getAverageColorOfImage(imgElement) {
  if (cache.hasOwnProperty(imgElement.src)) {
    return cache[imgElement.src];
  }

  const canvas = document.createElement('canvas');
  const context = canvas.getContext && canvas.getContext('2d');
  const averageColor = {
    r: 0,
    g: 0,
    b: 0,
  };

  ... // <----- 굉장히 복잡하고 고비용 로직이라고 하자.

  cache[imgElement.src] = averageColor;

  return averageColor;
}
```

<br>

getAverageColorOfImage 이라는 함수에 input이 들어오고 **averageColor 값을 만들기 위한 고비용 로직이다.**

<br>

cache에 input 값이 이전에 들어와있는지, `hasOwnProperty` 을 통해 **input 만의 고유한 값을 key로 넣어 이전에 캐싱되어 있었는지 비교한다.**

**만약 있다면 이전의 cache 값을 사용한다.**( `cache[imgElement.src]` )

<br>

**주의해야할 점은**

- 함수가 순수함수여서, **같은 input 값에 항상 같은 return 값이 나와야한다.**
- 최초에 cache에 저장할때, **cahce의 key을 각기 다른 input을 식별할 값으로 저장해야한다.** 위의 예제에서는 src(url)을 key로 설정했다.

<br>

하지만 매번 캐시를 하고 싶은 함수마다 memoization 하는 코드를 적어 주어야할까?

**보일러 플레이트 코드를 반복적으로 사용하는 것을 막기 위해 cache 모듈을 만들어보자.**

<br>

## memoization 폴더

![memoization react](./../Images/함수%20memoization,%20cache/함수%20memoization,%20cache-1.png)

<br>

위와 같이 모듈을 만들어주어서 memoization을 불러와 재사용한다.

<br>

memorize.js

```jsx
const cache = {};

function memorize(fc) {
  return function (...args) {
    const firstIndex = args[0]; // <----- // <----- callback의 첫번째 인자

    if (cache.hasOwnProperty(firstIndex)) {
      // <----- 첫번째 인자를 기준으로 cache key 값을 비교한다.
      return cache[firstIndex]; // <----- 이전에 호출한적 있으면, 그대로 캐싱된 값을 불러온다.
    }

    const result = fc(...args); // <-----  callback을 호출해서 cahce에 저장한다.
    cache[firstIndex] = result; // <----- 첫번째 인자를 key로 하고 callback 결과 값을 저장한다.

    return result;
  };
}

memorize((a, b) => a + b)(100, 900);
memorize(callback)(argument);
```

<br>

1. **캐싱을 하려는 함수를 memorize의 첫번째 인수로 넣어준다.**
2. argument는 **callback 함수의 인자값으로 들어가게된다.**
   - 예제를 보게되면 **a, b 는 각각 100, 900 값으로 들어간다.**

<br>

※ ...args : [Rest Parmeter](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/rest_parameters)

※ ... : [Spread syntax](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

<br>

### useMemo와 다른점

```jsx
const onLoadImage = (e) => {
  const averageColor = getAverageColorOfImage(e.target);
  dispatch(setBgColor(averageColor));
};

return <FullImage crossOrigin="*" src={src} alt={alt} onLoad={onLoadImage} />;
```

위와 같이 **event handler 내부에서 `getAverageColorOfImage`를 캐싱하고 싶을때는 useMemo를 사용할 수 없다.(`useMemo(**getAverageColorOfImage(e.target)**)`)**

**useMemo는 함수형 컴포넌트 또는 custom hook 내부에서만 사용가능하다.**

<br>

**따라서 event handler 내부에서 호출하는 함수를 memoization 하는 상황일때, useMemo 대신에 사용 하면된다.**

<br>

참고

- 인프런 프론트엔드 최적화 강의
- [https://ssangq.netlify.app/posts/memoization-and-react](https://ssangq.netlify.app/posts/memoization-and-react)
- [https://github.com/alexreardon/memoize-one](https://github.com/alexreardon/memoize-one)
- [https://www.daleseo.com/react-hooks-use-memo/](https://www.daleseo.com/react-hooks-use-memo/)

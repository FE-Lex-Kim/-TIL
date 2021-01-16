# CORS(Cross Origin Request) 오류

<br>

백엔드 서버는 4000포트, 리액트 서버는 3000포트로 열려 있다.

별도의 설정 없이 API를 호출 하면  CORS(Cross Origin Request) 오류가 생긴다.

네트워크 요청 할 때 주소가 다른 경우에 발생한다.

따라서 다른 주소에서도 API를 호출 가능하도록 서버쪽 코드를 수정하면된다.

<br>

**하지만!**

프로젝트를 다 완성한후 리액트 앱도 같은 호스트에서 제공해야하기 때문에 이러한 설정을 하면 불필요하다.

<br>

따라서 프록시를 사용하면된다.

<br>

웹팩 개발 서버에서 지원하는 기능으로, 개발 서버에서 요청하는 API들을 우리가 프록시로 정해둔 서버로 그대로 전달해주고 그 응답을 웹애플리케이션에서 사용할 수 있다.

<br>

**요청**

브라우저 ⇒ 리액트 개발 서버 ⇒ 프록시 ⇒ 백엔드 서버 요청

<br>

**응답**

백엔드 서버 ⇒ 개발서버 ⇒ 브라우저

<br>

### 프록시 설정 방법

<br>

package.json

```jsx
...
"development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  },
  "proxy": "http://localhost:4000/"
}
```

<br>

리액트 애플리케이션에서 client.get('/api/posts')를 하면 프록시를 통해

localhost:4000/api/posts 에 요청을 해준다.

<br>

참고 : [[책 리액트를 다루는 기술](http://www.yes24.com/Product/Goods/78233628?OzSrank=1)]
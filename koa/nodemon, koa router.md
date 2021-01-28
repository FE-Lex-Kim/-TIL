# nodemon, koa router

<br>

## nodemon

<br>

서버 코드를 변경할때마다 서버를 재시작해야 하는과정은 번거롭다.

nodemon 도구를 사용하면 코드가 변경 될때마다 자동으로 재시작 해주어 편리하다.

<br>

### 설치

<br>

개발용으로만 사용하는 도구이다 보니 개발용 의존 모듈로 저장하면 된다.

```bash
npm i nodemon -D
```

<br>

이후에 script를 package.json파일에 넣어준다.

npm start를 할때는 node src

npm run start:dev 할때는 nodemon --watch src/ src/index.js

<br>

cli 명령어에서 npm start를 제외하고는 npm 다음 run을 붙여주어야 실행된다.

<br>

—watch src/ 는 src디렉토리를 바로보고있다가 변경된부분이 있으면

src/index.js 를 재시작 시켜준다는 의미이다.

<br>

```json
"devDependencies": {
    "nodemon": "^2.0.7"
  },
  "scripts":{
    "start": "node src",
    "start:dev" : "nodemon --watch src/ src/index.js"
  }
```

<br>

이렇게 환경설정을 해준뒤에는 vscode를 재실행해주어야 적용된다.

<br>

## koa-router

<br>

브라우저에서 다른 주소로 api를 요청했을때 응답하는 방식을 처리할 수 있도록 하는 라우터를 사용해야 한다.

<br>

koa 자체에는 라우터 기능이 있지 않아 koa-router 모듈을 설치해야한다.

<br>

**설치**

```bash
npm i koa-router
```

<br>

### 사용법

<br>

**예제코드)**

```jsx
const koa = require('koa');
const Router = require('koa-router');

const app = new koa();
const router = new Router();

router.get('/', (ctx) => {
  ctx.body = '홈';
});

router('/about', (ctx) => {
  ctx.body = '자세히';
});

app.use(router.routes()).use(router.allowedMethods());

app.listen(4001, () => {
  console.log('Listening to port 4000');
});
```

<br>

처음에 koa-router를 모듈에서 가져온다.

가져와 Router 인스턴스를 만든후 HTTP요청하는 방식에따라 get,post,delete,put,patch메서드를 호출한다.

<br>

예를들어 브라우저에서 get방식으로 요청하면 router.get()메서드를 호출한다.

post방식으로 요청하면 router.post()메서드를 호출한다.

<br>

첫번째 인수에 HTTP 요청 받을 라우터를 넣어준다.

두번째 인수는 해당 HTTP 라우터에 응답할때 사용되어질 콜백함수를 넣어준다.

<br>

### 파라미터 ,쿼리

<br>

**예제코드)**

```jsx
const koa = require('koa');
const Router = require('koa-router');

const app = new koa();
const router = new Router();

router.get('/', (ctx) => {
  ctx.body = '홈';
});

router.get('/about/:name', (ctx) => {
  const { name } = ctx.params;
  ctx.body = `이름은 ${name}입니다.`;
});

router.get('/posts', (ctx) => {
  const { id } = ctx.query;
  ctx.body = `id는 ${id}입니다.`;
});

app.use(router.routes()).use(router.allowedMethods());

app.listen(4001, () => {
  console.log('Listening to port 4000');
});
```

<br>

😁**파라미터**

파라미터를 받을때 **ctx.params**로 받아올 수 있다.

만약 파라미터가 유동적으로 바뀌는 값으로 되어있다면 `'about/:name'`과 같이 **콜론을 붙여서 라우트 경로를 설정해준다.**

<br>

만약 파라미터가 존재 할지도 않을지도 모른다면 `'about/:name?'` 과 같이 **물음표를 붙여주면된다.**

<br>

😁**쿼리**

<br>

쿼리는 ctx.query에서 조회해서 가져올 수 있다.

받아올때는 쿼리 문자열이 자동으로 객체 형태로 파싱해주므로 파싱 함수를 실행시킬 필요가 없다.

<br>

만약 쿼리 문자열로 받으려고 할땐 ctx.querystring을 사용하면된다.

<br>

### 라우트 모듈화

<br>

index.js 파일 하나에 모든 코드를 작성하면 길어진다.

따라서 각 라우터별로 파일을 분리 시켜 작성하고, 불러와서 적용시키는 방법으로 하면 유지보수에도 좋다.

<br>

**예제코드)**

src/api/index.js

```jsx
const Router = require('koa-router');
const api = new Router();

api.get('/test', (ctx) => {
  ctx.body = 'test 성공';
});

module.exports = api;
```

<br>

src/index.js

```jsx
const koa = require('koa');
const Router = require('koa-router');
const api = require('./api');

const app = new koa();
const router = new Router();

router.use('/api', api.routes());
app.use(router.routes()).use(router.allowedMethods());

app.listen(4001, () => {
  console.log('Listening to port 4000');
});
```

<br>

api 디렉토리에있는 라우터를 src/index.js파일에 불러온다.(`require('./api');`)

불러온 파일을 index.js의 router 인스턴스에 적용시켜준다(`router.use`)

<br>

적용시켜줄때의 첫번째 인수에는 브라우저에서 api 디렉토리의 라우터들을 접근하기전 파라미터 주소를 적어준다.(`'api'`)

<br>

두번째 인수에는 api 디렉토리의 라우터들을 적용시켜준다(`api.routes()`)

<br>

**테스트)** [http://localhost:4001/api/test](http://localhost:4001/api/test) 에 접근하면 올바르게 나온다!

<br>

😁**api 라우트에서도 다른 라우터를 불러와서 적용 시킬 수 도있다.** 

<br>

**예제코드)**

src/api/posts/index.js

```jsx
const Router = require('koa-router');
const posts = new Router();

posts.get('/test', (ctx) => {
  ctx.body = 'test 성공';
});

const prinInfo = (ctx) => {
  ctx.body = {
    method: ctx.method,
    path: ctx.path,
    params: ctx.params,
  };
};

posts.get('/', prinInfo);
posts.post('/', prinInfo);
posts.get('/:id', prinInfo);
posts.delete('/:id', prinInfo);

module.exports = posts;
```

<br>

src/api/index.js

```jsx
const Router = require('koa-router');
const posts = require('./posts');
const api = new Router();

api.use('/posts', posts.routes());

module.exports = api;
```

<br>

src/index.js에 적용시켰던 방식대로 posts를 api 라우터에 적용시키면된다.

<br>

### 컨트롤러 파일

<br>

각 라우터 처리함수의 코드가 길면 라우터 설정할때 유지보수가 어렵고 가독성이 떨어진다.

따라서 **라우터를 처리하는 함수를 따로 모아놓은 파일을 만든다. 그 파일을 컨트롤러라고 부른다.**

<br>

브라우저에서 요청할때 POST, PUT, PATCH 방식으로 요청하면 JSON형식으로 payload가 온다.

JSON형식을 파싱하여 서버에서 사용할 수 있게해주는 미들웨어를 적용하면 좋다.

<br>

**설치**

```bash
npm i koa-bodyparser
```

<br>

**적용방법**

src/index.js

```jsx
const koa = require('koa');
const Router = require('koa-router');
const api = require('./api');
const bodyparser = require('koa-bodyparser');

const app = new koa();
const router = new Router();

router.use('/api', api.routes());

app.use(bodyparser());

app.use(router.routes()).use(router.allowedMethods());

app.listen(4001, () => {
  console.log('Listening to port 4000');
});
```

<br>

koa-bodyparser를 불러온뒤 bodyparser을 라우터 적용 전에 적용시켜준다.

<br>

**예제코드)**

<br>

posts/posts.ctrl.js

```jsx
let postId = 1;

const posts = [
  {
    id: 1,
    title: '제목',
    body: '내용',
  },
];

/* 포스트 작성
브라우저 요청 방법 POST /api/posts
{title, body}
 */
exports.write = (ctx) => {
  const { title, body } = ctx.request.body;
  postId += 1;
  const post = { id: postId, title, body };
  posts.push(post);
  ctx.body = post;
};
```

<br>

위와같이 라우트 함수를 처리하는 파일을 생성후

<br>

각각 함수를 exports 하며 함수를 만든다.

<br>

wirte코드에서는 브라우저에서 요청할때 받은 JSON 페이로드를 받을때 body로 보냈기 때문에

ctx.request.body을 통해 받는다.(`const { title, body } = ctx.request.body;`)

<br>

데이터베이스에서 데이터를 처리한 것을 보낼때는 ctx.body로 보내면된다.(`ctx.body = post;`)

<br>

posts 파일에 컨트롤러를 적용하는 방법

<br>

**예제코드)**

src/api/posts/index.js

```jsx
const Router = require('koa-router');
const postCtrl = require('./posts.ctrl');

const posts = new Router();

posts.post('/', postCtrl.write);

module.exports = posts;
```

<br>

컨트롤러 파일을 불러온뒤 라우터 처리함수 부분에 넣어주면된다.

<br>

- 이 글은 책 리액트를 다루는 기술을 참고하여 정리한 글 입니다.
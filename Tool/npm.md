# npm

<br>

## 지역,전역 설치( -g )

npm install 명령어에서 지역 또는 전역에서 설치 할 수 있다.

<br>

### 지역

지역은 해당 프로젝트 내에서만 사용할 수 있다.

옵션을 별도로 지정하지 않으면 지역에서 설치된다.

프로젝트 루트 디렉토리의 `node_module` 디렉토리가 자동으로 생성되고 그안에 패키지가 설치된다.

```bash
$ npm install <package>
```

<br>

### 전역

전역에 패키지를 설치하려면 `-g` 옵션을 지정하면된다.

모든 패키지에서 공통으로 사용하는 패키지는 전역에 설치하면 편리하다.

```bash
$ npm install -g <package>
```

- macOS의 경우
  - /usr/local/lib/node_modules

<br>

### 패키지 제거

```bash
# 로컬/개발 패키지 제거
$ npm uninstall <package-name>
# 전역 패키지 제거
$ npm uninstall -g <package-name>
```

## package.json

프로젝트 내에서 다양한 패키지들이 사용되어진다.

이러한 패키지들은 버전들이 빈번하게 업데이트 되므로 패키지들의 정보들을 일괄적으로 관리해야한다.

npm은 `package.json` 파일을 통해서 프로젝트의 정보와 패키지의 의존성을 관리한다.

<br>

여러 팀원들과 프로젝트를 할때 한명이 미리 패키지들을 다운받아 팀에게 배포하면 동일한 개발 환경에서 빠르게 설정 가능하다는 장점이 있다.

```bash
// package.json 설치
npm init
npm i
// 같은 init의 얼리어스
```

<br>

`npm init` 명령어를 하면 프로젝트의 여러가지 정보를 입력하라고 한다.

기본 설정값의 `package.json` 을 하려면 `npm init -yes` 또는 `-y` 옵션을 추가하면된다.

<br>

package.json 파일

```json
{
  "name": "jest-example",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "jest": "^27.2.0"
  }
}
```

`name` 과 `version`을 통해 각 패키지의 고유성을 판단한다.

<br>

`name` : 해당 프로젝트 이름

`version` : 해당 프로젝트 버전을 정의한다.

`description` : 프로젝트에 대한 설명, npm search로 패키지를 찾아내려고 할때 사용

`dependencies` : 해당 프로젝트가 의존하는 패키지들의 이름과 버전을 명시한다.

- `devDependencies` : 개발시에만 사용하는 개발용 의존 패키지를 의미한다. 배포할떄는 사용되지 않는다.

`scripts` : 프로젝트에서 자주 실행해야 하는 명령어를 scripts로 작성해두면 npm 실행 명령어로 사용가능하다.

`author` : 프로젝트 작성자 정보

`keywords` : `description` 과 마찬가지로 npm search로 패키지를 찾아냐려고 할때 사용

<br>

## script 프로퍼티 실행

`package.json` 파일 내부에서 `script` 의 명령어를 실행 시키기 위해서는 npm 과 함께 사용하면된다.

<br>

script의 start 프로퍼티를 실행

```bash
npm start
```

<br>

하지만 `start` 프로퍼티를 제외한 나머지의 명령어들은 모두 **`run`** 을 중간에 넣어주어야한다.

```bash
npm run test
npm run <script-name>
```

<br>

참고

- [https://poiemaweb.com/nodejs-npm](https://poiemaweb.com/nodejs-npm)
- [https://programmingsummaries.tistory.com/385](https://programmingsummaries.tistory.com/385)
- [https://edu.goorm.io/learn/lecture/557/한-눈에-끝내는-node-js/lesson/174371/package-json](https://edu.goorm.io/learn/lecture/557/%ED%95%9C-%EB%88%88%EC%97%90-%EB%81%9D%EB%82%B4%EB%8A%94-node-js/lesson/174371/package-json)

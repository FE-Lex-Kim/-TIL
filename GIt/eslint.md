# Eslint

<br>

ESLint는 코팅 스타일 가이드를 따르지 않거나 문제가 있는 코드나 안티 패턴을 찾기 위해 사용하는 Javascript linter이다.

<br>

개발자가 자신의 스타일 가이드를 작성할 수도 있다.

<br>

코딩 컨벤션에 위배되는 코드나 안티 패턴을 자동 검출 해주므로 옳바른 코딩 습관을 갖도록 돕는 유용한 툴이다.

<br>

## 설치방법

<br>

```bash
$ cd <project-folder>
$ npm init -y
# install eslint & friends
$ npm install eslint eslint-config-airbnb-base eslint-plugin-import eslint-plugin-html --save-dev
```

<br>

## VSCode extentio 설치

<br>

설정에서 추가

```json
...
  // ESLINT
  "eslint.validate": [
    "javascript",
    "html"
  ]
```

<br>

## .eslintrc.json 파일 생성

<br>

파일안에 룰셋 변경

```json
{
  "parserOptions": {
    "ecmaVersion": 9
  },
  "env": {
    "browser": true,
    "commonjs": true,
    "node": true,
    "jquery": true
  },
  "extends": "airbnb-base",
  "plugins": [ "import", "html" ],
  "rules": {
    // "off" or 0 - turn the rule off
    // "warn" or 1 - turn the rule on as a warning (doesn’t affect exit code)
    // "error" or 2 - turn the rule on as an error (exit code is 1 when triggered)
    // "no-var": 0,
    "no-console": 0,
    "no-plusplus": 0,
    "vars-on-top": 0,
    "no-underscore-dangle": 0, // var _foo;
    "comma-dangle": 0,
    "func-names": 0, // setTimeout(function () {}, 0);
    "prefer-arrow-callback": 0, // setTimeout(function () {}, 0);
    "prefer-template": 0,
    "no-nested-ternary": 0,
    "max-classes-per-file": 0,
    "arrow-parens": ["error", "as-needed"], // a => {}
    "no-restricted-syntax": [0, "ForOfStatement"],
    "no-param-reassign": ["error", { "props": false }]
  }
}
```
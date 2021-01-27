# npm install --save-dev, --save-prod

<br>

## —save-dev

<br>

—save-dev 커맨드는

패키지를 설치할때 배포용이 아닌 개발할때만 사용하는 패키지를 package.json의 `devDependencies` 에 저장해준다.

배포할때는 dev로 저장된 패키지는 적용되지 않는다.

<br>

—save-dev 대신에 단축명령어로 -D로 해도된다.

`-D, --save-dev: Package will appear in yourdevDependencies`

<br>

![--save-dev](../Images/--save_dev,%20--save-prod/--save-dev,--save_prod-1.png)

<br>

![--save-dev](../Images/--save_dev,%20--save-prod/--save-dev,--save_prod-2.png)

## --save-prod

<br>

--save-prod커맨드는

npm install의 기본값으로 package.json 의 dependencies에 저장해준다.

배포되어질때도 적용되어진다.

<br>

## 요약

<br>

배포용 패키지 설치는 npm install (—save-prod 자동 기본값) `dependencies` 에 저장

`npm install <package-name> [--save-prod]`

<br>

개발용 패키지 설치는 npm install —save-dev `devDependencies` 에 저장

`npm install <package-name> --save-dev`

<br>

참고

- 이 글은 [npm 공식문서](https://docs.npmjs.com/specifying-dependencies-and-devdependencies-in-a-package-json-file#adding-dependencies-to-a-packagejson-file-from-the-command-line)를 참고했습니다.
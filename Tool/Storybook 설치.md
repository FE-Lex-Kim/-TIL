# Storybook

<br>

스토리 북은 UI 테스팅 도구이다.

Storybook은 [컴포넌트 기반 개발(Component-Driven Development)](https://www.componentdriven.org/)으로 UI를 만들기에 좋다.

<br>

리액트에서 컴포넌트별로 분리하여 재사용이 용이하게 개발하는것이 목적이다.

<br>

하지만 페이지별로 컴포넌트를 화면에 렌더링을 하여 테스팅을 진행해야만 했고 이렇게 개발하다보니 컴포넌트별로 개발을 하지 못하고 하나의 페이지별로 컴포넌트의 코드를 작성하게 되는 상황이 자주 오게된다.

<br>

이러한 상황은 컴포넌트의 재사용을 하는데 더욱 힘들게 한다.

<br>

Storybook은 이러한점에서 페이지별 라우팅을 해주어야만 테스팅이 가능한것이 아닌 각각 컴포넌트별로 쉽게 테스팅을 할 수 있게 해주는 강력한 Tool이다.

<br>

## 설치

```bash
npx -p @storybook/cli sb init
```

<br>

스토리북을 설치한뒤에는 packge.json 파일의 스크립트 부분에는 storybook이 들어가 있다.

<br>

스토리북을 6006으로 실행한다는 스크립트이다.

```bash
npm run storybook
```

<br>

스크립트에서는 start를 제외하고 다른 스크립트는 run을 붙여주어야 한다.

![Storybook 설치](../Images/Storybook%20설치/Storybook설치-1.png)

Storybook을 설치하면 Storybook이라는 디렉토리가 생성된다.

<br>

이곳에서 실제로 스토리북에 보여지는 컴포넌트들을 정의해주는 곳이다.

<br>

참고

- 이글은 [Storybook 공식홈페이지](https://www.learnstorybook.com/)를 참고하여 정리 한글입니다.
- [https://hyunseob.github.io/2018/01/08/storybook-beginners-guide/](https://hyunseob.github.io/2018/01/08/storybook-beginners-guide/)
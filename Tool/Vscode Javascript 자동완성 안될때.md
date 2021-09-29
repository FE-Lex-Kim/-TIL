# Vscode Javascript 자동완성 안될때

실수로 vscode 하단 좌측에 빨간색으로 경고 메세지가 떴었는데 내용도 안읽고 그냥 해당 프로그램 삭제를 했었다.

그 이후로 Javscript의 자동완성이 아예 안되고 있었다.(React 자동완성이 안되는것을 보고 처음엔 React만 안되는줄 알았음)

<br>

예를들면 Math, doucument, Array.. 등등 자바스크립트에서 사용하는 모든 키워드들을 자동완성으로 사용할 수 없었다.

3시간 동안 해매다가 겨우 찾아 냈다.

<br>

해당 기능은 vscode의 default built-in extions 이였어서 삭제하며 안됬었다.

<br>

![Vscode Javascript 자동완성 안될때](../Images/Vscode%20Javascript%20자동완성%20안될때/Vscode%20Javascript%20자동완성%20안될때-1.png)

<br>

extion에서 `@builtin TypeScript and JavaScript Language Features` 을 타이핑 하고 해당 기능을 사용하면 된다.

<br>

참고

- [https://stackoverflow.com/questions/59860224/how-can-i-disable-initializing-js-ts-language-features-in-a-specific-project](https://stackoverflow.com/questions/59860224/how-can-i-disable-initializing-js-ts-language-features-in-a-specific-project)

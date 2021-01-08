# Custom Snippet

<br>

- 이 글은 책 [리액트를 다루는 기술](http://www.yes24.com/Product/Goods/78233628?OzSrank=1) 을 참고하여 정리한 글입니다.

<br>

VScode 마켓플레이스에서 리액트 컴포넌트 작업시간을 줄일 수 있는 vscode-styled-components를 통해 Snippet을 설치 할 수 있다.

<br>

직접 사용자의 편의에 따라 Snippet을 만들 수 있다.

먼저 Snippet으로 사용하고 싶은 코드를 복사한뒤 [snippet generator](https://snippet-generator.app/)의 좌측 박스에 붙여넣기 하면된다.

<br>

- Description은 설정하려는 Sinippet에 대한 설명을 적는다.
- Tab trigger 부분에는 단축키로 쓰려는 값을 적으면된다. ex) srfc

<br>

[Sinppet에서 사용할 수 있는 동적 값](https://code.visualstudio.com/docs/editor/userdefinedsnippets) 은 여기서 더 찾아 볼 수 있다.

<br>

그 우측 박스의 텍스트를 복사한다. 

<br>

예제코드)

```
"styled React Functional Component": {
  "prefix": "srfc",
  "body": [
    "import React from 'react';",
    "import styled from 'styled-components';",
    "",
    "const ${TM_FILENAME_BASE} = styled.div``;",
    "",
    "const ${TM_FILENAME_BASE} = () => {",
    "  return <${TM_FILENAME_BASE}Block></${TM_FILENAME_BASE}Block>;",
    "};",
    "",
    "export default ${TM_FILENAME_BASE};",
    ""
  ],
  "description": "styled React Functional Component"
}
```

TM_FILENAME_BASE는 확장자를 제외한 파일이름을 의미한다.

<br>

VScode로 돌아가서 사용자 코드조각으로 들어간다.

![Custom Snippet](../Images/Custom%20Snippet/Custom%20Snippet-1.png)

<br>

이후 javascriptreact을 검색한뒤 들어간다.

![Custom Snippet](../Images/Custom%20Snippet/Custom%20Snippet-2.png)

<br>

그곳에 이전에 복사한 텍스트를 붙여넣어준다.

이후 VScode 하단에 javscript에서 jascriptReact로 바꾸어 주어야 적용된다.

<br>

![Custom Snippet](../Images/Custom%20Snippet/Custom%20Snippet-3.png)

<br>

언어모드에서 .js에 대한 파일연결 구성을 누른뒤

javascriptReact를 선택하면 된다.

![Custom Snippet](../Images/Custom%20Snippet/Custom%20Snippet-4.png)

<br>

[Sinppet에서 사용할 수 있는 동적 값](https://code.visualstudio.com/docs/editor/userdefinedsnippets) 은 여기서 더 찾아 볼 수 있다.

<br>

이후 파일에 srfc를 입력하면 자동완성이 된다.
# DOM

<br>

브라우저의 렌더링 엔진은 HTML 문서를 파싱하여 브라우저가 이해할 수 있는 자료구조인 DOM을 생성한다.

<br>

이러한 HTML 요소 간의 부자 관계를 반영하여 HTML 문서의 구성 요소인 HTML 요소를 객체화한 모든 노드 객체들을 트리 자료구조로 구성한다.

**노드 객체들로 구성된 트리 자료구조를 DOM(Document Object Model)이라 한다**

<br>

## DOM 생성 과정

1. 서버는 브라우저가 요청한 HTML 파일을 읽어 바이트를 인터넷을 통해 응답한다.
2. 브라우저는 응답받은 HTML 문서를 바이트 형태로 응답 받는다.
   - 바이트 형태의 HTML 문서는 meta 태그의 charset 어트리뷰트에 의해 지정된 인코딩 방식을 기준으로 문자열로 변환된다.
   - 이 contenty-type 과 charset 어트리뷰트는 서버에서 응답 헤더에 담겨 응답받는다.
3. 변환된 문자열로 HTML 을 읽어 문법적 의미를 갖는 최소단위인 토큰들로 분해한다.

   ![DOM](../Images/브라우저%20렌더링%20과정/브라우저%20렌더링%20과정-1.png)

   이미지 출처 : [https://vallista.kr/2019/05/07/DOM-Document-Object-Model/](https://vallista.kr/2019/05/07/DOM-Document-Object-Model/)

4. 토큰들을 객체로 변환하여 노드들을 생성한다. 이 노드들은 문서 노드, 요소 노드, 어트리뷰트 노드, 텍스트 노드가 생성되어 DOM을 구성하는 기본요소가 된다.
5. HTML 요소들은 중첩 관계를 갖는다. 따라서 부모 자식 관계로 노드들이 구성되며 트리 자료구조로 구성되어진다. 이 트리 자료구조를 DOM(Document Object Model) 이라고 부른다.

<br>

즉, DOM은 HTML 문서를 파싱한 결과물이다.

<br>

## DOM의 사용

### 1. 랜더트리

이렇게 생긴 DOM과 CSSOM을 합쳐 렌더링을 위한 렌더 트리를 만든다.

<br>

렌더 트리는 **렌더링을 위한 트리 구조의 자료구조이다.**

따라서 브라우저 화면에 렌더링 되지 않는 노드와(meta, script 태그 같은) CSS에 의해 표시되지 않는(display : none) 과 같은 노드들은 포함 되지 않는다.

<br>

즉, 렌더 트리는 화면에 렌더링 되는 노드만으로 구성된다.

<br>

렌더 트리는 HTML 요소의 레이아웃(위치, 크기)을 계산하는데 사용되며 브라우저 화면에 픽셀을 렌더링 하는 페인팅 처리에 사용된다.

<br>

### 2. DOM API 조작

**DOM은 HTML 문서의 계층적 구조와 정보를 표현하는 것은 물론 노드 객체의 종류, 즉 노드 타입에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API(Application Programming Interface)로 제공한다.**

<br>

**이 DOM API를 통해 HTML의 구조나 내용 또는 스타일 등을 동적으로 조작할 수 있다.**

<br>

## DOM API 사용

<br>

### 요소 노드 취득

1. **id** 를 이용한 요소 노드 취득
   - `Document.prototype.getElementById` 메서드를 통해 id에 해당하는 요소를 탐색하여 반화한다.
   - 여러개의 id가 존재하더라도 첫번째 요소 노드를 반환한다.
   - 없으면 null
2. **태그** 이름을 이용한 요소 노드 취득
   - `Document.prototype/Element.prototype.getElementsByTagName` 메서드를 통해 해당 태그 이름을 갖는 모든 요소를 탐색해서 반환한다.
   - 여러개의 노드를 담은 HTMLCollection 객체를 반환한다.
   - 모든 노드를 가지려면 \*를하면된다.
3. **class** 를 이용한 요소 노드 취득
   - `Document.prototype/Element.prototype.getElementsByClassName` 메서드를 통해 해당 class와 같은 값을 탐색하여 반환한다.
   - 여러개의 노드를 갖는 HTMLCollection 반환한다.
4. **CSS** 를 이용한 요소 노드 취득
   - `Document.prototype/Element.prototype.querySelector` 메서드를 통해 인수로 전달한 CSS 선택자를 만족시키는 하나의 요소 노드를 반환한다.
   - 여러개인 경우 첫번째 노드
   - 없으면 null
   - css 선택자 문법에 맞지 않으면 DOMException에러
   - `querySelectorAll` 메서드는 css선택자를 만족한는 모든 노드를 NodeList에 들어가 반환된다.
     - 만족하지 않으면 빈 NodeList 반환
     - css 선택자 문법에 맞지 않으면 DOMException에러
5. 특정 요소 노드를 취득할 수 있는지 확인
   - `Element.prototype.matches` 메서드를 통해 CSS 선택자에 해당하는 노드를 취득할 수 있는지 확인후 true, false값을 반환한다.
6. HTMLCollection 과 NodeList
   - 이 둘 객체는 메서드를 사용해 여러개의 노드를 반환하기위한 객체이다.
   - 이터러블이여서 스프레드 문법을 사용해 배열로 변환하여 사용가능하다.
   1. HTMLCollection
      - 노드 객체의 상태 변화를 실시간으로 반영하는 객체이다
      - 따라서 반복문을 통해 상태를 변경시키면 객체가 실시간으로 바뀌므로 개발자의 의도대로 동작하지 않을 수 도 있다.
      - 따라서 스프레드 문법을 통해 배열로써 변환하여 사용하면된다.
   2. NodeList
      - HTMLCollection 객체의 부작용을 해결하기 위해 querySelectorAll 메서드를 사용해서 NodeList 객체를 반환 시키면된다.
      - NodeList 객체는 실시간으로 객체 상태 변경을 반영하지 않는다.
      - 하지만 childNodes 프로퍼티가 반환하는 NodeList는 live객체로 동작한다.
   - 따라서 HTMLCollection 이나 NodeList 객체를 배열로 변환하여 사용하는 것을 권장한다.

<br>

### 노드 탐색

- 요소 노드를 취득한후 , 취득한 요소 노드를 기점으로 부모, 형제, 자식 노드를 탐색할 경우도 있다.

1. **공백 텍스트** 노드
   - HTML 요소 사이의 스페이스, 탭, 줄바꿈 등의 공백 문자는 텍스트 노드를 생성한다.
   - 이것을 공백 텍스트 노드라고 한다.
2. **자식 노드** 탐색
   - 자식 노드를 탐색할때는 6가지 방법이 있다.
     1. `childNodes` → 텍스트 노드를 포함한 모든 자식 노드를 NodeList 담아 반환한다.
     2. `children` → 텍스트 노드를 제외한 요소 노드만 HTMLCollection에 담아 반환한다.
     3. `firstChild` →텍스트 노드를 포함해서 첫번째 자식 노드를 반환
     4. `lastChild` → 텍스트 노드를 포함해서 마지막 자식 노드를 반환
     5. `firstElementChild` → 요소 노드에 해당하는 자식 첫번째 자식 노드를 반환
     6. `lastElementChild` → 요소 노드에 해당하는 자식 마지막 자식 노드를 반환
3. 자식 노드 존재 확인
   - `Node.prototype.hasChildNodes` 메서드를 통해 텍스트 노드를 포함한 자식 노드가 존재 하면 true, 아니면 false를 반환한다.
   - 따라서 자식 요소 노드만 존재하는지 파악하려면 `childElementCount` 또는 `children.length` 를 사용하면 된다.
4. 요소 노드의 텍스트 노드 탐색
   - `firstChild` 를 통해 텍스트 노드로 접근할 수 있다.
5. 부모 노드 탐색

   - `Node.prototype.parentNode` 프로퍼티를 사용한다.
   - 부모노드가 텍스트 노드인 경우는 없다.

     → 텍스트 노드는 최종단 노드인 리프 노드이기 때문이다.

6. 형제 노드 탐색
   - 형제 노드 탐색방법 4가지
   1. `previousSibling` → 텍스트 노드를 포함한 이전 형제 노드 탐색
   2. `nextSibling` → 텍스트 노드를 포함한 다음 형제 노드 탐색
   3. `previousElementSibling` → 요소 노드만인 이전 노드 탐색
   4. `nextElementSibling` → 요소 노드만인 다음 노드 탐색

<br>

### 노드 정보 취득

- 2가지 방법으로 노드에 대한 정보를 취득 할 수 있다.

1. nodeType → 노드 타입을 상수로 반환한다.
   - 1이면 요소노드
   - 3이면 텍스트 노드
   - 9이면 문서 노드
2. nodeName
   - 노드의 이름을 문자열로 반환
   - 요소 노드: 대문자 문자열로 태그 이름을 반환
   - 텍스트 노드 : 문자열 `#text`를 반환
   - 문서노드 : 문자열 `#document` 를 반환

<br>

### 요소 노드의 텍스트 조작

1. nodeValue
   - **텍스트 노드**의 값인 텍스트를 반환한다.
   - 텍스트 노드가 아니면 null
2. textContent
   - 텍스트 노드가 아니더라도 요소 노드내의 모든 텍스트를 반환한다.
   - 하지만 textContent 값에 문자열을 할당하면 내부의 모든 노드들이 제거되고 입력한 문자열이 추가된다.
   - 유사한 동작을하는 InnerText 프로퍼티는 CSS에 의해 비표시 되는 요소의 텍스트는 반환하지 않고 CSS를 반영해서 속도도 느리다.

<br>

### DOM 조작

- DOM 조작은 새로운 노드를 생성하여 DOM에 추가하거나 삭제 또는 교체를 말한다.

1. innerHTML
   - `Element.prototype.innerHTML`
   - HTML 마크업이 포함된 문자열을 그대로 반환한다.
   - 문자열을 할당하면 요소 노드의 모든 자식 노드가 제거
   - 할당된 문자열에 요소 노드로 작성하면 DOM에 반영
   - XSS에 취약하여 보안에 위험이 있다.
2. insertAdjacentHTML 메서드
   - `Element.prototype.insertAdjacentHTML` 메서드는 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입한다.
   - 두 번째 인수로 전달한 HTML 마크업 문자열을 파싱하고 그 결과로 생성된 노드를 첫 번째 인수로 전달한 위치에 삽입하여 DOM에 반영한다.
   - 첫번째 인수 4가지
     1. "beforebegin" → 자신의 노드를 기준으로 시작 태그 이전 노드에 삽입
     2. "afterbegin" → 자신의 노드를 기준으로 시작 태그 내부 첫번째 노드에 삽입
     3. "beforeend" → 자신의 노드를 기준으로 내부에 종료 태그 이전 노드 삽입
     4. "afterend" → 자신의 노드를 기준으로 종료 태그 다음에 노드 삽입
3. 노드 생성과 추가
   1. 요소 노드 생성
      - `Document.prototype.createElement` 인수로 태그 이름을 넣어 노드를 생성후 반환
   2. 텍스트 노드 생성
      - `Document.prototype.createTextNode` 인수에 텍스트 노드 값인 문자열을 전달 하여 텍스트 노드 생성후 반환
   3. 텍스트 노드를 요소 노드의 자식 노드로 추가
      - `Node.prototype.appendChild` 인수로 전달한 텍스트 노드를 마지막 자식 노드로 추가
   4. 요소 노드를 DOM에 추가
      - `Node.prototype.appendChild` 인수로 전달한 요소 노드를 마지막 자식 노드로 추가
4. 복수의 노드 생성과 추가
   - 배열안의 텍스트나 노드를 요소 노드를 생성후 `Document.prototype.createDocumentFragment` 를 통해 생성한 container 요소 노드의 요소로 넣은 후 container 노드를 다른 노드에 할당하면 container 요소 노드는 자신이 제거되고 자식 노드만 DOM에 추가된다.
5. 노드 삽입
   - 마지막 노드로 추가 → `Node.prototype.appendChild`
   - 지정한 위치에 노드 삽입 → `Node.prototype.insertBefore` 메서드는 첫 번째 인수로 전달받는 노드를, 두 번째 인수로 전달받은 노드앞에 삽입한다.
6. 노드 이동
   - DOM에 이미 존재하는 노드를`appendChild` 또는 `insertBefore` 메서드를 사용하여 DOM에 다시 추가하면 현재 위치에서 노드를 제거하고 새로운 위치에 노드를 추가한다.
7. 노드 복사
   - `Node.prototype.cloneNode` 메서드느 생성하여 반환한다.
   - 인수에 true를 전달하면 깊은 복사를 하여 모든 자식 노드가 사본을 생성
   - false를 인수로 전달 또는 생략하면 노드 자신만 사본을 생성
8. 노드 교체
   - `Node.prototype.replaceChild(newChild, oldChild)` 메서드는 자신을 호출한 노드의 자식 노드를 다른 노드로 교체한다. 첫 번째 매개변수 newChild에는 교체할 새로운 노드를 인수로 전달하고, 두 번째 매개변수 oldChild에는 이미 존재하는 교체될 노드를 인수로 전달한다.
9. 노드 삭제
   - `Node.prototype.removeChild(child)`메서드는 child 매개변수에 인수로 전달한 노드를 DOM에서 삭제한다.

<br>

### 어트리뷰트

1. 어트리뷰트 노드와 atrributes 프로퍼티
   - 모든 어트리뷰트 노드의 참조는 유사 배열 객체이자 이터러블인 NamedNodeMap 객체에 담겨서 요소 노드의 attributes 프로퍼티에 저장된다.
   - 요소 노드의 모든 어트리뷰트 노드는 요소 노드의 `Element.prototype.attributes` 프로퍼티로 취득할 수 있다.
2. HTML 어트리뷰트 조작
   - **어트리뷰트 값을 참조**하려면`Element.prototype.getAttribute(attributeName)` 메서드를 사용한다.
   - **어트리뷰트 값을 변경**하려면`Element.prototype.setAttribute(attributeName, attributeValue)` 메서드를 사용한다.
   - **어트리뷰트가 존재**하는지 확인하려면 `Element.prototype.hasAttribute(attributeName)` 메서드를 사용
   - **어트리뷰트를 삭제**하려면 `Element.prototype.removeAttribute(attributeName)` 메서드를 사용한다.
3. HTML 어트리뷰트 vs DOM 프로퍼티

   - HTML 어트리뷰트는 DOM 중복 관리되고 있는 것처럼 동작한다.

     1. 요소 노드의 attribute 프로퍼티에 저장된 어트리뷰트 프로퍼티(요소 노드의 attribute에 저장되어있음)

        → 초기 상태 관리

     2. HTML 어트리뷰트에 대응하는 요소노드의 프로퍼티(요소노드의 프로퍼티에 저장되어있음)

        → 최신 상태 관리

   1. 어트리뷰트 노드
      - HTML 어트리뷰트로 지정한 HTML 요소의 초기 상태는 어트리뷰트 노드에서 관리한다.
      - 값이 변경되어도 값이 변경되어지지 않는다.
   2. DOM 프로퍼티
      - 사용자가 입력한 최신 상태는 HTML 어트리뷰트에 대응하는 요소 노드의 DOM 프로퍼티가 관리한다. DOM 프로퍼티는 사용자의 입력에 의한 상태 변화에 반응하여 언제나 최신 상태를 유지한다.
   3. DOM 프로퍼티 값의 타입
      - checked 프로퍼티 값은 불리언 타입이다.
      - getAttribute 메서드로 취득한 어트리뷰트 값은 언제나 문자열이다.

4. data 어트리뷰트와 dataset 프로퍼티
   - data 어트리뷰트와 dataset 프로퍼티를 사용하면 HTML 요소에 정의한 사용자 정의 어트리뷰트와 자바스크립트 간에 데이터를 교환할 수 있다.

<br>

### 스타일

1. 인라인 스타일 조작
   - `HTMLElement.prototype.style` 프로퍼티는 요소 노드의 인라인 스타일을 취득하거나 추가 또는 변경한다.
2. 클래스 조작
   - className
     - `Element.prototype.className` 프로퍼티는 class 어트리뷰트 값을 문자열로 반환한다.
   - classList
     - `Element.prototype.classList` 프로퍼티는 class 어트리뷰트의 정보를 담은 DOMTokenList 객체를 반환한다.
     - DOMTokenList 객체는 다음과 같이 유용한 메서드들을 제공한다.
       1. `add`메서드는 인수로 전달한 1개 이상의 문자열을 class 어트리뷰트 값으로 추가한다.
       2. `remove`메서드는 인수로 전달한 1개 이상의 문자열과 일치하는 클래스를 class 어트리뷰트에서 삭제한다
       3. `item` 메서드는 인수로 전달한 index에 해당하는 클래스를 class 어트리뷰트에서 반환한다.
       4. `contains` 메서드는 인수로 전달한 문자열과 일치하는 클래스가 class 어트리뷰트에 포함되어 있는지 확인한다.
       5. `replace` 메서드는 class 어트리뷰트에서 첫 번째 인수로 전달한 문자열을 두 번째 인수로 전달한 문자열로 변경한다.
       6. `toggle` 메서드는 class 어트리뷰트에 인수로 전달한 문자열과 일치하는 클래스가 존재하면 제거하고, 존재하지 않으면 추가한다.
          - 두 번째 인수로 불리언 값으로 평가되는 조건식을 전달할 수 있다.
          - 이때 조건식의 평가 결과가 true이면 class 어트리뷰트에 강제로 첫 번째 인수로 전달받은 문자열을 추가하고, false이면 class 어트리뷰트에서 강제로 첫 번째 인수로 전달받은 문자열을 제거한다.

<br>

참고

- [책 모던 자바스크립트 Deep Dive](http://www.yes24.com/Product/Goods/92742567)

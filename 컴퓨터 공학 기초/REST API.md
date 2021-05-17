# REST API

<br>

- [https://www.youtube.com/watch?v=RP_f5dMoHFc](https://www.youtube.com/watch?v=RP_f5dMoHFc) 을 보고 정리한 글입니다.

<br>

### 인터넷에서 어떻게 정보를 공유할 것인가?

정보들을 하이퍼텍스트로 연결한다.

<br>

정보를 표현 하는 방식을 HTML 형식으로 한다.

그 정보들에 대한 **식별자(식별자에 정보에대한 값이 바인딩 되어있음)**를 URI로 한다.

그 정보들을 전송하는 방법으로 HTTP를 사용한다.

\*\* 하이퍼텍스트 : A에서 B로 갈때, A와 B사이에 있는 모든 페이지를 거치는것이 아니라, 비선형적으로 연결되어 있어서 한번에 B로 넘어간다는 것이다.

<br>

## REST가 생겨나진 배경

HTTP를 설계한 사람중 한명인

Roy T. Fielding(로이 필딩)은 HTTP가 이미 생겨난 시점에서 명세에 기능을 더하고 기존의 기능을 고쳐야 하는 상황이였다.

<br>

이미 구축되어있던 웹과 호환성을 고민해야했다.

이것이 REST이다.

<br>

즉, REST는 웹의 독립적 진화를 위해 만들어졌다.

<br>

## REST

REST는 분산 하이퍼 미디어 시스템(예: 웹)을 위한 아키텍쳐 스타일

<br>

### 아키텍쳐 스타일?

- 제약조건의 집합

<br>

즉, 제약조건을 모두 다 만족시켜야 REST를 따른다 라고 할 수 있다.

<br>

## 그렇다면 REST를 구성하는 스타일이 무엇인가?

- client-server
- stateless
- cache
- **uniform interface**
- layered system
- code-on-demand(optional)

  → 코드를 서버에서 클라이언츠로 보내서 실행할 수 있어야하는것을 의미 ex) 자바스크립트

<br>

모두다 HTTP만 따르면 조건에 부합하지만 Uniform Interface의 제약조건을 따르기가 힘들다.

<br>

### Uniform Interface의 제약조건이란?

- identification of resources

  → 리소스가 URI로 식별되면 된다.

- manipulation of resources through representations

  → representation 전송을 통해서 리소스를 조작해야한다.

  → 리소스를 만들거나, 지우거나, 삭제, 조작할때 HTTP 메세지에다가 표현을 담아서 전송을 해야한다.

  → 이미 대체로 잘 지켜진다.

- **self-descriptive messages**
- **hypermedia as the engine of application state(HATEOAS)(헤이티오스)**

<br>

이중 3, 4번은 현재 대부분의 REST API에서 잘지켜지지 않고있다.

<br>

### **self-descriptive messages**

메세지는 스스로를 설명해야한다.

<br>

- 목적지를 추가해야한다.

  ![REST API](../Images/REST%20API/REST%20API-1.png)

  이미지 출처 : [https://www.youtube.com/watch?v=RP_f5dMoHFc](https://www.youtube.com/watch?v=RP_f5dMoHFc)

- 어떤 문법으로 작성되어있는지 알아야하므로 컨텐트 타입을 넣어주어야한다.

  → 파싱이 가능하고 문법을 이해할 수 있다.

  → `application/json`

  ![REST API](../Images/REST%20API/REST%20API-2.png)

  이미지 출처 : [https://www.youtube.com/watch?v=RP_f5dMoHFc](https://www.youtube.com/watch?v=RP_f5dMoHFc)

- 또한 `-patch+json` 이부분을 추가적으로 넣어주어야한다.

  → op , path가 어떤의미인지 알아야하므로 patch+json 명세를 본뒤 그에대한 명세를 찾아서 의미를 부여한뒤 해석한다.

<br>

이 처럼 메세지를 보았을때 메세지의 내용으로 온전히 해석이 다 가능해야한다.

하지만 오늘날의 API가 다 만족을 하지 못한다.

<br>

`application/json`으로만 되어있고(어떤 문법으로 작성되어 있는지만 알려짐) 이걸 어떻게 해석 해야하는지는 메세지만 보고서는 알 수 없다.(`-patch+json`)

<br>

### **hypermedia as the engine of application state(HATEOAS)**

애플리케이션의 상태는 Hyperlink를 이용해 전이되어야한다.

<br>

상태 전이란 무엇일까?

어떠한 정보를 조회하는 URI가 있을때 그 이후 동작할 행동들을 상태 전이가 가능한 것이라고 하는것이다

ex) 게시글 조회하는 URI

- 다음 게시물 조회
- 게시물 저장
- 댓글 달기

이러한 행동들을 상태 전이가 가능한것이고 이것들을 응답 본문에 넣어 주어야한다.

![REST API](../Images/REST%20API/REST%20API-3.png)

이미지 출처 : [https://www.youtube.com/watch?v=RP_f5dMoHFc](https://www.youtube.com/watch?v=RP_f5dMoHFc)

이처럼 Hyperlink를 클릭해 링크를 따라가면서 전이 되었기 때문에 이것은 HATEOAS가 되었다.

<br>

![REST API](../Images/REST%20API/REST%20API-4.png)

이미지 출처 : [https://www.youtube.com/watch?v=RP_f5dMoHFc](https://www.youtube.com/watch?v=RP_f5dMoHFc)

<br>

위의 코드 풀이)

이전 정보는 `<articles /1>` 이고

다음 정보는 `<articles /3>` 이다.

<br>

이 메세지의 상태가 어떤지 파악하고 hyperlink를 타고 다른 상태로 전이가 가능하다.

<br>

Link라는 해더는 이 리소스와 연결되어있는 다른 리소스를 가리킬 수 있는 기능이다.

```jsx
HTTP/1.1 200 OK
Content-Type: application/json

{'objects': [...], 'nextUrl': 'http://example.com/objects/?from=...'}
```

<br>

```jsx
HTTP/1.1 200 OK
Link: <http://example.com/objects/?from=...>; rel=next
Content-Type: application/json
```

출처 : [https://blog.hongminhee.org/2012/05/27/http-link-header/](https://blog.hongminhee.org/2012/05/27/http-link-header/)

<br>

와 같이 리소스에 대한 정보를 링크 해더를 통해서 정보를 받을 수 있다.

<br>

장점:

1. 개발자 입장에서 API를 보면 다음 상태가 어떻게 변화할 수 있는지 예측이 가능하다.
2. 다음 상태를 응답 받기위해 미리 URL을 저장하지 않고 href 값을 얻어와 호출할 수 있다.
3. 요청 URI가 변경되더라도 클라이언트에서 동적으로 생성된 URI를 사용함으로써, 클라이언트가 URI 수정에 따른 코드를 변경하지 않아도 되는 편리함을 제공한다(?)

<br>

## Uniform Interface 가 필요한 이유

독립적 진화

- 서버와 클라이언트가 각각 독립적으로 진화한다.

  → 서버의 기능이 바뀌었다. ex) 새로운 API가 생겨나고 바뀌고 등등 변경이 되었을떄 클라이언트는 변경될 필요가 없다.

  → 이것을 독립적 진화라 한다.

  → 즉, REST가 생겨나진 계기와 같다.(HTTP를 고치면 웹이 고장날것 같은데 어떻게 해야 이 문제를 해결하지?)

<br>

따라서 Uniform Interface가 만족이 되어야 REST라고 할 수 있다.

<br>

### 예시

- 웹 페이지가 변경되었을때 웹 브라우저를 업데이트할 필요가 없다.
  - 웹페이지의 이미지, 내용이 변경되어도 항상 사용하던 웹 브라우저에 들어가면된다.
- 웹 브라우저를 업데이트 했다고 해서 웹페이지가 오류가 난다거나 안들어가지지 않는다.
- HTTP 명세가 변경되어도 웹이 잘 동작한다(HTTP 2.0이 2014년도에 나와도 잘 동작한다)
- HTML 명세가 변경되어도 웹은 잘 동작한다.

<br>

만약 모바일앱을 업데이트 해야지 앱이 동작해야 한다면 그것은 REST가 잘지켜지지 않았다고 할 수 있다.

<br>

하지만 웹은 서버가 변경되어도 호환이 된다. REST가 잘 지켜짐(각각 독립적 진화)

<br>

## REST API는 REST를 잘 지키고 있을까?

- REST API는 REST 아키텍쳐 스타일을 따라야한다.
- 하지만 오늘날 스스로 REST API라고 하는 API 들의 대부분이 REST 아키텍쳐 스타일을 따르지 않는다.

<br>

### 원격 API가 꼭 REST API 여야 하는 건가?

반드시 지켜야하는것은 아니다.

<br>

- 클라이언트와 서버를 스스로 다 만들다던가 또는 클라이언트 또는 서버 개발자를 통제 가능하거나
- 해당 어플리케이션이 계속해서 업데이트(발전)을 할 필요가 없거나 안한다면

REST에 대해 따지느라 시간을 낭비하지 않아도 된다 라고 로이 필딩이 말했다.

<br>

**현재에는 실제로 REST를 지키지 않기 때문에 REST API가 아니지만 REST API 라고 부르고 있다.**

<br>

## REST API에 HATEOAS와 **self-descriptive messages을 지키기 어려운 이유**

<br>

### **self-descriptive**

self-descriptive을 지키기 위해서

1. 매번 media type을 정의 해준뒤
2. IANA에 미디어 타입을 매번 정의 해준뒤
3. 메세지를 통해 명세를 찾아가 메세지의 의미를 해석할 수 있어야한다.

<br>

이렇게 매번 media type을 정의 해주어야 한다는 점에 번거롭다.

<br>

또는 profile을 사용해서 Link 헤더에 명세를 링크해주는 방법이 있다.

이 또한 클라이언트가 Link와 profile을 이해해야한다는 단점이 있다.

<br>

### HATEOAS

<br>

1. 각각의 본문 body에 내용별로 link를 키로 URI를 값으로 설정한뒤 하이퍼 링크를 표현하는 방법이 있다.
   - 링크를 표현하는 방법을 직접 정의해야한다.
2. 또는 JSON으로 하이퍼링크를 표현하는 방법을 정의한 명세를 활용하는 방법
   - 기존 API를 많이 고쳐야한다.
3. Link, Location 등의 헤더로 링크를 표현한다.
   - 정의된 relation만 활용하면 표현에 한계가 있다.

<br>

## 정리

- 오늘날 대부분 REST API는 사실 REST를 따르지 않고 있다.
- REST 제약 조건중 특히 Self-descriptive와 HATEOAS를 잘 만족하지 못한다.
- REST는 긴 시간에 걸쳐 진화하는 웹 애플리케이션을 위한 것이다.
- REST를 따를 것인지는 API를 설계하는 이들이 스스로 판단하여 결정해야한다.
- REST를 따른다면 Self-descriptive와 HATEOAS를 만족시켜야한다.
  - Self-descriptive는 custon media type이나 profile link relation등으로 만족시킬 수 있다.
  - HATEOAS는 HTTP 헤더나 본문에 링크를 담아 만족시킬 수 있다.

<br>

궁금한점

- HATEOAS 이후에 URI를 변경하여도 클라이언트에서는 수정이 필요하지 않다라는 점이 있다고 하는데 URI가 변경되면 애초에 요청이 불가능한데 수정이 불필요하다라는게 무슨 말일까?

<br>

EndPoint란 클라이언츠가 접근가능한 URL을 말한다.

참고

- [https://www.youtube.com/watch?v=RP_f5dMoHFc](https://www.youtube.com/watch?v=RP_f5dMoHFc)

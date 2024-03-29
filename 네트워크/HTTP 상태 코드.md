# HTTP 상태코드

HTTP 상태코드는 크게 5가지 종류가 있다.

1. 1xx : 요청이 수신되어 처리중
2. 2xx : 요청 정상 처리
3. 3xx : 요청을 완료하려면 추가 행동이 필요
4. 4xx : 클라이언트 오류, 잘못된 문법등으로 서버가 요청을 수행할 수 없음
5. 5xx : 서버오류, 서버가 정상 요청을 처리하지 못함

<br>

HTTP 상태코드는 엄청나게 많지만 실무에서 쓰이거나 알아두면 괜찮은 상태코드를 알아보자

<br>

## 2xx 오류

### 200 OK

요청에대해 성공적으로 수행했음 의미한다.

<br>

### 201 Created

요청된 데이터에 의해서 새로운 리소스가 생성됨을 알려준다.

<br>

예를들어 클라이언트가 POST 메서드를 통해 데이터를 보내서 회원 가입을 했다면, 서버쪽에서 회원가입된 회원의 새로운 Location 헤더에 URI를 같이 보내준다.

<br>

즉, 자원이 성공적으로 생성되었음을 알려준다.

<br>

### 202 Accepted

요청이 접수되었으나 처리가 완료되지 않음

예를들어 요청 접수후 1시간뒤에 프로세스가 요청을 처리할때

<br>

### 204 No Content

서바가 성공적으로 수행했지만, 응답 페이로드 데이터가 없을때 사용

<br>

예를들어)

- 웹 문서 편집기에서 save 버튼을 클릭해서 해당 문서의 정보를 서버에 저장하기만 한다면
- 보낼 데이터가 없어도 204 메시지 만으로 성공을 인식할 수 있다.

<br>

※ 많은 상태코드가 있지만 모두 사용되지 않는다. 너무 많아서 팀원과 커뮤니케이션을 통해서 몇몇개만 지정해서 사용한다. 너무 많아지면 관리가 힘들어지기 때문이다. 따라서 200, 201이 주로 사용된다.

<br>

## 3xx Redirection

요청을 완료하기 위해 유저의 추가적 조치가 필요할때 사용

<br>

### 리다이렉션 이해

웹 브라우저는 3xx 응답에 Location 헤더가 있으면, 해당 Location 위치로 자동으로 이동한다.

<br>

웹 브라우저가 요청한 위치가 더 이상 사용하지 않는 위치라면 서버에서는 Location에 변경된 위치를 넣어주어서 응답해준다.

이후 브라우저는 3xx 응답을 받아 Location의 값을 바탕으로 자동으로 **Location의 위치로 요청을 다시 한다.**

<br>

종류

- 영구 리다이렉션
  - 특정 리소스의 URI가 영구적으로 이동함을 의미한다.
  - ex) /members → /users
  - ex) /event → /new-event
- 일시 리다이렉션
  - 일시적인 변경일때 사용한다. 예를들어 주문 완료 버튼을 눌러 서버에 요청한뒤, 주문 내역 화면으로 이동시켜줄때 사용한다.
  - PRG: Post/Redirect/Get

<br>

### 영구 리다이렉션

301, 308

- 리소스의 URI가 영구적으로 이동
- 원래 URL를 사용하지 않는다. 또한 검색 엔진 등에서도 변경을 인지한다.
- 301 Moved Permanently
  - 리다이렉트시 요청 메서드가 GET로 변하고, 본문이 제거될수있다.(May)
  - 이전페이지는 더이상 사용하지 않아서, Location의 위치로 다시 서버에 요청해서 정보를 받는다.
- 308
  - 301과 기능이 같다.
  - 리다이렉트시 요청 메서드와 본문을 유지한다.(처음 POST를 보내면 리다이렉트도 POST로 요청한다.)
  - 처음 보냈던 payload도 똑같이 보낸다. 단지 요청 위치가 다르다.
  - 하지만 스펙에는 있지만, 실무에서는 새로운 페이지로 바뀐다는 의미는 내부적 의미더ㅗ 다르므로 그냥 GET으로 요청하라고 한다.

<br>

### 일시적인 리다이렉션

302, 307, 303

리소스의 URI가 일시적으로 변경되었을때 사용한다.

따라서 검색 엔진 등에서 URL을 변경하면 안된다.

<br>

302 Found

- 리다이렉트시 요청 메서드가 GET으로 변하고, 본문이 제거 될 수 있음.(May)

303 see Other

- 302와 기능은 같다.
- 하지만 리다이렉트시 요청 메서드가 GET으로 무조건 변경되서 요청한다.(302는 GET으로 변경할지 모르는 거지만, 303은 확실함)

307 Temporary Redirect

- 리다이렉트시 요청 메서드와 본문을 유지한다.
- 처음 요청 메스드가 POST이고 payload가 있으면, 리다이렉션 요청도 POST와 payload가 그대로 간다.
- 단지 응답 받은 Location으로 주소가 변경된것 뿐이다.

<br>

**언제 사용될까?**

PRG : POST / Redirect / GET

<br>

POST로 주문후에 웹 브라우저를 새로고침한다면,

새로고침은 다시 요청하게 한다.

따라서 중복 주문이 될 수 있는 큰 문제가 발생한다.

<br>

잘못된 요청 과정

1. POST 요청(클)
2. DB에 요청한 아이템의 주문 상품이 담김
3. 클라이언트에게 200 OK 응답
4. 웹 브라우저 새로고침
5. POST 요청이 다시가게됨(클)
6. 또 다시 DB에 요청 아이템이 쌓임

<br>

따라서 POST로 주문후에 새로 고침으로 인한 중복 주문을 방지 해야한다.

클라이언트에서 주문을 할것인지에 대한 팝업창이 뜨는 것도 중복 방지가 되는 방법중 하나이다.

<br>

또한 서버에서 방지하는 방법중 하나는 POST로 주문후에 주문 결과 화면을 GET 메세드로 리다이렉트 하는 방법이 있다.

이렇게 된다면 새로고침을 해도 결과 화면을 GET으로 조회하기 때문에 중복 주문 대신에 결과 화면만 GET으로 다시 요청한다.

<br>

올바른 요청 과정

1. POST 요청(클)
2. DB에 요청한 아이템 주문 상품이 담김
3. 클라이언트에게 302 Found 상태 및 결과 화면 정보에 대한 URI를 Location에 담아서 응답함
4. 클라이언트는 3xx 요청이기 때문에 다시 GET으로 Location 위치에 요청함
5. 서버는 주문 완료 결과 화면 HTML을 응답함
6. 웹 브라우저에서 새로고침해도 GET요청을 다시 하므로 이상없음

<br>

### 기타 리다이렉션

304 Not Modified

- 캐시를 목적으로 사용한다.
- 클라이언트에게 리소스가 수정되지 않음을 알려준다. 따라서 클라이언트는 웹 브라우저에 저장된 캐시를 재사용한다(캐시로 리다이렉트 한다.)
- 304 응답은 응답에 메시지 바디를 포함하면 안된다.(로컬 캐시를 사용해야 하므로)

<br>

## 4xx 클라이언트 오류

클라이언트의 요청에 잘못된 문법등으로 서버가 요청을 수행할 수 없을때 발생하는 오류이다.

클라이언트가 이미 잘못된 요청, 데이터를 보내고 있기 때문에 똑같은 요청 재시도가 있어도 항상 오류가 난다.

<br>

### 400 Bad Request

클라이언트가 잘못된 요청을 해서 서버가 요청을 처리할 수 없다는 의미

예를들어 요청 구문, 메시지 등등 오류일때 발생

<br>

클라이언트는 요청 내용을 다시 검토하고 보내야한다.

- 요청 파라미터가 잘못되거나, API 스펙이 맞지 않을때

<br>

### 401 Unauthorized

클라이언트가 해당 리소스에 대한 인증이 필요한 경우

<br>

인증(Authentication)되지 않았을때 발생한다.

401 오류 발생시 응답에 WWW-Authenticate 헤더와 함께 인증 방법을 설명해서 보내주어야한다.

<br>

※인증(Authentication)이란 의미는 해당 사용자가 누구인지 확인하는 것을 의미한다.(로그인)

- 로그인이 되지 않았는데도 불구하고 로그인을 해야만 얻을 수 있는 정보를 요청한다면 401 에러가 발생한다.

※인가(Autorization)이란 권한을 부여하는 것을 말한다.

- 로그인과 상관없이 해당 정보에 접근하기위한 특별한 권한을 의미한다.

<br>

### 403 Forbidden

서버가 요청을 이해했지만 승인을 거부한것을 의미한다.

<br>

주로 인증(ex 로그인) 자격 증명은 있지만, 접근 권한이 불충분한 경우를 말한다.

- 어드민 등급이 아닌 사용자가 로그인은 했지만, 어드민 등급의 리소스에 접근하는 경우

<br>

### 404 Not Found

요청 리소스를 찾을 수 없는것을 의미한다.

<br>

요청 리소스가 서버에 없다면 해당 에러가 발생한다.

또는 클라이언트가 권한이 부족한 리소스에 접근한경우 해당 리소스를 숨기려고 할때도 사용한다.

- 403 상태코드대신 사용하려고 할때 사용한다는 의미이다.

<br>

## 5xx 서버 오류

서버 문제로 오류가 발생했을때를 의미한다.

서버에 문제가 있기 때문에 요청을 재시도 하면 성공할 수도 있다.(복구가 된다면)

<br>

### 500 Internal Server Error

서버 문제로 오류가 발생한것을 의미한다.

서버 문제의 오류인데 어떤 상태코드를 써야할지 애매할때 500 오류를 사용하면 된다.

<br>

### 503 Service Unavailable

서비스 이용 불가함을 의미한다.

서버가 **일시적인** 과부하 또는 예정된 작업으로 **잠시** 요청을 처리할 수 없을때 사용한다.

<br>

Retry-After 헤더 필드로 얼마뒤에 복구되는지 보낼 수도 있다.

<br>

참고

- 모든 개발자들을 위한 HTTP 웹 기본 지식 인프런 강의

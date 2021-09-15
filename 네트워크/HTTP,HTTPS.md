# HTTP / HTTPS

<br>

## HTTP

인터넷 상에서 클라이언트와 서버가 데이터를 주고 받을 때 쓰는 통신 규약이다.

<br>

HTTP는 암호화가 되지 않은 텍스트 데이터 교환 프로토콜 이므로, 누군가 네트워크에서 신호를 가로채면 내용이 노출되는 보안 이슈가 존재한다.

<br>

이런 보안 문제를 해결하기위한 프로토콜인 HTTPS가 등장했다.

<br>

## HTTPS

<br>

HTTP에 데이터 암호화가 추가된 프로토콜이다. HTTPS는 HTTP와 다르게 433번 포트를 사용하며, 네트워크 상에서 중간에 제3자가 정보를 볼 수 없도록 공개키 암호화를 지원하고 있다.

<br>

### 공개키, 개인키

HTTPS는 공개키/개인키 암호화 방식을 이용해 데이터를 암호화하고 있다.

<br>

공개키와 개인키는 서로를 위한 1쌍의 키이다.

- 공개키: 모두에게 공개가능한 키
- 개인키: 서버만 가지고 알고 있어야 하는 키

<br>

공개키 암호화: 공개키로 암호화를 하면 개인키로만 복호화할 수 있다. → 개인키는 서버만 가지고 있으므로 서버만 볼 수 있다.

개인키 암호화: 개인키로 암호화하면 공개키로만 복호화할 수 있다. -> 공개키는 모두에게 공개되어 있으므로, 서버가 인증한 정보임을 알려 신뢰성을 보장할 수 있다.

<br>

### HTTPS 동작 과정

<br>

데이터는 암호화되어 전송되기 때문에 임의의 사용자가 데이터를 조회하여도 원본의 데이터를 보는 것은 불가능하다.

<br>

따라서 서버는 클라이언츠가 사용할 공개키를 생성해서 주어야한다.

<br>

공개키를 만드는 방법은, 인증된 기관(CA, Certificate Authority) 에 공개키를 전송하여 인증서를 발급 받는다.

<br>

1. 서버에서 공개키 / 개인키를 발급한다.
2. CA기업에게 돈을 지불하고, 공개키를 저장하는 인증서의 발급을 요청한다.
3. CA 기업은 CA기업의 이름, 서버의 공개키, 서버의 정보등을 기반으로 인증서를 생성하고, CA기업의 개인키로 암호화하여 서버에게 인증서를 제공한다.
4. 서버는 클라이언트에게 암호화된 인증서를 준다.
5. 브라우저는 CA기업의 공개키를 미리 다운받아 갖고 있어서, 암호화된 인증서를 복호화 한다.
6. 암호화된 인증서를 복호화하여 얻는 서버의 공개키로 데이터를 데이터를 암호화하여 요청을 전송한다.

<br>

HTTPS는 이러한 공개키 / 개인키 기반의 대칭키 암호화 방식을 활용하여 안정성을 확보하고 있다.

<br>

## HTTP VS HTTPS

<br>

### HTTP

장점

- 암호화를 하지않아 속도가 빠르다.

<br>

단점

- HTTP는 암호화가 되어있지 않아서 보안에 취약하다.

<br>

### HTTPS

장점

- HTTPS는 안전하게 데이터를 주고받을 수 있다.

<br>

단점

- HTTPS를 이용하면 암호화 / 복호화의 과정이 필요하기 때문에 HTTP보다 속도가 느리다.(하지만 요즘은 속도가 더빠르기도 하다.)
- HTTPS는 인정서를 발급하고 유지하기위해 추가 비용이 발생한다.

<br>

따라서

민감한 데이터를 주고 받아야하면 HTTPS를 이용해야한다.(뉴스 기사)

하지만 단순 정보 조회만을 처리한다면 HTTP를 이용하면 된다.(인증, 전자상거래)

<br>

참고

- [https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Network#http와-https](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Network#http%EC%99%80-https)
- [https://jeong-pro.tistory.com/89](https://jeong-pro.tistory.com/89)
- [http://cryptocat.tistory.com/3](http://cryptocat.tistory.com/3)
- [https://mangkyu.tistory.com/98](https://mangkyu.tistory.com/98)
- [https://webactually.com/2018/11/16/http에서-https로-전환하기-위한-완벽-가이드/](https://webactually.com/2018/11/16/http%EC%97%90%EC%84%9C-https%EB%A1%9C-%EC%A0%84%ED%99%98%ED%95%98%EA%B8%B0-%EC%9C%84%ED%95%9C-%EC%99%84%EB%B2%BD-%EA%B0%80%EC%9D%B4%EB%93%9C/)

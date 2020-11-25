# MySQL 환경설정

<br>

데이터들을 가져오거나 넣을떄 사용하는 시스템이 필요하다.

그럴때 사용하는 툴들의 종류들이 RDMBS 같은 종류들이 있는 데이터 베이스들이 있다.

<br>

그중에서 MySQL이 RDBMS로서 사용하는 시스템이다.

<br>

**License**

MySQL을 포함하는 하드웨어나 소프트웨어 기타 장비를 판매하는 경우 라이센스 필요하다.
배포시 소스를 공개하면 무료이지만 소스공개를 원하지 않는 경우 사용라이센스 필요하다.
서비스에 이용하는건 무료로 가능하다.

<br>

가격이 싸고 무료여서

많은 회사들이 있다.

<br>

**하지만 오라클은**

오라클이 가격이 5000만가까이 될정도로

비싸지만 안정성, 기술력, 직원이 파견할정도로 대처해준다.

큰 금융권들이 사용한다.

<br>

**쿼리문이란**

데이터를 가져오거나 넣을때 사용하는 코드들을 쿼리문이라고 한다.

<br>

오라클이 MySQL을 인수했다.

때문에 오라클이 시장을 독점적으로 지배한다.

<br>

**데이터를 넣는 과정**

테이블안에 데이터를 넣는다.

<br>

하나의 서버를 하나의 컴퓨터에서만 관리하고 사용할수있다.

<br>

하지만 클라우드는 컴퓨터 공유를 해서 다수의 컴퓨터에서 사용할수있다.

<br>

**컴파일러 언어**

컴파일러가 0,1로 파싱한다.

파싱한 파일을 실행한다.

<br>

인터프리터가 한줄한줄 코드 해석하고 실행하는 과정보다 ****속도가 빠르다. **장점**

파싱하기전 과정이 필요하다. **단점**

<br>

**쿠키란 무엇일까?**

인터넷 사용자가 어떠한 웹사이트를 방문할 경우 

그 사이트가 사용하고 있는 서버를 통해 

인터넷 사용자의 컴퓨터에 설치되는 작은 기록 정보 파일을 말한다.

<br>

이 기록 파일에 담긴 정보는 인터넷 사용자가 같은 웹사이트를 방문할 때마다 

읽히고 수시로 새로운 정보로 바뀐다.

<br>

웹 브라우저에 의해 클라이언트 컴퓨터에 저장된다.

<br>

하드디스크에 저장

도메인 별로 각각 브라우저에 문자열 데이터로 저장을한다. ex) 구글, 네이버

<br>

**세션란 무엇일까?**

서버에 저장하는 객체 데이터, 브라우저 연결시 세션 ID 생성한다.

세션 ID을 쿠키에 저장함으로써 로그인 연결 유지

<br>

**캐쉬란 무엇일까?**

데이터를 빠르게 가져오기위해 브라우저의 렘에 담아 가져온다.

<br>

**세션**은 서버에서 사라지게 한다.

**캐쉬**는 컴퓨터가 끄면 사라진다.

**쿠키**는 브라우저에서 지울 수 있다.

<br>

**프레임워크란?**

라이브러리들이 모아져 있어

다양한 라이브러리들의 기능들을 가져와 사용할 수 있게된다.

<br>

**라이브러리?**

<br>

코드들을 모아 라이브러리로 미리 만들어 다른 코드에서도 쓰게한다.

22 → pc에서 서버쪽으로 명령을 내릴떄  ssh로 접근해서 명령하는데 사용하는 포트가 22이다. 

80 → 웹 http

3306 → mysql 서버

27017 → mongo DB 서버

<br>

키페어 이름, fds

키페어 다운로드

<br>

아이디 패스워드를 입력해야할때 키파일이 패스워드 형태를 해주어서

할수있게한다.

<br>

pem같이 중요한 파일은 따로 저장해준다.

<br>

![MySQL](../Images/MySQL%20환경설정/MySQL-1.jpg)

<br>

로컬에서 ssh로 명령어를 입력하면 원격으로 서버에 접속이 가능하다.

<br>

접속한후 사용하는 명령어는 서버에서 적용되는 명령어들이다.

앞으로 적용되는 명령어는 서버에서 원격으로 모두다 적용된다.

<br>

우분투 패키지 매니지를 받는다.

<br>

![MySQL](../Images/MySQL%20환경설정/MySQL-2.jpg)

<br>

ex) 우분투 패키지를 업데이트하는 명령어를 쓰면 서버에서 적용되고 로컬에서는 되지 않는다.

<br>

네트워크가 다르므로 인터넷망을 통해서 서버에 접근하므로 퍼블릭ip를 사용해야한다.

<br>

**MySQL Server 설치**

![MySQL](../Images/MySQL%20환경설정/MySQL-3.jpg)

<br>

**MySQL  설정**

![MySQL](../Images/MySQL%20환경설정/MySQL-4.jpg)

<br>

**설정값**

Would you like to setup VALIDATE PASSWORD plugin? N
New password: rada
Re-enter new password: rada
Remove anonymous users? Y
Disallow root login remotely? N

Remove test database and access to it? Y
Reload privilege tables now? Y

mysql> SELECT user,authentication_string,plugin,host FROM mysql.user;

→ 패스워드를 확인하는 문

<br>

**내부접속이 가능하게한다.**

<br>

![MySQL](../Images/MySQL%20환경설정/MySQL-5.jpg)

1. 비밀번호를 바꾼다.

<br>

**외부접속 설정**

다른 아이피에서 접근가능하게한다.(외부접속)

<br>

![MySQL](../Images/MySQL%20환경설정/MySQL-6.jpg)

0.0.0.0으로 하게하여 어디서든 접속할수있게한다.

<br>

특정 ip를하면 특정ip에서만 접속가능하다.

<br>

![MySQL](../Images/MySQL%20환경설정/MySQL-7.jpg)

호스트 네임에서 publicIP를 넣어준다.

store in valut에서 fds를 해준다.

<br>

workbench를 통해서 MySQL에 접근한다.

<br>

**public IP**

공유기가 가지고있는 IP가있다.

그 공유기를 사용하는 컴퓨터에서

다 같은 IP를 사용한다.

<br>

**private IP**

다같은 IP이므로 서로 구분하기 위한 것이 Private IP이다.

AWS의 퍼블릭 IP를 넣게되면 누구든지 접속가능하게된다.

<br>

SSH 명령어는 접속하는 명령어

SCP는 전송하는 명령어

<br>

scp /서버에 보내려고 할때 인증파일/ /보내려는 파일/ 받는 서버주소

<br>

![MySQL](../Images/MySQL%20환경설정/MySQL-8.jpg)

<br>

-i ~/.ssh/fds.pem 으로 보낼때 인증한다.

~/Downloads/sql/* 파일들을

ubuntu@PublicIP주소:~/에 보낸다.

<br>

명령어로 scp를 사용할때 서버쪽에 데이터를 빠르게 보낼수 있는 장점이 있다.

<br>

![MySQL](../Images/MySQL%20환경설정/MySQL-9.jpg)

<br>

**Ubuntu 들어가는법**

ssh -i ~/.ssh/fds.pem ubuntu@퍼블릭IP

<br>

**mySQL 들어가는법**

mysql -u root -pfds(비밀번호 입력)

또는 

mysql -u root -p 입력후 다음 코드에 비밀번호 입력

<br>

**데이터를 보내는 방법**

![MySQL](../Images/MySQL%20환경설정/MySQL-10.jpg)

mysql -u root -p (폴더이름) < (보내려는 데이터 파일)

다음줄은 비밀번호 입력

**또는**

mysql -u root -pfds(비밀번호) (폴더이름) < (보내려는 데이터 파일)
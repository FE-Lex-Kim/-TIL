# ICMP

<br>

## ICMP의 정의

Internet COntrol Message Protocol

인터넷 제어 메세지 프로토콜

<br>

IP 통신은 목적지에 패킷을 정상적으로 전달하는 방법은 있지만 에러 발생시 처리 불가

<br>

ICMP는 IP 통신의 에러 상황을 출발지에 전달하고 정보전달 역활을 한다.

<br>

ICMP의 패킷 형태는 여러가지가 있고 그중에 type은 ICMP 패킷의 종류를 의미한다.

## ICMP의 기능

<br>

## 정리

- IP 통신의 에러 상황을 출발지에 전달 또는 메시지 제어 역활
- 주 타입은 정보용(8,0,9,10)과 오류 보고용(3,5,11,12)으로 구분된다.

# DHCP

<br>

IP 할당방법에는 정적할당, 동적할당이 있다.

이중 DHCP는 동적할당에 해당한다.

<br>

동적할당 : LAN으로 연결된 컴퓨터의 대수가 많고 IP주소가 변경되어도 괜찮은 기기에 DHCP서버로 할당한다.

<br>

## DHCP정의

Dynamic Host Control Protocol

동적 호스트 구성 프로토콜

<br>

DHCP 서버를 사용하여 클라이언트인 네트워크 장치에 IP주소를 자동으로 할당

<br>

## DHCP 동작 과정

<br>

### IP할당

기본 네트워크 구성, Gateway - Switch - DHCP Server - PC

<br>

1. PC는 DHCP Server를 발견한다.
2. DHCP Server는 PC에게 IP를 제안한다.
3. PC는 제안 받은 IP 할달을 요쳥한다.
4. DHCP Server는 요청을 수락한다.

<br>

### IP 갱신

지정된 IP 갱신 타임이 도래하면 갱신을 요청한다.

1. PC는 기존 IP 재할당을 요청한다
2. DHCP Server는 IP 확인 후 요청 수락을 한다.

<br>

### IP 해제

사용중인 PC가 전원 off 되는 경우

1. PC는 더 이상 IP 할당이 필요없음을 알린다.

<br>

## 정리

- DHCP(Dynamic Host Control Protocol)는 IP를 할당하는 방법중 하나로 으로 동적할당이다.
- DHCP 서버를 사용하여 클라이언트인 네트워크 장치에 IP주소를 자동으로 할당한다.
- DHCP 동작은 IP할당, 갱신, 헤제의 과정이 있다.

<br>

참고

- 패스트캠퍼스 네트워크 강의
- [https://studyforus.tistory.com/137](https://studyforus.tistory.com/137)

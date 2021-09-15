# 스위치와 ARP

## L2 스위치

2계층의 대표적인 장비로 MAC 주소 기반 통신

<br>

허브의 단점 보안

Half duplex → Full duplex

<br>

라우팅 기능이 있는 스위치를 L3스위치라고도 부른다.

<br>

### 스위치 동작방식

목적지 주소를 MAC주소 테이블에서 확인하여 연결된 포트로 프레임 전달

<br>

1. Learning
   - 출발지 주소가 MAC 주소 테이블에 없다면 해당 주소를 저장
2. Flooding - Brodacasting
   - 목적지 주소가 MAC 주소 테이블에 없으면 전체 포트로 전달
3. Forwarding
   - 목적지 주소가 MAC 주소 테이블에 있으면 해당 포트로 전달
4. Filtering - collision Domain
   - 출발지와 목적지가 같은 네트워크 영역이면 다른 네트워크로 전달하지 않음
5. Aging
   - MAC 주소 테이블의 각 주소는 일정 시간 이후에 삭제

<br>

### Learning

<br>

PC는 스위치의 각 포트에 연결되고 프레임이 스위치에 전달한다.

스위치는 해당 포트로 유입된 프레임을 보고 MAC 주소를 테이블에 저장한다.

<br>

### Flooding

만약 PC1이 목적지인 PC5의 MAC주소로 프레임을 전달하려고 한다.

이때 스위치는 해당 주소가 MAC Table에 없어서 전체 포트로 전달하게 된다.

<br>

### Forwarding

PC1은 목적지 PC5 MAC주소로 프레임을 전달한다.

스위치는 해당 주소가 MAC Table에 존재하므로 해당 프레임을 PC5로 전달한다.

<br>

### Filtering

PC1은 목적지인 PC2주소로 프레임을 전달하려고한다.

스위치는 해당 주소가 동일 네트워크 영역임(같은 포트)을 확인해서 다른 포트로 전달하지 않는다.

<br>

필터링은 각 포트별 Collision Domain을 나누어 효율적 통신이 가능하다.

### Aging

스위치 MAC 주소 테이블은 시간이 지나면 삭제된다.

삭제되는 이유는 테이블 저장 공간을 효율적으로 사용하기 위해서 이다.

<br>

- CPU성능, 메모리 한계
- 해당 포트에 연결된 PC가 다른 포트로 옮겨진 경우도 발생한다.

기본 300초 저장, 다시 프레임이 발생되면 다시 카운트 된다.

## ARP(Address Resolution Protocol)

IP주소를 통해서 MAC주소를 알려주는 프로토콜

<br>

컴퓨터 A와 B가 IP통신을 시도하고 통신을 수행하기 위해 목적지 MAC주소를 알아야한다.

목적지 IP에 해당하는 MAC주소를 알려주는 역활을 ARP가 해준다.

<br>

<br>

### ARP 동작 과정

1. pc1은 목적지인 PC2에 IP로 패킷전송을 시도하기 위해 우선 ARP Cache Table을 확인한다.(기존에 통신을한 이력이 있으면 이곳에 IP주소에 대한 MAC주소가 저장되어있다.)
2. PC1이 PC2의 IP주소에 대한 ARP Request를 전송한다.(Flooding - Broadcasting)
   - PC2의 IP주소에 해당하는 MAC주소에 대해 요청을 하고 그에대한 MAC주소를 PC1의 IP에 돌려주는 것을 요청하는 작업
3. PC2 IP에서 목적지 MAC주소를 ARP Reply로 전달한다.
4. 목적지 MAC 주소는 ARP Cache Table에 저장되고 PC1은 패킷을 전송할 수 있게된다.

<br>

## 정리

- L2 스위치는 2계층의 대표적인 장비로 MAC주소 기반 통신한다.
- 스위치 동작방식은 5단계가 있다
  - Learning - Flooding - Forwarding - Filtering - Aging
- ARP는 IP주소를 통해서 MAC주소를 알려주는 프로토콜이다.

<br>

참고

- 패스트캠퍼스 네트워크 강의를 듣고 정리한 글입니다.

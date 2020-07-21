Collapsing margins 
===

![Collapsing margins](../Images/Collapsing%20margins.jpg)

CSS에서 두 개 또는 그 이상의 상자들(형제일 수도 있고 아닐 수도 있음)의 인접한 margin은 결합하여 하나의 마진을 만들 수 있다. 이렇게 결합하는 margin은 '병합' 한다고 하며, 그 결과 결합 마진은  'margin collaped'이라고 한다.

- A and B combine to form C : A와B를 결합해서 C를 생성한다. 
---
![Collapsing margins](../Images/Collapsing%20margins1.jpg)

수직으로 인접한 마진 병합, (다음 제외):

1. 루트요소(html) 상자의 margin 은 margin collapse 가 되지않는다.
2. 'clearance'가 있는 요소의 위쪽과 아래쪽 margin이 인접할때, 그 margin은 다음 형제자매의 인접 margin과 함께 collapse되지만, 그 결과 margin은 부모 블록의 아래쪽 margin과 함께 collapse되지 않는다.




- A if and only if B : '만약 A가 참이라면 B도 참이고 또한 만약 B가 참이라면 A도 참이다'
- Root element란 가장 상위 단계 부모요소인 html 이다.




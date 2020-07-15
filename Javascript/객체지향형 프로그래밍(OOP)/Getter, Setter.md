Getter, Setter
===

Getter,Setter는 객체 지향 프로그래밍에서 객체가 가진 프로퍼티 값을 객체 바깥에서 읽거나 쓸 수 있도록 제공하는 메서드를 말한다. 객체의 프로퍼티를 객체 바깥에서 직접 조작하는 행위는 데이터의 유지 보수성을 해치는 주요한 원인이다.

```js
class Person{
  constructor(name){
    this.name = name;
  }
  
  greeting(){
    console.log(`Hi i'm ${this.name}`);
  }

}

class Teacher extends Person{
  constructor(name,age,subject){
    super(name);
    this._age = age;
    this._subject = subject;
  }

  get age() {
    return this._age;
  }

  set age(newAge){
    this._age = newAge;
  }
}
```
> 변수에 _ 언더바를 넣어서 구분하여 getter와 setter할때 에러가 나지 않게한다.


>접근자를 쓰는 이유
 "객체 지향"에서 얘기하는 '캡슐화(encapsulation)'를 달성하기 위함이다.
  캡슐화란 서로 관련있는 데이터와 그 데이터를 다루는메서드를 하나의 클래스로 묶는 것을 의미하는데, 캡슐화의 가장 큰 장점은 다른 객체에게 자신의 정보를 숨기고, 오직 연산만을 통해서 접근할 수 있도록 하는 '정보 은닉(Information Hiding)'이 가능하다는 것이다.

```
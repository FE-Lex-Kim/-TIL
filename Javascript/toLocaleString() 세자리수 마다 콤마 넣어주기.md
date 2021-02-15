# toLocaleString() 세자리수 마다 콤마 넣어주기

<br>

number타입의 숫자뒤에 메서드로 toLocaleString()에 인수를 넣지 않으면 String으로 타입 변환한뒤 콤마(,)가 세자리 수마다 찍히게 된다.

<br>

```jsx
20000.toLocaleString() // 20,000
40000.toLocaleString() // 40,000
```

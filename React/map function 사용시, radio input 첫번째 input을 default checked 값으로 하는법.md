# map function 사용시, radio input 첫번째 input을 default checked 값으로 하는법

<br>

map 메소드를 통해 input radio 버튼이 여러개 생성했을시,

<br>

그중 첫번째 radio만 chcked 하는방법

<br>

```jsx
<ul className="pb-8 text-center">
  {suggestType.map((type, i) => (
    <li key={i} className="inline-block px-2 pb-8">
      <input
        defaultChecked={i === 0}
        className="hidden"
        id={`${i}`}
        type="radio"
        name="md"
      />
      <label
        onClick={() => {
          dispatch(getMdSuggestionGoodsInfo(i));
          setMdCurIndex(type);
        }}
        className="cursor-pointer focused-sibling:text-white focused-sibling:font-bold focused-sibling:bg-kp-600 inline-block h-16 px-8 pb-4 pt-4 rounded-r-2 bg-kg-500  text text-r-1.4 leading-6"
        htmlFor={`${i}`}
      >
        {type}
      </label>
    </li>
  ))}
</ul>
```

<br>

defaultChecked props를 사용하여 index가 0번째인경우에 조건식으로 true값을 만들어 checked 값으로 만든다

<br>

참고

<br>

- [https://stackoverflow.com/questions/56211588/how-to-set-a-defaultchecked-to-only-first-input-in-radio-button-when-using-map-f](https://stackoverflow.com/questions/56211588/how-to-set-a-defaultchecked-to-only-first-input-in-radio-button-when-using-map-f)

# img 태그 이미지가 없을경우 대체이미지 생성

<br>

img태그에서 이미지의 정보를 src로 불러왔을때, 만약 src주소의 img가 없다면 빈 이미지가 들어가게된다.

<br>

만약 img태그에 src의 이미지 주소가 없다면 에러가 발생하므로

<br>

그에따라 onError 이벤트핸들러에 `e.target.src`인 img 요소의 src에 대체이미지 주소를 넣어주면된다.

<br>

```jsx
onError={e => (e.target.src = '/img/commingsoonresize.png')}
```

<br>

<img src='../Images/img태그%20대체이미지/img태그대체이미지-1.png' width='300' height='300'>

<img src='../Images/img태그%20대체이미지/img태그대체이미지-2.png' width='300' height='300'>

<br>

참고

- [https://sub0709.tistory.com/126](https://sub0709.tistory.com/126)
- [https://stackoverflow.com/questions/34097560/react-js-replace-img-src-onerror](https://stackoverflow.com/questions/34097560/react-js-replace-img-src-onerror)

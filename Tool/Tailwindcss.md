# Tailwindcss

<br>

Tailwindcss 는 utility-first CSS 프레임 워크이다.

<br>

utility-first CSS는 미리 정해지어진 css를 바탕으로 클래스를 통해 css를 적용하는 방식이다.

<br>

😁**class 이름 형태** 

```jsx
.{property}{side}-{size}
```

<br>

예제)

<br>

`text-gray-50` : 텍스트의 컬러를 gray색으로 50~500중 50정도의 컬러를 적용한다.

`p-14`: padding을 전체적으로 14(3.5rem)를 준다.

`py-48` : padding의 y좌표인 위와 아래를 48(12rem)씩 준다.

`pr-80` : padding 의 오른쪽만 80(20rem) 정도 준다.

<br>

### Tailwindcss 설치

<br>

😁**설치**

```bash
npm install tailwindcss@npm:@tailwindcss/postcss7-compat @tailwindcss/postcss7-compat postcss@^7 autoprefixer@^9
```

<br>

😁**Install and configure CRACO**

```bash
npm install @craco/craco
```

CRACO를 설치해주어야 Tailwindcss의 환경설정이 적용이 가능해준다.

<br>

설치후에 `package.json` 파일에서 scripts 파일을 업데이트 해준다.

craco를 사용하기위해서 `scripts` 에서 eject만 제외하고 다 바꾸어준다.

```json
"scripts": {
    "start": "craco start",
    "build": "craco build",
    "test": "craco test",
    "eject": "react-scripts eject"
},
```

<br>

`craco.config.js` 파일을 프로젝트의 루트에서 파일을 생성해준뒤 tailwindcss와 autoprefixer을 플러그인으로 추가해준다.

```jsx
// craco.config.js
module.exports = {
  style: {
    postcss: {
      plugins: [
        require('tailwindcss'),
        require('autoprefixer'),
      ],
    },
  },
}
```

<br>

😁**configuration file**

```bash
npx tailwindcss init
```

Tailwindcss 설정파일(tailwind.config.js)을 생성해준다.

<br>

😁TailwindCss 적용하기

```css
/* ./src/index.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

src/index.css 파일에 위의 세개의 css을 import 해준다.

<br>

## 사용하기

<br>

css파일에서 클래스에 css을 적용시킨뒤 원하는 태그에 클래스를 부여해서 스타일을 입혀준다.

<br>

마찬가지로 tailwindcss가 이미 생성해놓은 css 클래스를 바탕으로 원하는 태그에 클래스를 넣어 css을 적용하는 방식이다.

<br>

ex) width와 heigth을 조정할때 태그에 클래스를 넣어주는 것만으로도 적용된다.

```jsx
<div className='w-12, h-12'></div> 
```

<br>

실제 css를 tailwindcdd와 네이밍이 다르기 때문에 처음에 tailwindcss을 사용하는데에 있어 힘들다는 점이있다.

<br>

따라서 tailwindcss의 클래스를 다 외울수 없으니 찾아가면서 적용시켜야한다.

<br>

[Tailwindcss 공식홈페이지](https://tailwindcss.com/)에서 검색하면 바로 알 수 있으니 적극적으로 활용해야한다.

![Tailwindcss](../Images/Tailwindcss/Tailwindcss-1.png)

<br>

## Tailwindcss **장점**

<br>

- 더이상 클래스 이름을 네이밍하는데 시간을 쏟지 않아도된다. 추상적인 이름의 네이밍을 하지 않아도 된다는 점은 생산성을 향상시켜준다.
- 우리가 사용했던 전통적인 방식의 css파일은 우리가 새로운 css을 적용시킬 때마다 점점 용량이 커진다. 하지만 tailwindcss는 재사용하기 걱정하지 않아도된다.
- css는 글로벌로 적용된다. 그리고 css를 적용하는데에 있어 클래스 네이밍이 겹친다고 했을때 오버라이딩 된다는 점을 걱정해야 할지도 모른다. 하지만 tailwindcss는 이미 만들어진 클래스를 바탕으로 적용하여 걱정이 없다.

<br>

tailwindcss는 굉장한 자부심이 있는것같다.

만약 tailwindcss을 사용하면 이미 적용되어있는 class을 바탕으로 작업을 하기 때문에 기존의 css 스타일링이 고문처럼 고통스럽게 느껴질거라고 했다.

<br>

## 인라인 스타일을 사용하지 않는이유

<br>

### 1. 가독성이 좋지 않은 css value

직접 인라인 스타일로 적용할때,  `color: #012739` 와 같이 css값은 어렵게 느껴진다. tailwindcss는 이미 정의되어진 디자인 시스템을 바탕으로 쉽게 UI를 작성할 수 있게 한다. ex) text-gray-50

<br>

### 2. 반응형 디자인

우리가 만약 미디어 쿼리로 인라인 스타일을 적용하려고 할때 인라인스타일로는 적용 할 수 없다.

tailwindcss는 반응형 유틸리티가 있어 굉장히 쉽게 반응형 디자인을 사용하여 만들 수 있다.

[반응형 디자인의 자세한 정보](https://tailwindcss.com/docs/responsive-design)

<br>

### 3. Hover, focus..

인라인 스타일로는 그 대상에 hover, foucs등등 지정해줄 수 없다. 따로 그 클래스를 :hover 와 같이 지정해주어야한다.

tailwindcss는 쉽게 적용가능하다.

<br>

다음은 Tailwindcssd의 예제 코드이다. 

쉽게 반응형 스타일과 hover,focuse를 적용한것을 쉽게 알 수 있고 적용할 수 있음을 보여주는 코드이다.

<br>

**예제코드)**

```jsx
<div class="py-8 px-8 max-w-sm mx-auto bg-white rounded-xl shadow-md space-y-2 sm:py-4 sm:flex sm:items-center sm:space-y-0 sm:space-x-6">
  <img class="block mx-auto h-24 rounded-full sm:mx-0 sm:flex-shrink-0" src="/img/erin-lindford.jpg" alt="Woman's Face">
  <div class="text-center space-y-2 sm:text-left">
    <div class="space-y-0.5">
      <p class="text-lg text-black font-semibold">
        Erin Lindford
      </p>
      <p class="text-gray-500 font-medium">
        Product Engineer
      </p>
    </div>
    <button class="px-4 py-1 text-sm text-purple-600 font-semibold rounded-full border border-purple-200 hover:text-white hover:bg-purple-600 hover:border-transparent focus:outline-none focus:ring-2 focus:ring-purple-600 focus:ring-offset-2">Message</button>
  </div>
</div>
```

<br>

참고

- [https://so-so.dev/web/tailwindcss-w-twin-macro-emotion/](https://so-so.dev/web/tailwindcss-w-twin-macro-emotion/)
- Tailwindcss 공식홈페이지 : [https://tailwindcss.com](https://tailwindcss.com/docs/installation)
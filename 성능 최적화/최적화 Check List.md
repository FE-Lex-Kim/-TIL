# 최적화 Check List

<br>

## 로딩 성능 최적화

- [ ] **적절한 이미지 크기를 가져왔는지?**
  - ex) 원래 사이즈 1200 _ 1200 px 인데, 브라우저에서는 120 _ 120 px로 사용
  - 2배 정도의 크기를 가져오는 것이 좋다.
  - **[적절한 이미지 크기 가져오는 방법](https://github.com/FE-Lex-Kim/-TIL/blob/master/%EC%84%B1%EB%8A%A5%20%EC%B5%9C%EC%A0%81%ED%99%94/%EC%A0%81%EC%A0%88%ED%95%9C%20%EC%9D%B4%EB%AF%B8%EC%A7%80%20%ED%81%AC%EA%B8%B0%20%EB%A1%9C%EB%93%9C.md)**
- [ ] **텍스트의 사이즈가 2kb 이상인 리소스를 gzip 압축 했는지?**
  - gzip을 사용하면 브라우저는 클라이언트 서버에서 리소스를 다운로드 하는 속도를 줄여 응답속도가 빠르다는 장점이 있다.
  - **[텍스트 압축 방법](https://github.com/FE-Lex-Kim/-TIL/blob/master/%EC%84%B1%EB%8A%A5%20%EC%B5%9C%EC%A0%81%ED%99%94/Text%20Compression%20%ED%85%8D%EC%8A%A4%ED%8A%B8%20%EC%95%95%EC%B6%95.md)**
- [ ] **최적화된 이미지 포맷을 사용했는지?**
  - **[이미지 포맷 변경 방법](https://github.com/FE-Lex-Kim/-TIL/blob/master/%EC%84%B1%EB%8A%A5%20%EC%B5%9C%EC%A0%81%ED%99%94/%EC%9D%B4%EB%AF%B8%EC%A7%80%20%EC%82%AC%EC%9D%B4%EC%A6%88%20%EC%B5%9C%EC%A0%81%ED%99%94.md)**
- [ ] **웹 폰트 사이즈를 최적화해서 로드했는지?**
  - **[웹 폰트 사이즈 최적화 방법](https://github.com/FE-Lex-Kim/-TIL/blob/master/%EC%84%B1%EB%8A%A5%20%EC%B5%9C%EC%A0%81%ED%99%94/%ED%8F%B0%ED%8A%B8%20%EC%82%AC%EC%9D%B4%EC%A6%88%20%EC%B5%9C%EC%A0%81%ED%99%94.md)**

<br>

## 렌더링 성능 최적화

- [ ] **code splitting을 통한 최초 렌더링 성능을 향상 시켰는지?**
  - 첫 페이지에 진입시에만 필요한 코드를 받고, 다른 페이지에 진입시 그때 코드를 받게 하는 방법을 쓰면 성능이 향상된다.
  - **[code splitting 방법](https://github.com/FE-Lex-Kim/-TIL/blob/master/%EC%84%B1%EB%8A%A5%20%EC%B5%9C%EC%A0%81%ED%99%94/code%20splitting%2C%20Lazy%20loading.md)**
- [ ] **다른 페이지로 넘어가기 전에 미리 페이지(컴포넌트)를 가져왔는지?**
  - 다른 페이지로 넘어가기전에, 미리 페이지를 로드해놓아서 로드하는 시간을 없앤다.
  - **[Preloading 컴포넌트 방법](https://github.com/FE-Lex-Kim/-TIL/blob/master/%EC%84%B1%EB%8A%A5%20%EC%B5%9C%EC%A0%81%ED%99%94/%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%20Preloading.md)**
- [ ] **이미지 리소스를 Preload 해서 캐시에 저장했는지?**
  - **이미지를 미리 불러온뒤, 브라우저 캐쉬에 저장해서** 이미지를 사용하는 페이지에서 캐싱된 이미지를 사용한다.
  - **[이미지 Preloading 방법](https://github.com/FE-Lex-Kim/-TIL/blob/master/%EC%84%B1%EB%8A%A5%20%EC%B5%9C%EC%A0%81%ED%99%94/%EC%9D%B4%EB%AF%B8%EC%A7%80%20preloading.md)**
- [ ] **현재 뷰포트에 보여지지 않는 이미지를 나중에 로드해서, 웹 페이지 최초 로딩 속도를 향상 시켰는지?**
  - 스크롤해서 보여지는 부분의 이미지들은 **스크롤해서 이미지가 보여질때 로드한다.**
  - **[이미지 지연 로딩1](<https://github.com/FE-Lex-Kim/-TIL/blob/master/%EC%84%B1%EB%8A%A5%20%EC%B5%9C%EC%A0%81%ED%99%94/%EC%9D%B4%EB%AF%B8%EC%A7%80%20%EC%A7%80%EC%97%B0%20%EB%A1%9C%EB%94%A9(Image%20lazy%20loading)%20with%20intersection%20observer.md>)**
  - **[이미지 지연 로딩2](<https://github.com/FE-Lex-Kim/-TIL/blob/master/%EC%84%B1%EB%8A%A5%20%EC%B5%9C%EC%A0%81%ED%99%94/%EC%9D%B4%EB%AF%B8%EC%A7%80%20%EC%A7%80%EC%97%B0%20%EB%A1%9C%EB%94%A9(Image%20lazy%20loading)%20with%20react-lazyload%2C%20Native%20Lazy%20loading.md>)**
- [ ] **웹 폰트 적용 시점을 올바르게 설정해서 사용자 경험을 올렸는지?**
  - 폰트를 로드 시간이 길어져 브라우저에 폰트 적용 시점을 설정해서 사용자 경험을 향상 시킨다.
  - [**폰트 적용 시점 방법1**](https://github.com/FE-Lex-Kim/-TIL/blob/master/%EC%84%B1%EB%8A%A5%20%EC%B5%9C%EC%A0%81%ED%99%94/%ED%8F%B0%ED%8A%B8%20%EB%A0%8C%EB%8D%94%EB%A7%81%20%EC%B5%9C%EC%A0%81%ED%99%94.md)
  - [**폰트 적용 시점 방법2**](https://github.com/FE-Lex-Kim/-TIL/blob/master/%EC%84%B1%EB%8A%A5%20%EC%B5%9C%EC%A0%81%ED%99%94/%ED%8F%B0%ED%8A%B8%20Preload.md)
- [ ] **Layout Shift를 방지 했는지?**
  - 브라우저가 렌더링하는 동안 **예기치 않게 레이아웃이 화면상에서 이동하는 것을 방지한다.**
  - **[Layout Shift 방지 방법](https://github.com/FE-Lex-Kim/-TIL/blob/master/%EC%84%B1%EB%8A%A5%20%EC%B5%9C%EC%A0%81%ED%99%94/Layout%20Shift.md)**
- [ ] **복잡한 로직을 메모이제이션 기법을 사용했는지?**
  - **빈번하게 발생되는 복잡한 로직일때, 메모이제이션 기법으로 캐싱을 하는 방법을 사용하면 렌더링 성능에 향상된다.**
  - **[memoization 방법](https://github.com/FE-Lex-Kim/-TIL/blob/master/%EC%84%B1%EB%8A%A5%20%EC%B5%9C%EC%A0%81%ED%99%94/memoization%20%EB%A0%8C%EB%8D%94%EB%A7%81%20%EC%84%B1%EB%8A%A5%20%ED%96%A5%EC%83%81.md)**

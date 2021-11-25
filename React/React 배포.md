# React 배포

이번에 사이드 프로젝트를 시작하면서 제대로된 서비스를 런칭해보려고 했다.

커스텀 도메인과 https 등등 여러 기능과 함께 **실제 서비스를 만드는 과정에 대해 알아보자.**

<br>

## build

프로젝트의 개발이 모두 완성되고 배포하려고 할때, **가장 먼저할 작업은 build를 해야한다.**

빌드를 통해 Webpack + Babel을 진행한다.

<br>

React

```bash
$ npm run build
```

<br>

빌드가 진행되고 build 폴더 내부에 배포 버전으로 파일들이 만들어진다.

폴더 내부에서는 **사이트에 필요한 파일들만 모아둔 배포 버전들이 있다**.

<br>

웹 서비스에서 여러 파일들을 **여러번 로드한다면 비효율적이다.**

**그래서 여러 모듈, 사용했던 라이브러리를 하나의 파일로 만들어서 로드하게 한다.**

<br>

코드를 수정해서 공백을 다 없애고 자동으로 압축시켜놓아서 **난독화 작업을 통해 코드의 유출을 막기도한다.**

## AWS s3

**데이터를 저장하는 객체 스토리지이다.**

퍼블릭하게 열려있어서 누구든지 접근이 가능하다는 단점이 있다.

<br>

## CloudFront

**CloudFront는 AWS에서 제공하는 CDN 서비스 이다.**

<br>

**캐싱을 통해 사용자에게 좀 더 빠른 전송 속도를 제공한다.**

**또한 여러가지 기능들이 있어서 AWS s3와 같이 사용하는 것이 좋다.**

<br>
그 이유는

- **퍼블릭하게 엑세스가 허용되어있는 것을 막아준다.**
- AWS s3는 http만 지원해서 보안에 취약하지만, **cloudFront는 HTTPS(SSL)이 적용가능하다.**
- AWS s3는 **도메인 설정이 불가능하다.**
- **캐싱을 통해 로딩 성능이 향상된다.**
- S3 직접 공유보다 **저렴하다.**

... 아직 작성중

<br>

참고

- 패스트 캠퍼스 개발 초격차 강의
- [https://docs.aws.amazon.com/ko_kr/AmazonS3/latest/userguide/Welcome.html](https://docs.aws.amazon.com/ko_kr/AmazonS3/latest/userguide/Welcome.html)
- [https://www.youtube.com/watch?v=h_uOyr9FZm4](https://www.youtube.com/watch?v=h_uOyr9FZm4)
- [https://velog.io/@\_junukim/CloudFront로-React앱-배포하기](https://velog.io/@_junukim/CloudFront%EB%A1%9C-React%EC%95%B1-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0)
- [https://velog.io/@seongkyun/AWS-S3-CloudFront-Route53을-이용한-정적-호스팅](https://velog.io/@seongkyun/AWS-S3-CloudFront-Route53%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EC%A0%95%EC%A0%81-%ED%98%B8%EC%8A%A4%ED%8C%85)
- [https://velog.io/@noyo0123/React로-개발한-클라이언트-앱-HTTPS로-배포하고-사용자-인증까지-4yk1wxaobk#로컬에서-개발해서-배포했을때-발생한-문제](https://velog.io/@noyo0123/React%EB%A1%9C-%EA%B0%9C%EB%B0%9C%ED%95%9C-%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8-%EC%95%B1-HTTPS%EB%A1%9C-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B3%A0-%EC%82%AC%EC%9A%A9%EC%9E%90-%EC%9D%B8%EC%A6%9D%EA%B9%8C%EC%A7%80-4yk1wxaobk#%EB%A1%9C%EC%BB%AC%EC%97%90%EC%84%9C-%EA%B0%9C%EB%B0%9C%ED%95%B4%EC%84%9C-%EB%B0%B0%ED%8F%AC%ED%96%88%EC%9D%84%EB%95%8C-%EB%B0%9C%EC%83%9D%ED%95%9C-%EB%AC%B8%EC%A0%9C)
- [https://real-dongsoo7.tistory.com/86](https://real-dongsoo7.tistory.com/86)

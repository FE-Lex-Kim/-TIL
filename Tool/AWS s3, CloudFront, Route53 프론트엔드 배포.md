# AWS s3, CloudFront, Route53 프론트엔드 배포

이번에 사이드 프로젝트를 시작하면서 제대로된 서비스를 런칭해보려고 했다.

커스텀 도메인과 https 등등 여러 기능과 함께 **실제 서비스를 만드는 과정에 대해 알아보자.(각 배포 과정별로 자세한 사용법보다 흐름을 먼저 알아보고 이후에 사용법에 대해 공부해보자.)**

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

<br>

## AWS s3

매번 로컬 **PC를 24시간 켜놓고 사용자들에게 리소스를 줄 순 없다.**

리소스를 저장해두고 **사용자에게 리소스를 전달해줄 클라우드 스토리지가 필요하다.**

<br>

AWS s3는 **데이터를 저장하는 객체 스토리지이다.**

사용자는 **거의 모든 유형의 데이터(HTML, CSS, JS,이미지.. 등등)를 클라우드에 저장가능하다.**

<br>

S3에는 **버킷이라는 것을 생성해야한다.**

버킷에 **데이터를 저장하고 검색하는데** 사용되는 컨테이너라고 생각하면된다.

<br>

## CloudFront

**CloudFront는 AWS에서 제공하는 CDN 서비스 이다.**

**CloudFront는 AWS s3로 오는 네트워크 트래픽이 지나는 통로를 지킨다.**

<br>

![AWS s3, CloudFront, Route53 프론트엔드 배포](../Images/AWS%20s3,%20CloudFront,%20Route53%20프론트엔드%20배포/AWS%20s3,%20CloudFront,%20Route53%20프론트엔드%20배포-1.png)

<br>

웹 사이트를 **엣지 서버에서 호스팅해 유저가** 웹 사이트를 빠르게 로드 할 수 있도록 도와준다.

※ 엣지 서버 : 사용자와 가장 가까이 있는 서버

- 예를 들어) **서울에 origin 서버가 있다. 만약 미국에 있는 사용자가 서비스를 접근하려고 할때**, 서울까지 네트워크를 요청하면 시간이 **오래걸린다**. **미국에 있는 엣지 서버를 사용해서** 해당 하는 리소스가 있으면 그대로 사용해 **리소스 요청 시간을 줄인다.** 만약 **없다면 origin 서버에서 받아온** **다음, 미국의 엣지 서버에 저장한다.** 이후 **같은 요청을 하게 되면 미국의 엣지 서버에서 리소스를 가져온다.**

<br>

이렇게 **캐싱을 통해 사용자에게 좀 더 빠른 전송 속도를 제공한다.**

또한 이것 뿐만 아니라 **여러 기능이 있어서** AWS s3 + CloudFront을 사용한다.

<br>

**같이 사용하는 이유에 대해 알아보자.**

- AWS s3는 http만 지원해서 보안에 취약하지만, **cloudFront는 HTTPS(SSL)이 적용가능하다.**
- AWS s3는 **커스텀** **도메인 설정이 불가능하다.**
- 가까운 엣지 서버를 사용해 **캐시되어 있으면 사용하기 때문에 로딩 성능이 향상된다.**
- S3 직접 공유보다 **저렴하다.**
- origin 서버로 요청 시간이 길어지며, 시간이 지연되면 **요청이 반환되어 오류가 뜰 수 있다.**

<br>

## Route53

지금까지 **AWS s3, CloudFront을 통해서 리소스를 저장하고 캐싱 서버를 만들었다.**

<br>

**사용자가 Custom Domain에 접속했을때**, 2가지 경우로 나뉜다.

1. **이전에 방문한 사이트면,** **CloudFront에 저장된 리소스를 가져온다.**
2. **이전에 방문한 적 없던 사이트면**, CloudFront에 리소스가 캐싱되지 않은 것을 확인후, **Origin 서버 즉, AWS s3에 요청해서 리소스를 로드한다.**

<br>

따라서 **Custom Domain과 CloudFront와 연결해야한다.**

그 역활을 **Route53이 한다.**

**Custom Domain과 CloudFront을 연결해주는 역활을 해준다.**

<br>

**Route53은 AWS에서 제공하는 DNS 서비스 이다.**

DNS 서비스는 Domain Name System이다.

**Name Server을 사용해 CloudFront와 연결한다는 의미이다.**

※ **Name Server을 모른다면, 애니메이션으로** 잘 설명한 동영상 하나를 보고오는 것을 **강추한다.**

<iframe width="950" height="534" src="https://www.youtube.com/embed/2ZUxoi7YNgs" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<br>

Route53에 **CloudFront의 Domain을 등록하고 Custom Domain을 등록해준다.**

<br>

## Custom Domain

마찬가지로

**Custom Domain을 제공해주는 업체에는 Name Server을 가지고 있다.**

<br>

**Custom Domain Name Server을 통해 Route53의 Name Server을 간다음 CloudFront의 캐싱된 리소스를 가져와야한다.**

**즉, Custom Domain(Name Server) → Route53(Name Server) → CloudFront(CDN)**

<br>

**Route53으로 갈 수 있게 Custom Domain Name Server에 Route53의 Name Server을 등록해준다.**

<br>

![AWS s3, CloudFront, Route53 프론트엔드 배포](../Images/AWS%20s3,%20CloudFront,%20Route53%20프론트엔드%20배포/AWS%20s3,%20CloudFront,%20Route53%20프론트엔드%20배포-2.png)

<br>

참고

- [https://aws.amazon.com/ko/s3/](https://aws.amazon.com/ko/s3/)
- [https://docs.aws.amazon.com/ko_kr/AmazonCloudFront/latest/DeveloperGuide/CNAMEs.html#CreatingCNAME](https://docs.aws.amazon.com/ko_kr/AmazonCloudFront/latest/DeveloperGuide/CNAMEs.html#CreatingCNAME)
- [https://opentutorials.org/course/608/3012](https://opentutorials.org/course/608/3012)
- [https://www.youtube.com/watch?v=2ZUxoi7YNgs&t=1s](https://www.youtube.com/watch?v=2ZUxoi7YNgs&t=1s)
- [https://velog.io/@\_junukim/CloudFront로-React앱-배포하기](https://velog.io/@_junukim/CloudFront%EB%A1%9C-React%EC%95%B1-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0)
- [https://velog.io/@seongkyun/AWS-S3-CloudFront-Route53을-이용한-정적-호스팅](https://velog.io/@seongkyun/AWS-S3-CloudFront-Route53%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EC%A0%95%EC%A0%81-%ED%98%B8%EC%8A%A4%ED%8C%85)
- [https://velog.io/@noyo0123/React로-개발한-클라이언트-앱-HTTPS로-배포하고-사용자-인증까지-4yk1wxaobk#로컬에서-개발해서-배포했을때-발생한-문제](https://velog.io/@noyo0123/React%EB%A1%9C-%EA%B0%9C%EB%B0%9C%ED%95%9C-%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8-%EC%95%B1-HTTPS%EB%A1%9C-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B3%A0-%EC%82%AC%EC%9A%A9%EC%9E%90-%EC%9D%B8%EC%A6%9D%EA%B9%8C%EC%A7%80-4yk1wxaobk#%EB%A1%9C%EC%BB%AC%EC%97%90%EC%84%9C-%EA%B0%9C%EB%B0%9C%ED%95%B4%EC%84%9C-%EB%B0%B0%ED%8F%AC%ED%96%88%EC%9D%84%EB%95%8C-%EB%B0%9C%EC%83%9D%ED%95%9C-%EB%AC%B8%EC%A0%9C)

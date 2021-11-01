# Text Compression 텍스트 압축

<br>

로드할때 성능 최적화 하는 여러가지 방법이 있다.

1. code splitting
2. text compression

<br>

이전에 code splitting을 공부했고
**text compression(텍스트 압축)은 문서 사이즈를 줄여주는 방법이다.**

<br>

브라우저에서 페이지를 요청하면,

클라이언트 서버에서 JS, CSS, Img 등등 과 같은 리소스를 압축해서 보내준다.

그렇게 되면 다운로드하는 리소스의 사이즈가 작아지게 되니깐 성능이 좋아진다.

<br>

실제 서비스되는 페이지가 텍스트 압축이 되는지 확인해보자.

<br>

## 텍스트 압축 프로세스

### gzip 사용전

1. 브라우저가 클라이언트 서버측에 리소스를 요청한다.
2. 서버는 Request를 받는다.
3. Response에 요청한 내용을 담아 보낸다.
4. Response를 기다렸다가 브라우저에 보여준다.

<br>

### gzip 사용후

1. 브라우저가 클라이언트 서버측에 리소스를 요청한다.
2. 서버는 Request를 받는다.
3. Response에 요청한 내용을 **압축해** 담아 보낸다.
4. Response header에 압축이 되어있다는 정보(`content-encoding:gzip`)를 확인후 브라우저는 해당 내용을 받고, 압축을 해제한 후 사용자에게 보여준다.

<br>

gzip을 사용하면 브라우저는 클라이언트 서버에서 리소스를 다운로드 하는 속도를 줄여 응답속도가 빠르다는 장점이 있다.

<br>

> HTTP/1.1 을 지원하는 대부분의 현재의 브라우저는 gzip 으로 압축된 콘텐츠를 사용 가능하다.

<br>

## 웹 서비스 텍스트 압축 확인해보기

<br>

Naver 메인페이지에 들어갔을때의 요청이다.

![Text Compression 텍스트 압축](./../Images/Text%20Compression%20텍스트%20압축/Text%20Compression%20텍스트%20압축-1.png)

Header중 `content-encoding: gzip` 이 보인다.

리소스가 gzip 인코딩해 왔다는 의미이다.

<br>

### 텍스트 압축 효과

![Text Compression 텍스트 압축](./../Images/Text%20Compression%20텍스트%20압축/Text%20Compression%20텍스트%20압축-2.png)

이미지 출처: [https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/optimize-encoding-and-transfer?hl=ko](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/optimize-encoding-and-transfer?hl=ko)

<br>

GZIP 압축으로 얻을 수 있는 절감 효과를 나타내는 표이다.

이러한 절감 효과는 60%에서 88% 사이로 압축이 되어진다.

<br>

### gzip 주의점

하지만 때로는 원본의 리소스를 gzip으로 압축한 결과가 오히려 더 큰 사이즈를 만들기도한다.

원본 리소스의 크기가 매우 작아서 압축해서 절감되는 사이즈보다 더 큰 경우가 그럴수도 있다.

또한

압축(encoding)하는데 걸리는 시간과 해제(decoding)하는데 걸리는 시간때문에 성능이 더 안좋을 수 있다.

<br>

따라서 보통 2KB 이하의 사이즈는 압축을 하지 않는 것이 좋다.

<br>

## gzip 압축하는 방법

텍스트 압축은 파일을 제공하는 쪽에서 해줘야 한다.

주로 apache, nginx 와 같은 게이트웨이에서 하기도 하지만, 때에 따라서는 서버에서 직접 하기도 한다.

프론트엔드의 번들 파일들이 CDN에 올라가 있다면 CDN에 텍스트 압축 설정을 해주어야한다.

<br>

여러 gzip을 하는 방법이 있지만 그중에 두 가지에 대해 더 자세한 설명을 링크 걸어두었다.

1. [Apache 나 Nginx 등 게이트웨이 Web서버에서 처리하기](http://nginx.org/en/docs/http/ngx_http_gzip_module.html)
2. [AWS CloudFront와 같은 CDN 에서 gzip 설정해주기](https://docs.aws.amazon.com/ko_kr/AmazonCloudFront/latest/DeveloperGuide/ServingCompressedFiles.html)

<br>

참고

- 프론트엔드 최적화 강의 인프런
- [https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/optimize-encoding-and-transfer?hl=ko](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/optimize-encoding-and-transfer?hl=ko)
- [https://taetaetae.github.io/2018/04/01/apache-gzip/](https://taetaetae.github.io/2018/04/01/apache-gzip/)
- [https://gitabout.com/18](https://gitabout.com/18)
- [https://docs.aws.amazon.com/ko_kr/AmazonCloudFront/latest/DeveloperGuide/ServingCompressedFiles.html](https://docs.aws.amazon.com/ko_kr/AmazonCloudFront/latest/DeveloperGuide/ServingCompressedFiles.html)
- [https://asecurity.dev/entry/왜-NGINX를-사용할까](https://asecurity.dev/entry/%EC%99%9C-NGINX%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%A0%EA%B9%8C)
- [https://m.blog.naver.com/jhc9639/220967352282](https://m.blog.naver.com/jhc9639/220967352282)
- [http://nginx.org/en/docs/http/ngx_http_gzip_module.html](http://nginx.org/en/docs/http/ngx_http_gzip_module.html)

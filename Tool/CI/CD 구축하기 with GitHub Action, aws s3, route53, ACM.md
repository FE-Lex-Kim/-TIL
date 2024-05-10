# CI/CD 구축하기 with GitHub Action, aws s3, route53, **ACM**

## CI/CD란?

CI/CD는 `지속적 통합 (Continuous Integration)`과 `지속적 배포 (Continuous Deployment 또는 Continuous Delivery)`의 약자입니다. 각각의 개념을 간략하게 살펴보겠습니다.

### CI 지속적인 통합

**지속적 통합**은 프로젝트를 **빌드**하고 **테스트**하는 과정을 자동화한 것이다.

- 코드 변경 사항이 발생 (push)할 때마다 자동으로 빌드 및 테스트를 실
- 개발자들은 자주 코드를 통합할 수 있으며, 코드가 충돌되는 현상을 미리 발견할 수 있다.
- 프로덕트 품질 관리 및 버그 발견이 빨라진다.

### CD 지속적인 배포

**지속적 배포**는 말 그대로 빌드된 프로젝트를 **서버에 업데이트**하여 올리는 것이다. 이를 통해 자동화하여 시간도 줄이고, 안정성도 보장할 수 있다.

- 코드 변경 사항이 테스트 및 승인을 거쳐 자동으로 프로덕션 환경에 배포된다.
- 새로운 기능과 버그 수정 사항이 실제 사용자에게 빠르게 제공한다.
- 사용자 피드백을 수집하고 제품을 개선하는 속도를 향상시킬 수 있다.

### CI/CD 과정

- Github Action을 통해 CI/CD 파이프라인을 자동화 한뒤
- Amazon S3을 통해 배포한뒤
- CloudFront를 통해 콘텐츠를 제공한뒤
- Route 53을 사용하여 도메인을 관리하는 과정을 알아볼 것이다.

GitHub Actions 기본 구성

### 필요한 용어 설명

- **GitHub Actions**: GitHub 저장소 기반으로 자동화 워크플로우를 설정하는 도구이다.
- **Workflow**: GitHub Actions에서 정의된 자동화된 프로세스이다.
- **Job**: Workflow 내에서 실행되는 일련의 단계이다.
- **Step**: Job 내에서 실행되는 개별 작업이다.
- **Action**: Step에서 사용할 수 있는 미리 만들어진 작업 또는 커맨드이다.
- **IAM (Identity and Access Management)**: AWS 리소스에 대한 액세스를 안전하게 제어할 수 있는 서비스이다.

### 1. 워크플로우 파일 생성

GitHub Actions 설정은 `.github/workflows` 폴더 내에 YAML 형식의 워크플로우 파일을 통해 이루어진다. 예를 들어, `ci.yml`이라는 이름의 파일을 만들 수 있다.

- 워크플로우: GitHub Actions에서 정의된 자동화된 프로세스이다.

### 2. 이벤트 트리거 설정

워크플로우는 특정 이벤트에 의해 자동으로 실행된다. 가장 흔한 이벤트는 `push`와 `pull_request`이다.

```yaml
name: CI for Node.js

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
```

이 설정은 `main` 브랜치에 코드가 푸시되거나, `main` 브랜치를 대상으로 하는 풀 리퀘스트가 열릴 때마다 워크플로우가 실행되도록 한다.

### 3. 작업 정의

워크플로우는 여러 작업(`jobs`)으로 구성된다. 각 작업은 다른 작업과 독립적으로 또는 순차적으로 실행될 수 있다.

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Use Node.js
        uses: actions/setup-node@v4
        with:
          node-version: "20.x"
      - name: Install dependencies
        run: npm install
      - name: Run tests
        run: npm test
      - name: Build
        run: npm run build
```

- `runs-on`: 작업이 실행될 가상 환경을 지정한다. 예를 들어, `ubuntu-latest`, `windows-latest`, `macos-latest` 등이 있다.
  - GitHub Actions 워크플로우 설정에서 `ubuntu-latest`를 사용하는 것이 좋다. GitHub Actions에서 `runs-on` 항목은 워크플로우가 실행될 가상 환경을 지정하는 것이다.
  - 호환성과 지원: Ubuntu는 널리 사용되는 리눅스 배포판으로, 대부분의 개발 도구와 소프트웨어가 잘 지원된다. 많은 개발자들이 리눅스 환경을 선호하므로, 테스트와 배포에 있어서 광범위한 호환성을 제공한다.
  - 자원 접근성: GitHub Actions에서 제공하는 리눅스 기반 이미지는 편리하고, 빠르게 세팅하여 사용할 수 있는 환경을 제공한다.
  - 비용 효율성: 리눅스 가상 환경은 일반적으로 다른 운영 체제(예: Windows)보다 자원 사용이 효율적이어서, 실행 시간과 관련된 비용을 절감할 수 있다.
- `steps`: 각 작업은 여러 단계로 이루어진다. `uses` 키워드는 공개된 액션 라이브러리를 사용하여 환경을 설정하거나 필요한 도구를 설치하는 데 사용된다.
- `run`: 스크립트 명령을 실행한다. 예를 들어, 의존성을 설치하고 테스트를 실행하며, 프로젝트를 빌드한다.

### **4. 의존성 관리**

여러 작업이 있을 때, 하나의 작업이 다른 작업을 기다렸다가 실행되도록 설정할 수 있다.

```yaml
yamlCopy code
jobs:
  build:
    # 이전에 정의된 build 작업
  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Deploy to AWS
        run: echo "Deploying to AWS"

```

`deploy` 작업은 `build` 작업이 성공적으로 완료된 후에 실행된다.

그럼 본격적으로 GitHub Actions을 적용해보자.

## 1. GitHub Actions Workflow 설정

1. 워크플로우 파일 생성: `.github/workflows` 디렉토리에 `deploy.yml` 파일을 생성한다.
2. 워크플로우 정의:
   - 이 워크플로우는 메인 브랜치에 푸시될 때마다 실행된다.
   - AWS 권한인증을 위해 GitHub Secrets에 `AWS_ACCESS_KEY_ID`와 `AWS_SECRET_ACCESS_KEY`를 저장한다.

```yaml
name: Deploy to AWS

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: "20.x"

      - name: Install Dependencies
        run: npm install

      - name: Build
        run: npm run build
        working-directory: .

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ap-northeast-2

      - name: Deploy to S3
        run: aws s3 sync build s3://${{ secrets.AWS_S3_BUCKET }} --delete
        working-directory: .

      - name: Invalidate CloudFront Distribution
        run: aws cloudfront create-invalidation --distribution-id ${{ secrets.CLOUDFRONT_DISTRIBUTION_ID}} --paths "/*"
```

### 코드 설명

- Checkout Repository: 현재 리포지토리를 체크아웃하여 작업 디렉토리에 소스 코드를 가져온다.
  - 현재 리포지토리는 GitHub에 호스팅된 소스 코드 저장소를 의미한다. 작업 디렉토리는 GitHub Actions가 실행되는 가상 환경의 디렉토리가 생기고 그곳을 의미한다. GitHub Actions 워크플로우가 시작되면, `actions/checkout@v2` 액션을 사용하여 GitHub 저장소의 코드를 이 가상 환경의 작업 디렉토리로 복사한다. 이렇게 하면 워크플로우 내의 다음 단계들에서 소스 코드에 접근할 수 있다.
- Set up Node.js: Node.js를 설치한다. `actions/setup-node` 액션을 사용하여 특정 버전의 Node.js를 설정한다.
- Install Dependencies: `npm install`을 실행하여 필요한 의존성을 설치한다.
- Build: `npm run build`를 실행하여 프로덕션 빌드를 생성한다.
- Configure AWS credentials: AWS 권한설정을한다. 이는 AWS 서비스와 상호 작용하는 데 사용된다.
  - `secrets.AWS_ACCESS_KEY_ID`와 `secrets.AWS_SECRET_ACCESS_KEY`는 GitHub 저장소의 Settings > Secrets 메뉴에서 설정할 수 있다. 이는 AWS 계정과 연동하여 서비스를 안전하게 이용할 수 있도록 해주며, 워크플로우 파일에 직접적으로 권한 정보를 노출하지 않아도 되도록 한다. GitHub Secrets에 정보를 저장하면, 워크플로우 실행 중에만 이를 안전하게 사용할 수 있다.
- Deploy to S3: `aws s3 sync` 명령을 사용하여 빌드된 파일을 S3 버킷으로 동기화한다. `-delete` 옵션은 S3 버킷 내에 존재하지 않는 로컬 파일을 삭제한다.
  - `aws s3 sync` 명령은 지정한 디렉토리(여기서는 `build/` 디렉토리)의 내용을 S3 버킷과 동기화한다. `--delete` 옵션은 S3 버킷에 없는 로컬의 파일을 S3에서도 삭제하도록 함으로써, 로컬과 S3 버킷 간의 내용을 일치시키는 역할을 한다. 이 옵션을 사용하면, 삭제된 파일이 로컬에만 남아 있지 않고 S3 버킷에서도 삭제되어 버전 관리가 더욱 명확해진다. 로컬 파일 자체를 삭제하는 것이 아니라, S3 버킷 내의 파일을 로컬의 상태와 동기화하는 것이다.
- Invalidate CloudFront Distribution: CloudFront 캐시를 무효화한다. 이는 변경된 콘텐츠가 사용자에게 빠르게 전달되도록 한다.
  - CloudFront를 사용하지 않고 S3만으로 서비스하는 것도 가능하다. 이번예제는 사용하지 않아보도록 해보겠다.

## 2. AWS에 배포

### Amazon S3 설정

### 1. S3 버킷 생성

- AWS Management Console 접속: [AWS Management Console](https://aws.amazon.com/console/)에 로그인한다.
- S3 서비스 선택: 서비스 목록에서 "Storage" 카테고리 아래의 "S3"를 선택한다.

![CI/CD 구축하기 with GitHub Action, aws s3, route53, ACM ](/Images/CI:CD%20구축하기%20with%20GitHub%20Action,%20aws%20s3,%20route53,%20ACM%20/1.png)

- "Create bucket" 버튼을 클리한다.
  ![CI/CD 구축하기 with GitHub Action, aws s3, route53, ACM ](/Images/CI:CD%20구축하기%20with%20GitHub%20Action,%20aws%20s3,%20route53,%20ACM%20/2.png)
- 버킷 이름 입력: 버킷의 이름은 전 세계적으로 유일해야 한다. 일반적으로 프로젝트 이름을 포함하는 것이 좋다.
- 리전 선택: 사용자의 대부분이 위치한 지역에 가까운 리전을 선택한다.
- 모든 퍼블릭 액세스 차단: S3 버킷에 직접 접근시키는 것이 아니라, CloudFront를 통해서 캐시된 CDN 서버에 접근시킬 것이기 때문에 **모든 퍼블릭 액세스 차단**에 체크한다.
  ![CI/CD 구축하기 with GitHub Action, aws s3, route53, ACM ](/Images/CI:CD%20구축하기%20with%20GitHub%20Action,%20aws%20s3,%20route53,%20ACM%20/3.png)
- "Create bucket" 클릭하여 버킷 생성 완료.

### 2. 정적 웹사이트 호스팅 활성화

- 버킷 설정 접속: 생성한 버킷을 클릭하여 상세 페이지로 이동한다.
- 속성 탭 접속: 상단의 "속성" 탭을 선택한다.
- "정적 웹사이트 호스팅" 섹션을 찾아 "Edit"을 클릭한다.
  ![CI/CD 구축하기 with GitHub Action, aws s3, route53, ACM ](/Images/CI:CD%20구축하기%20with%20GitHub%20Action,%20aws%20s3,%20route53,%20ACM%20/4.png)

- "활성화" 옵션을 선택하고, 인덱스 문서로 `index.html`을 지정한다. 필요에 따라 오류 문서도 설정할 수 있다.
- 설정 저장.

### 3. 배포 파일 업로드

- 빌드된 파일 준비: React 애플리케이션을 로컬에서 빌드하고, `build` 폴더 내의 파일들을 준비한다.
- 이때 build 폴더 자체를 업로드 하는게 아니라, build 폴더 내부의 있는 파일들을 업로드 해야한다.
- AWS Management Console에서 버킷으로 이동한다.
- "업로드" 버튼을 클릭한다.
- 빌드 폴더의 파일들을 드래그 앤 드롭하거나 "파일추가"를 클릭하여 파일 선택.
- "업로드"를 클릭하여 파일을 버킷에 업로드.

![CI/CD 구축하기 with GitHub Action, aws s3, route53, ACM ](/Images/CI:CD%20구축하기%20with%20GitHub%20Action,%20aws%20s3,%20route53,%20ACM%20/5.png)

### Amazon CloudFront 설정

### 1. CloudFront 배포 생성

- CloudFront 접속: AWS Management Console에서 "네트워킹 및 콘텐츠 전승" 카테고리 아래의 "CloudFront"를 선택한다.
- " CloudFront 배포 생성"을 클릭한다.
- **"Origin Domain Name"에 앞서 설정한 S3 버킷의 주소를 입력한다. 이 주소는 버킷 설정에서 "Static website hosting" 섹션을 통해 확인할 수 있다.**
- Enable Origin Shield을 “예” 를 누른다.**(선택사항)**
  - Origin Shield는 2020년 10월에 발표된 새로운 기능으로, CloudFront에서 캐싱 계층을 하나 더 추가하여 사용자(클라이언트)와 엣지 서버간의 거리를 줄이는 기능이다. 캐시 적중률을 높이고 오리진 서버의 부하를 줄여주어 로드 속도를 향상시키는 효과가 있다.
  - Origin Shield를 활성화하면 요청이 Origin Shield를 경유할 때마다 비용이 추가로 발생된다.

![CI/CD 구축하기 with GitHub Action, aws s3, route53, ACM ](/Images/CI:CD%20구축하기%20with%20GitHub%20Action,%20aws%20s3,%20route53,%20ACM%20/6.png)

- 자동으로 객체 압축을 **Yes**로 설정한다. 요청할 리소스의 파일 크기를 비약적으로 줄여줄 수 있다.
- 뷰어 프로토콜 정책는 **Redirect HTTP to HTTPS**로 설정한다. 이렇게 설정하면 HTTP 프로토콜로 접속 시 자동으로 HTTPS로 리다이렉트된다.
- "Create Distribution"을 클릭하여 CloudFront 배포 생성.

### AWS S3에 직접 접근 못하게 하고 CloudFront에서만 접근가능하게 하기

1. 수정하고자 하는 CloudFront 배포를 찾아 선택한다.
2. 배포 대시보드에서 "원본" 탭을 클릭한다.
3. 기존 Origin을 선택후 편집을 클릭한다.
   - **S3 버킷을 선택한다.**
4. OAC생성 및 S3 버킷 정책 복사

   ![CI/CD 구축하기 with GitHub Action, aws s3, route53, ACM ](/Images/CI:CD%20구축하기%20with%20GitHub%20Action,%20aws%20s3,%20route53,%20ACM%20/7.png)

- 원본 엑세스에서 원본 엑세스 제어설정을 클릭한다.
- "Create new OAC"를 클릭해 생성해준다.
- **이때 중요한게 있다. cloudfront만 s3 버킷에 엑세스 할수있게 하는 정책을 복사해야한다.**
  ![CI/CD 구축하기 with GitHub Action, aws s3, route53, ACM ](/Images/CI:CD%20구축하기%20with%20GitHub%20Action,%20aws%20s3,%20route53,%20ACM%20/8.png)
- **복사한 정책을 s3 버킷에 들어간뒤, “권한” 탭에서 “버킷 정책” 편집 클릭후 붙여 넣어주면된다.**
  - 여기서 위의 볼드 처리한 3가지 부분이 가장 중요하다!

## 3. AWS_ACCESS_KEY_ID와 AWS_SECRET_ACCESS_KEY 설정

이제 다시 돌아와서 github actions에 access key와 secret key를 넣어주어야한다.

```yaml
name: Deploy to AWS

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: "20.x"

      - name: Install Dependencies
        run: npm install

      - name: Build
        run: npm run build
        working-directory: .

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ap-northeast-2

      - name: Deploy to S3
        run: aws s3 sync build s3://${{ secrets.AWS_S3_BUCKET }} --delete
        working-directory: .

      - name: Invalidate CloudFront Distribution
        run: aws cloudfront create-invalidation --distribution-id ${{ secrets.CLOUDFRONT_DISTRIBUTION_ID}} --paths "/*"
```

1. aws에서 우측 상단에 본인의 아이디를 클릭해서 “보안 자격 증명” 을 클릭한다.

![CI/CD 구축하기 with GitHub Action, aws s3, route53, ACM ](/Images/CI:CD%20구축하기%20with%20GitHub%20Action,%20aws%20s3,%20route53,%20ACM%20/9.png)

2. 액세스 키 만들기
   ![CI/CD 구축하기 with GitHub Action, aws s3, route53, ACM ](/Images/CI:CD%20구축하기%20with%20GitHub%20Action,%20aws%20s3,%20route53,%20ACM%20/10.png)

생성후 GitHub Secrets에 등록하면된다.

1. GitHub Secrets에 AWS key 저장
   1. GitHub에서 해당 프로젝트 리포지토리로 이동한다.
   2. 리포지토리의 "Settings" 탭을 클릭한다.
   3. "Secrets and variables" 섹션에서 "Actions"를 선택한다.
   4. "New repository secret" 버튼을 클릭한다.
   5. `AWS_ACCESS_KEY_ID`와 `AWS_SECRET_ACCESS_KEY`를 각각 이름으로 사용하고 값을 입력한다.
   6. "Add secret"을 클릭하여 각각 저장한다.

---

## **4. 도메인 구매 및 연결**

1. **Route 53 대시보드 접근**:
   - AWS Management Console에 로그인 후, "Services"에서 "Route 53"을 선택한다.
   - 시작하기 클릭한다.
2. **도메인 등록**:

   ![CI/CD 구축하기 with GitHub Action, aws s3, route53, ACM ](/Images/CI:CD%20구축하기%20with%20GitHub%20Action,%20aws%20s3,%20route53,%20ACM%20/11.png)

   - Route 53 대시보드에서 "도메인 등록"을 선택한다.
   - 원하는 도메인 이름을 입력하고 사용 가능 여부를 확인합니다. 도메인이 사용 가능하다면, 구매 절차를 진행한다.
   - 필요한 등록 정보(연락처 정보 등)를 입력하고 결제 정보를 제공하여 도메인을 구매한다.

   ### **DNS 설정 및 관리**

   1. **호스팅 영역 생성**:
      - 도메인이 등록되면, 자동으로 도메인에 대한 호스팅 영역(Hosted Zone)이 생성됩니다. 이 영역 내에서 DNS 레코드를 관리할 수 있다.
      - Route 53 대시보드에서 "호스팅 영역"를 선택하고, 구매한 도메인의 이름을 클릭한다.
   2. **DNS 레코드 설정**:
      - CloudFront 또는 S3와 같은 서비스로 트래픽을 라우팅하기 위해 레코드를 추가한다.
      - "레코드 생성"을 선택한다.
   3. **CNAME 레코드 설정 (CloudFront의 경우)**:
      - "Name" 필드에는 도메인의 서브도메인(예: **`www`**)을 입력한다.
      - "Type"에서 "CNAME"을 선택한다.
      - "값" 필드에는 CloudFront 배포의 도메인 이름(예: **`d1234.cloudfront.net`**)을 입력한다.
      - 레코드를 생성하여 저장한다.

## **5. HTTPS 설정**

1. **AWS Certificate Manager (ACM)에서 SSL/TLS 인증서 요청**:

   ![CI/CD 구축하기 with GitHub Action, aws s3, route53, ACM ](/Images/CI:CD%20구축하기%20with%20GitHub%20Action,%20aws%20s3,%20route53,%20ACM%20/12.png)

   - **위의 사진 처럼 미국 동부(버지니아 북부)로 설정을 해야한다!!!(이것 때문에 몇시간을 버렸는지..)**
   - ACM으로 이동하여 "인증서 요청"을 클릭한다.
   - 퍼블릭 인증서 요청을 클릭하고 다음을 클릭한다.
   - 도메인 이름을 입력하고, 인증서 요청을 진행한다.
   - 도메인 소유권을 증명하기 위한 이메일 또는 DNS 검증 절차를 해야한다.
   - “Route 53에서 레코드 생성”을 클릭한다.
     ![CI/CD 구축하기 with GitHub Action, aws s3, route53, ACM ](/Images/CI:CD%20구축하기%20with%20GitHub%20Action,%20aws%20s3,%20route53,%20ACM%20/13.png)
   - 해당 버튼을 클릭하시면 Route 53 을 통해 자동으로 CNAME 이 등록되며 **DNS 검증이 진행된다**.
   - 실제로 Route 53에서 “호스팅역역”으로 가면 CNAME이 생성된 것을 확인할 수 있다.(총 5개의 레코드 가 있어야한다.)
     - NS, SOA 2개(처음에 만들어진것)
     - A 유형 2개(lex-kim,www.lex-kim) → 이건 각각 별칭에 cloudfront 주소가 들어가있어야한다.
     - CNAME (ACM으로 생성된 레코드)
       ![CI/CD 구축하기 with GitHub Action, aws s3, route53, ACM ](/Images/CI:CD%20구축하기%20with%20GitHub%20Action,%20aws%20s3,%20route53,%20ACM%20/14.png)

2. **CloudFront에 인증서 연결**:

   - CloudFront 배포 설정으로 돌아가서 "Edit"을 클릭한다.
   - 대체 도메인 이름에 route53 도메인 이름을 넣는다.

   ![CI/CD 구축하기 with GitHub Action, aws s3, route53, ACM ](/Images/CI:CD%20구축하기%20with%20GitHub%20Action,%20aws%20s3,%20route53,%20ACM%20/15.png)

   - "SSL Certificate" 섹션에서 ACM에서 발급받은 인증서를 선택한다.
   - 설정을 저장하고 배포를 업데이트 한다

참고

- [https://velog.io/@sangwoong/CICD-GitHub-Action으로-CICD-구축하기](https://velog.io/@sangwoong/CICD-GitHub-Action%EC%9C%BC%EB%A1%9C-CICD-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0)
- [https://velog.io/@coddingyun/프론트엔드-CICD-구축하기](https://velog.io/@coddingyun/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C-CICD-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0)
- https://puterism.com/deploy-with-s3-and-cloudfront/
- https://stackoverflow.com/questions/43166169/aws-custom-ssl-certificate-option-is-disabled-in-cloudfront-but-i-created-a-ss

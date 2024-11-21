# Chapter 4-1. 성능 최적화 Part 1


<br>


## 1. 배포 파이프라인

<br>

### 개요
![배포 파이프라인](https://github.com/user-attachments/assets/d2a4391f-8a8c-41f9-b440-ac778cb00043)


- 개발이 완료된 코드를 main 브랜치에 push합니다.
- Next.js의 빌드 파일들이 GitHub Actions을 통해 S3 버킷으로 업로드됩니다.
- S3 버킷에 연결된 CloudFront CDN을 통해 콘텐츠를 사용자에게 빠르게 제공합니다.

<br>

## 2. GitHub Action

### workflow 과정
1. 저장소를 체크아웃합니다.
2. Node.js 18.x 버전을 설정합니다.
3. 프로젝트 의존성을 설치합니다.
4. Next.js 프로젝트를 빌드합니다.
5. AWS 자격 증명을 구성합니다.
6. 빌드된 파일을 S3 버킷에 동기화합니다.
7. CloudFront 캐시를 무효화합니다.


<br>

## 3. 주요 링크

- **S3 버킷 웹사이트 엔드포인트:**  
  [http://hanghae-test.s3-website.ap-northeast-2.amazonaws.com/](http://hanghae-test.s3-website.ap-northeast-2.amazonaws.com/)
- **CloudFront 배포 도메인 이름:**  
  [https://d8fx9nicfl2kt.cloudfront.net/](https://d8fx9nicfl2kt.cloudfront.net/)


<br>

## 4. 주요 개념

### 4.1. GitHub Actions과 CI/CD 도구
- **CI/CD 도구:** 코드 빌드, 테스트, 배포 과정을 자동화하여 개발 효율성을 높이는 도구입니다.
- **GitHub Actions:** GitHub에서 제공하는 CI/CD 플랫폼으로, 워크플로를 정의하고 실행할 수 있습니다.

### 4.2. S3와 스토리지
- **스토리지:** 데이터를 저장하고 관리할 수 있는 클라우드 기반 저장소 서비스입니다.
- **S3:** AWS의 객체 스토리지 서비스로, 정적 웹사이트 호스팅 및 대규모 데이터 저장에 적합합니다.

### 4.3. CloudFront와 CDN
- **CDN(Content Delivery Network):** 사용자와 가까운 서버에서 콘텐츠를 제공하여 속도를 향상시키는 네트워크입니다.
- **CloudFront:** AWS의 CDN 서비스로, S3와 연결하여 빠르고 안전하게 콘텐츠를 배포합니다.

### 4.4. 캐시 무효화(Cache Invalidation)
- CloudFront의 캐시된 콘텐츠를 삭제하여 S3의 최신 데이터를 사용자에게 전달할 수 있게 하는 작업입니다.


### 4.5. Repository secret과 환경변수
- **환경변수:** 시스템 설정값이나 인증 정보 등을 코드 외부에서 관리할 수 있도록 정의된 변수입니다.
- **Repository secret:** GitHub Actions에서 민감한 정보를 암호화하여 안전하게 관리하는 설정입니다.

<br>

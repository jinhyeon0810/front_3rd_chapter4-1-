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

## 5. CDN 도입 여부에 따른 성능 개선 보고서

### 5.1. 테스트 환경
- **대상:** 동영상을 포함한 페이지, 폰트를 포함한 페이지, 기본 Next.js 프로젝트
- **비교 배포 방식:**
    - **S3 단독 배포:** AWS S3를 정적 파일 저장소로 사용
    - **CloudFront 결합 배포:** AWS S3와 CloudFront(CDN)를 함께 사용
- **측정 도구:** 크롬 개발자 도구의 네트워크 탭

### 5.2 성능 분석
#### 5.2.1 동영상을 포함한 페이지 성능 분석
아래 이미지는 S3 단독 배포(왼쪽)와 CloudFront 결합 배포(오른쪽)의 네트워크 요청 비교입니다.

<img width="1290" alt="s3,cloudfront 네트워크 비교" src="https://github.com/user-attachments/assets/e1f970c6-718c-4492-b18d-79c1000c2163">
<br>

| 항목                     | S3 배포 결과       | CloudFront 배포 결과 | 차이점 및 분석                               |
|--------------------------|--------------------|-----------------------|---------------------------------------------|
| **네트워크 요청 수**      |        30       |         30        |     -     |
| **로드 타임**            |     84ms     |        51ms        |        40%감소        |
| **비디오 요청 시간**      |         131ms       |         234ms      |       70%증가        |
| **전송된 데이터 크기**     |   2.1MB |    1.5MB   |         30%감소        |


#### 5.2.2 폰트를 포함한 페이지 성능 분석
아래 이미지는 S3 단독 배포(왼쪽)와 CloudFront 결합 배포(오른쪽)의 네트워크 요청(글꼴) 비교입니다.

<img width="1183" alt="s3,cloudfront 폰트 네트워크 비교" src="https://github.com/user-attachments/assets/379fd008-2659-459a-af18-b367ecfa1729">

<br>

| 항목                     | S3 배포 결과       | CloudFront 배포 결과 | 차이점 및 분석                               |
|--------------------------|--------------------|-----------------------|---------------------------------------------|
| **요청 수**              |       3       |         3          |              -                       |
| **로드 타임**              |       89ms       |          69ms       |     23% 감소      |
| **전송된 데이터 크기**    |    203.4   |      203.3       |         -         |


<br>

## 6. 요약 및 결론

### 요약
- **CDN 도입의 필요성:** 
- **캐시 전략 최적화:** 
- **비용 관리:** 

### 결론




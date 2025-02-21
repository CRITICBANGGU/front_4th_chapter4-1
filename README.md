# 프론트엔드 배포 파이프라인

## 목차

- [주요 링크](#주요-링크)
- [주요 개념](#주요-개념)

## 주요 링크

- AWS S3 버킷: http://hanghaehomework.s3-website.eu-north-1.amazonaws.com
- CloudFront: https://d25g7ciy2pj3dw.cloudfront.net

## 성능 비교 평가

<img width="608" alt="image" src="https://github.com/user-attachments/assets/f6f7a0b8-334f-4dc8-9765-83807d18fb05" />

### AWS S3 버킷

  <img width="608" alt="image" src="https://github.com/user-attachments/assets/cfbe46b4-4345-4274-afb7-e3f07616207b" />

- 총 로딩 시간: 1.06초
- DOMContentLoaded: 731ms
- 요청 수: 34건
- 전송된 데이터: 1.3MB
- 리소스 수: 2.1MB

### CloudFront

<img width="608" alt="image" src="https://github.com/user-attachments/assets/e7d605be-4985-4252-b46c-06c6393c92f9" />

- 총 로딩 시간: 0.561초
- DOMContentLoaded: 209ms
- 요청 수: 36건
- 전송된 데이터: 1.3MB
- 리소스 수: 2.1MB

### 결과

- 총 로딩시간 1.06초 -> 0.561 초 (약 47.08% 향상)
- DOMContentLoaded 731ms -> 209ms (약 71.41% 향상)

## 주요개념

### GitHub Actions과 CI/CD 도구

GitHub Actions는 GitHub에서 제공하는 CI/CD(Continuous Integration/Continuous Deployment) 도구로, 코드 변경 사항을 자동으로 빌드, 테스트, 배포할 수 있도록 돕는다. 이를 통해 개발자는 효율적인 협업과 빠른 배포 프로세스를 구축할 수 있다.

### S3와 스토리지:

Amazon S3(Simple Storage Service)는 AWS에서 제공하는 확장성 높은 객체 스토리지 서비스로, 정적 웹사이트 호스팅, 백업 및 데이터 아카이빙 등에 활용된다. 빠른 데이터 접근과 내구성을 제공하며, 비용 효율적인 데이터 관리를 지원한다.

### CloudFront와 CDN:

Amazon CloudFront는 AWS의 CDN(Content Delivery Network) 서비스로, 전 세계 엣지 로케이션을 활용해 정적 및 동적 콘텐츠를 빠르게 제공한다. 이를 통해 웹사이트의 로딩 속도를 향상시키고 트래픽 부하를 줄이며, 보안 강화도 가능하다.

### 캐시 무효화(Cache Invalidation):

캐시된 콘텐츠를 최신 상태로 유지하기 위해 특정 파일을 캐시에서 제거하는 과정이다. CloudFront와 같은 CDN을 사용할 때, 변경된 리소스가 즉시 반영되지 않는 문제를 해결하기 위해 사용되며, 불필요한 캐시로 인한 사용자 경험 저하를 방지할 수 있다.

### Repository secret과 환경변수:

GitHub Actions에서 보안이 필요한 API 키, 인증 토큰 등을 저장하는 방법으로, Repository secrets를 사용하면 민감한 데이터를 안전하게 관리할 수 있다. 환경변수(Environment Variables)는 실행 중인 워크플로에서 특정 설정을 동적으로 적용하는 데 활용된다.

## 질문사항과 나름의 답변들

1.  CDN 서버에서 데이터가 없을떄, root 서버에서 원본 데이터를 가져오는데, 그 때 동안 사용자가 기다리는 시간을 줄일 수는 없나?

    -> 근처 CDN 서버로 요청하면 되지 않나?

        -> 최악의 상황에는 근처 CDN서버로 요청만 계속 할 수도 있음. 그럼 오히려 대기시간이 엄청 길어질 수 있음.

        -> 홉이 증가하게 되면 생기는 단점
            -> 네트워크 홉이 증가하면서 성능이 저하될 수 있다. (지연시간 발생, 데이터 손실 가능성, 네트워크 대역폭 달라짐(병목현상 발생 가능성 증가))

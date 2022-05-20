# istio
---

과거

### 모놀리틱 아키텍쳐 (하나의 머신, 하나의 서버)

- 과거 머신들에 애칭이 붙던시절 (운영머신, 개발머신 등)
- 하나의 서버에 모든 비즈니스 로직이 다 들어가 있음
- 하나의 DB에 모든 데이터가 집중

장점

→ 기술의 단일화

→ 관리/운영에 유리

단점

→ 여러 언어/기술스택을 가져가기 어려움

→ 빌드/배포 시간이 오래 걸림

→ 모든 전체 로직을 알고있는 사람이 필요함

→ 스케일링에 문제가 많음.

→ 의존성이 커서 잘못된 코드 배포시 서버 전체 장애 확률이 높음

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/627fa7c8-7ed6-471a-995f-7c8925d9dc6f/Untitled.png)

### 마이크로서비스 아키텍처

장점

→ 기술스택의 다양화/해당팀의 전문성과 개발속도가 빨라짐

→ 적절한 기술의 적절한 배치

→ 전체 장애가 아닌 부분 장애

단점

→ 운영관점에서는 여러 기술을 다뤄야함

→ 요청 추적이 어려워짐

→ 각 서비스별 방어로직이 적을 때 장애 여파

→ 보안, 통제가 힘들어짐

### 서비스 메쉬(Service Mesh)의 등장

- 컨테이너 오케스트레이션 툴 kubernetes의 등장
- 여러 사이드카 컨테이너를 함께 띄울 수 있는 Pod
- API기반 무중단 리로드가 가능한 proxy 기술의 발달

SRE - 일관된 서비스 네트워킹 정책 관리

### 현대의 MSA 구조

서비스 메쉬 구조

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0ee9fe0d-2498-4186-a96a-efd578da0566/Untitled.png)

- 클라우드/컨테이너 환경과 어우러지면서 API-GW와는 또 다른 분산 책임 형태로 해당 문제들을 품.
- 각 서비스마다 proxy 컨테이너를 소유하면서 네트워크 레벨에서 해결할 수 있는 common한 문제들을 손쉽게 풀어냄

### DevOps

- 인프라/운영팀(안정성)과 각 개발팀(신속성)의 목표가 다르다보니 충돌하는 영역이 발생
- 개발팀과 인프라팀 서로가 서로를 불신하고 일이 진행이 되지 않음.
- 마이크로서비스 아키텍쳐 흐름에 와서 개발/운영의 사이클에 깊게 관여하는 DevOps문화가 대두

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/032ff2eb-9ffa-4311-87fc-c8a8659760c3/Untitled.png)

- 애자일 개발문화가 Ops까지 연결되는 지점
- 개발팀과 운영팀의 경계를 허물고 함께 디밸롭하는 문화
- 개발 라이프사이클의 개선
    - 지속적인 통합(CI)와 지속적인 배포(CD)를 중심으로 운영/피드백 과정까지 참여
- DevOps Engineer

## 구글의 DevOps 구현 SRE(site reliability engineering)

- 사이트 안정성에 무게중심
- 가용성에 대한 목표 정의
- 장애 발생에 대한 대응/분석/부검

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cabb385b-63bf-41ce-913d-6ae581046575/Untitled.png)

### Metrics & Monitoring

- SLI (Service Level Indicator) 지표를 정의
- SLO (Service Lebel Objective) : SLI 지표들의 Goal

메인 서비스의 응답시간을 SLI로 정했다면, SLO는 다음과 같이 정의할 수 있음

 “ 우리 서비스의 매주 00% 해당 서비스의 응답시간은 100ms 이하여야한다.”

### Capacity Planning

- 시스템자원의 확보
- 자원 활용의 효율성 / 소프트웨어 성능 / 시스템 안정성

### Change Management

- 애자일 문화의 보편화
- 시스템 장애 원인의 대부분은 변경작업 때문
- 점진적인 배포와 변경 / 빠른 원인 분석
- Resilience (회복성)

### Emergency Response

- 장애 처리에 대해 빠른 반응
- 사람의 복구가 아닌 시스템에 의한 복구
- 장애 분석 및 재발 방지에 대한 플랜

### Culture

- Toil Management (노가다 Operation관리)
- 서로를 비난하지 않는 문화
- 실수를 언제든지 할 수 있고 책임을 나눠가지는 문화

---

## 쿠버네티스란

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/09be0223-514e-4312-b5ed-e6e865b5f377/Untitled.png)

- 전통적인 방식은 하나의 OS위에 app 프로세스들을 여러개 띄우는 방식
- 하나의 물리서버 자원을 공유하다보니 app간 영향도가 큼

- 해당 문제를 해결하고자 가상화를 도입한 VirtualMachine의 격리 방식
- 하이버 바이저 위에 OS하나하나를 다 만들기때문에 사용자는 실제 머신으로 착각할 정도
- 하지만 그만큼 리소스 비용이 많이 든다.

- 컨테이너는 VM과 유사하지만 실제 머신의 운영체제 격리 속성을 활용
- VM 이미지를 생성하는 것보다 훨씬 비용이 저렴
- 운영자는 각 VM들을 관리하기보다 어플리케이션 중심으로 운영/관리 가능

### Kubernetes (k8s)

- 구글의 15년간의 프로덕션 운영 경험과 문제들을 풀어낸 컨테이너 오케스트레이션 툴
- 2014년 오픈소스화
- 컨테이너화된 워크로드와 확장가능성이 높은 플랫폼을 디자인
- 선언적 구성과 자동화에 대해 용이하도록 설계

### Kubernetes (k8s) 구성요소

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/17d7e5a0-b29a-4366-8975-7bf436c436bb/Untitled.png)

- 클러스터를 관리해주는 마스터 노드와 실제 서비스 컨테이너들이 뜨는 워커노드들로 분리
- 내결함성과 고가용성을 지원
- API 서버를 지원

### Kubernetes (k8s) 핵심 패러다임

- 선언적 배포방식
    - Desired state를 정의하고 운영자가 원하는 방식을 정의
    - 쿠버네티스는 해당 스테이트를 만들기 위해 지속적 노력 (Sync loop)
    - 과거 전통적인 명령형 배포방식 (server start, server down)과 다른 패러다임
    - 운영 비용이 엄청나게 줄어들음
    - yaml, helm 등 을 이용하여 선언적 배포방식
- 레이블을 이용한 간편한 워크로드 구성
    - Label을 활용한 서비스 워크로드
    - Pod 집합에서 실행중인 애플리케이션을 네트워크 서비스로 노출시키는 방법
    - 새 배포에도 라우팅을 건드릴 필요가 없다. (Label을 활용한 느슨한 커플링)
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/480b92f7-aa33-4ea3-9403-792aa29d8b5b/Untitled.png)
    
    - 서비스 워크로드
    - Pod 집합에서 실행중인 애플리케이션을 네트워크 서비스로 노출
    - 새 배포에도 라우팅을 건드릴 필요가 없다.
- API 서버를 지원
    - API-Server
    - 쿠버네티스의 상태를 핸들링 할 수 있는 컨트롤 플레인의 api-server 제공
    - Kubectl 명령어 도구
    - 쿠버네티스의 오브젝트들의 질의하고 조작할 수 있다.

---

## Service mesh 란?

### MSA구조의 공통적인 문제들

- 모니터링
- 중앙 로그 수집
- 분산 트랜잭션 추적
- 중복 인증처리
- 기타 등등

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b3dff36f-d810-4ff6-832e-9531a620dd67/Untitled.png)

- API-GATEWAY가 장애가 나면 전체 서비스가 마비가된다
- 장애사항 이슈를 찾는게 힘들다.

### 장애상황 시 SRE/DevOps 엔지니어의 패닉

- 결국 긴박한 장애상황에 덤프를 떠서 분석을 하는 것은 시간이 많이 걸림.
- 마이크로서비스팀이 사용하는 프레임워크가 다 다르고 네트워크 설정이 다 다름.
- 어떤 서비스가 문제야? 원인구간 특정하는 시간이 오래걸림 (서비스 디스커버리 타임)
- 서비스별 모니터링 도구도 다르고 로깅 정책도 다름
- 결국 원인 추정이 느려진다.

### 서비스 메쉬 아키텍쳐의 기본전략

![프록시 사이드카 패턴](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8021e54c-ee07-42a5-ac5a-e3bc425380d1/Untitled.png)

프록시 사이드카 패턴

- 어플리케이션 레이어와 인프라 레이어 간의 간극을 줄이자
- 어플리케이션 전용 프록시를 각 서비스별로 줌
- 일관된 메트릭 처리
- 일관된 네트워크 정책관리

### Pod

- 하나 이상의 컨테이너 그룹
- 스토리지 및 네트워크를 공유
- 따라서 서비스메쉬 구성 시 sidecar 구성으로 프록시끼리 통신이 가능하게 만듬

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0ee9fe0d-2498-4186-a96a-efd578da0566/Untitled.png)

컨트롤 플레인과 데이터 플레인

- 컨트롤 플레인을 이용해 마이크로서비스들의 프록시를 정책 관리
- 각 마이크로서비스들은 소유한 프록시를 활용
    - 세밀한 네트워크 정책 수정
    

Go-language를 사용하는 Istio

---

## istio와 envoy

서비스메쉬의 핵심 컴포넌트 확인

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/88ca6479-03c4-4972-90e2-c35524cf44ce/Untitled.png)

컨트롤 플레인 → istiod

데이터 플레인 → istio-proxy (envoy)

서비스메쉬 구현체

### Proxy

- 보안강화
- 확장성, 유연성
- 로드밸런싱
- 한마디로 중계서버
- Example
    - Nginx
    - Envoy
    - HAProxy

### Envoy proxy

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/31aa154a-4aba-42b9-b959-1fea08e2565d/Untitled.png)

- C++로 구현된 고성능 프록시
- 네트워크의 투명성을 목표
- 다양한 필터체인 지원
- L3/L4 필터
- HTTP L7 필터
- 동적 configuration API 제고
- 기능이 많다.
- CNCF / 벤더가 없다

### Istio-proxy

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e7689632-9dd6-46cb-9139-0fb06375bbe9/Untitled.png)

- Golan으로 짜여져 있는 envoy 래핑한 프록시
- envoy를 활용해 서비스앱의 사이드카 컨테이너로 활용
- 중앙 컨트롤 플레인인 istiod와 통신하고 서비스 앱의 트래픽을 정해진 정책/필터에 따라 처리
- 관찰가시성을 위한 메트릭 제공

### 래핑 아이템으로 envoy를 선택한 이유

- API 기반 Hot reload 기능
- 다양한 기능
- 친절한 네트워크 메트릭 제공

### Pod / sidecar container

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/de8b5c28-4943-418f-a621-9fd67e2d23e7/Untitled.png)

- 파드내 네트워크 인터페이스를 컨테이너끼리 공유
- iptables를 조작하여 inbound traffic과 outbound traffic 모두를 istio-proxy가 선처리
- 서비스 앱의 세밀한 네트워킹 설정에 대한 책임을 가져옴

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4169cf82-e7c7-49fc-97df-435c3d177afc/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e65f3572-2e43-4047-b8cb-3f60a026d92e/Untitled.png)

---

## Kubernetes 관리 툴 (운영 도구 셋업)

## Lens

- 쿠버네티스 운영툴
- 다양한 OS 지원

### VSCode의 Kubernetes 패키지

- Lens보다 부드럽지 않지만 기능 충분
- 코드화 추출 용이
- 터미널 기능이 함께 있음.

## yaml 파일을 이용한 선언적 구성

- GitOps 형태의 발전의 근간
- 선언형 구성에 대한 API를 제공하는 컨트롤 플레인에서 데이터스토어 역할까지

### Pod

- 쿠버네티스에서 배포가능한 가장 작은 컴퓨팅 단위
- 하나 이상의 컨테이너 그룹
- 보통 Pod자체를 define해서 apply하지 않고 Deploy/replic 등을 이용하여 리소스 배포 최소 단위로 사용

### ConfigMap

- 기밀이 아닌 카-값 쌍으로 데이터를 저장하는데 사용하는 API 오브젝트
- Pod는 볼륨에서 환경 변수, 커맨드-라인
- 말그대로 설정값 키밸류 스토어
- 의도치 않은 설정값 반영으로 종종 장애 원인
- Secret한 정보는 담지 않는 것 추천
- 파드에 환경변수 매핑 or 데이터 마운트용

### Deployment

- 디플로이먼트 → 레플리카셋 → Pod

### Service

- 네트워크 서비스를 외부로 노출
- .spec.selector = 연결된 라밸매핑
- Port → targer Port

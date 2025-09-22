
<h1 align="center">plomarble</h1>

---

## 📑 목차
1. [개요](#-개요)
2. [주요 기능](#-주요-기능)
3. [플랫폼 아키텍처](#-플랫폼-아키텍처)
4. [향후 개선사항](#-향후-개선사항)

---

## 📝 개요
세상을 더 깨끗하게 만드는 걷기 운동을 위해 **여행지를 추천하는 서비스** **"플로마블"**  

- 1인 개발자로서 **기획 → 개발 → 배포** 전 과정을 직접 수행  
- 현재 **iOS 앱스토어**에 배포 완료  

👉 [App Store 바로가기](https://apps.apple.com/kr/app/id6752662902)
  <img src="assets/qr.png" alt="스크린샷1" width="100" height="100" />

<p align="center">
  <img src="assets/ plomarble_1.png" alt="스크린샷1" width="250" height="300" />
  <img src="assets/plomarble_2.png" alt="스크린샷2" width="250" height="300" />
  <img src="assets/plomarble__3.png" alt="스크린샷3" width="250" height="300" />
</p>


## 💻 주요 기능

### 🔹 Server
- **모놀리식 API 서비스 + 추천 서비스**를 Ingress로 라우팅
- 한국관광공사 API 기반 **정형 데이터 획득 → 임베딩 벡터화 → Airflow DAG 자동화**
- **실시간 추천 코스 목록 제공**
  - 텍스트 추론 기반 후보군 확보  
  - 위도/경도 기반 스코어와 텍스트 유사도 점수를 가중합하여 **rerank**  
  - 최종 코스 목록을 **snapshot**으로 제공

### 🔹 Client
- **Expo 기반 크로스플랫폼 앱 (iOS/Android)**
- **zustand**를 활용한 경량 상태 관리
- **Kakao Mobility 길찾기 API + OSRM API**를 활용한 보행자 길찾기 및 Polyline 시각화

### 🔹 Inference
- 여행 **코스 제목 임베딩 → 텍스트 유사도 스코어링**에 활용

---

## 🏗 플랫폼 아키텍처

<p align="center">
  <img src="https://github.com/user-attachments/assets/530c6823-9f07-455d-a79a-4ea7aa896fd5" alt="플랫폼 아키텍처 다이어그램" width="800" />
</p>


1. 단일 노드 클러스터 내에서 **namespace**로 서비스 분리 (milvus, plog, cert 등)  
2. 한국관광공사 정형 데이터를 가공 → **Milvus 벡터 DB(S3 기반)** 저장  
   - Airflow 파이프라인(local) + 포트 포워딩  
3. 추천 코스 생성 시 **FastAPI inference pod** 구성 → **FQDN 기반 네임스페이스 간 통신**

---

## 🚀 향후 개선사항
- 멀티 AZ 기반 **이중화/삼중화 클러스터 구성** → 롤링 업데이트 통한 무중단 배포  
- Milvus 파드 분리 및 **추가 워커 노드/NAS 기반 캐시 Pod 구성**  
- **Gateway 도입**을 통한 서비스 디스커버리  
- **Inference server node 확장** → 사용자 행동 데이터를 활용한 **개인화 추천**  
- **모니터링 시스템 도입** (Prometheus / Grafana 등)  
- **CI/CD 파이프라인 구축**으로 자동화된 배포 프로세스 최적화  

---

## 📌 Summary
플로마블은 **여행 추천 + 걷기 운동 가치 확산**을 목표로,  
플로깅을 목적으로 여행을 계획하는 사람들을 위한 서비스입니다. 

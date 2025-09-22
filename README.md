# plomarble
플로마블 소개를 위한 repo

EKS 기반 **Kubernetes 프로젝트** 

---

## 📑 목차
1. [개요](#개요)
2. [주요 feature 구성](#주요-feature-구성)
3. [플랫폼 아키텍처](#아키텍처)
4. [배포 및 운영전략 ](#배포-eks)
5. [향후 개선사항](#향후-개선사항)


---

## 개요 


세상을 좀 더 깨끗하게 만드는 걷기 운동을 위해 추천하는 여행지를 제공하는 서비스 **"플로마블"**

1인 개발자로서 기획 / 개발을 담당하였습니다. 

현재 IOS 앱을 배포하여 store에서 제공중입니다.

appStore 링크: https://apps.apple.com/app/plomarble/id6752662902


[ plomarble splash ] ,  [ 이용 화면 ]

---

## 주요-feature-구성

- **[ server ]**
- **모놀리식 API 서비스** + **추천 서비스**를 Ingress로 라우팅
- 한국관광공사 제공 API를 활용하여 정형 데이터를 획득, 해당 데이터를 임베딩하여 관광지 data를 벡터화 -> [Airflow DAG 스크립트 작성을 통한 자동화]
- 실시간 추천 코스 리스트 기능 구현 -> [text 추론을 활용한 후보군을 확보하고 위도 , 경도 기반 텍스트 유사도 점수 + 위치 기반 점수를 가중합하여 rerank한 코스목록을 snapshot 제공]

- **[ client ]**
- **Expo 기반 크로스 플랫폼 app **
- zustand를 활용한 상태 관리
- kakao mobility 길찾기 API + OSRM 프로젝트 api를 활용한 보행자 길찾기 API 활용한 polyline 그리기

- **[ inference ]**
- 여행 코스 제목을 추출하여 텍스트 임베딩 기반 유사도 스코어링에 활용함



## 플랫폼 아키텍처







<img width="300" height="300" alt="스크린샷 2025-09-22 오전 11 17 08" src="https://github.com/PLOG-ORGANIZATION/public-plomarble-project/plomarble.png" />


---

## 📑 목차
1. [개요](#개요)
2. [주요 feature 구성](#feature)
3. [플랫폼 아키텍처](#플랫폼_아키텍처)
4. [향후 개선사항](#개선사항)


---

## 개요 


세상을 좀 더 깨끗하게 만드는 걷기 운동을 위해 추천하는 여행지를 제공하는 서비스 **"플로마블"**

1인 개발자로서 기획 / 개발을 담당하였습니다. 

현재 IOS 앱을 배포하여 store에서 제공중입니다.

appStore 링크: https://apps.apple.com/app/plomarble/id6752662902


<img width="300" height="300" alt="스크린샷 2025-09-22 오전 11 17 08" src="https://github.com/user-attachments/assets/" />
,  [ 이용 화면 ]

---

## feature

- **[ server ]**
  - **모놀리식 API 서비스** + **추천 서비스**를 Ingress로 라우팅
  - 한국관광공사 제공 API를 활용하여 정형 데이터를 획득, 해당 데이터를 임베딩하여 관광지 data를 벡터화 <br />
     -> Airflow DAG 스크립트 작성을 통한 자동화 <br />
  - 실시간 추천 코스 리스트 기능 구현 <br />
     -> text 추론을 활용한 후보군을 확보하고 위도 , 경도 기반 텍스트 유사도 점수 + 위치 기반 점수를 가중합하여 rerank한 코스목록을 snapshot 제공

- **[ client ]**
  - Expo 기반 크로스 플랫폼 app
  - zustand를 활용한 상태 관리
  - kakao mobility 길찾기 API + OSRM 프로젝트 api를 활용한 보행자 길찾기 API 활용한 polyline 그리기

- **[ inference ]**
  - 여행 코스 제목을 추출하여 텍스트 임베딩 기반 유사도 스코어링에 활용함



## 플랫폼_아키텍처

<img width="994" height="695" alt="스크린샷 2025-09-22 오전 11 17 08" src="https://github.com/user-attachments/assets/530c6823-9f07-455d-a79a-4ea7aa896fd5" />


[ platform architecture ]

👍 1. 단일 노드로 namespace를 구분하여 milvus, plog , cert 등을 구성함. <br />
👍 2. 한국 관광공사 정형데이터를 가공하여 milvus 벡터로 구성하는 Air-flow 파이프라인을 local에 구성 -> 포트 포워딩으로 milvusDB(s3)에 영속화 <br />
👍 3. 추천 코스 리스트 구성시 text 임베딩을 위해 fasti api pod를 구성 -> FQDN을 활용해 다른 namespace간 통신 <br />

## 개선사항

하고 싶었지만 과금 혹은 현재 도메인 볼륨에 맞지 않다고 판단한 내용입니다.

1. 단일 노드가 아닌 az 구성에 따른 이중화 혹은 삼중화구성 -> 롤링 업데이트를 활용한 무중단배포
2. milvus 파드 분리 및 다른 추가 워커노드 혹은 nas를 활용한 private 존 구성하여 캐시 pod 구성.
3. gateway를 활용한 service discovery
4. inferece server node 추가 -> 유저 행동기반 데이터를 축적하여 개인화된 추천 목록 제공 
5. 모니터링 시스템 도입
6. ci/cd flow 도입으로 배포 파이프라인 최적화

<div align="center">

# HAN YOUNGSEO

### Backend Engineer

AI Backend and Data Systems

데이터가 정확하게 반영되고, 요청이 몰려도 예측할 수 있게 동작하는 백엔드 시스템을 만듭니다.

![Java](https://img.shields.io/badge/Java-17%2B-007396?style=flat-square&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-6DB33F?style=flat-square&logo=springboot&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=flat-square&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Kafka](https://img.shields.io/badge/Kafka-231F20?style=flat-square&logo=apachekafka&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)

</div>

---

## About

Java와 Spring을 중심으로 백엔드를 개발하고 있으며, AI 서비스와 데이터 처리 시스템까지 경험을 넓히고 있습니다.

새로운 기술을 추가하는 것보다 왜 이 처리 방식이 필요한지를 먼저 살펴봅니다. 동시 요청으로 데이터가 덮어써지는 상황, 재시도로 같은 작업이 두 번 반영되는 상황, 외부 API 지연이 서버 전체로 번지는 상황을 재현한 뒤 해결 방법을 선택했습니다. 선택한 방법이 막는 문제뿐 아니라 다른 방법과 비교해 감수한 단점, 현재 구현의 한계, 테스트 결과도 함께 기록합니다.

관심 분야는 데이터 정합성, 동시성 제어, 비동기 처리, 트랜잭션, 장애 대응, 성능 검증입니다.

---

## Projects

| Project | 해결하려고 한 문제 | 선택한 방법 |
|---|---|---|
| [Reliable LLM Chatbot Backend](https://github.com/kangwoul2/llm-chat-platform) | 느린 외부 LLM 호출과 요청 급증이 서버 전체 지연으로 이어지는 문제 | 비동기 처리, 동시 호출 제한, 크기가 제한된 대기열 |
| [Loan Refinance Service](https://github.com/kangwoul2/loan-refinance-service) | 금융 계산 오차와 동시 갱신으로 데이터가 잘못 저장되는 문제 | `BigDecimal`, 짧은 트랜잭션, 낙관적 락, 멱등성 |
| [Commerce Event Pipeline](https://github.com/kangwoul2/commerce-event-pipeline) | Kafka 재전달과 동시 처리로 매출이 중복 집계되는 문제 | 유일성 제약조건, 멱등 소비자, 원자적 갱신 |
| [HR Data Pipeline](https://github.com/kangwoul2/hr-data-pipeline) | 수동 분석 과정에서 입력과 실행 순서에 따라 결과가 달라지는 문제 | 데이터 품질 검사, 트랜잭션 적재, Airflow 실행 관리 |
| [Market Prediction Research](https://github.com/kangwoul2/market-prediction-research) | 미래 정보가 학습 과정에 들어가 모델 성능이 과대평가되는 문제 | 시간 순 데이터 분할, 기준 모델 비교, 시간 이동 검증 |

---

## Reliable LLM Chatbot Backend

[Repository](https://github.com/kangwoul2/llm-chat-platform)

외부 LLM의 응답을 기다리는 동안 서버가 다른 요청을 처리할 수 있도록 비동기 처리를 적용했습니다. 비동기 처리만 사용하면 외부 API로 호출이 한꺼번에 몰릴 수 있으므로 세마포어로 동시 호출 수를 제한하고, 오래 걸리는 작업은 크기가 제한된 대기열로 분리했습니다. 대기열이 가득 차면 요청을 계속 쌓지 않고 거절해 서버가 회복할 여지를 남깁니다.

RAG 응답은 BM25 검색 점수가 기준보다 낮을 때 생성을 중단해 근거가 부족한 답변을 줄였습니다. 처리량만 높이는 것을 목표로 삼지 않고 p95와 p99 지연시간, 오류율, 대기시간을 함께 확인할 수 있도록 측정 구조를 만들었습니다.

---

## Loan Refinance Service

[Repository](https://github.com/kangwoul2/loan-refinance-service)

금리 차이만 비교하지 않고 중도상환수수료, 대환 비용, 상환 방식을 포함해 실제 총비용을 계산합니다. 금액과 비율 계산에는 부동소수점 오차를 피하기 위해 `BigDecimal`을 사용했습니다.

같은 상품을 여러 요청이 동시에 수정할 때 마지막 요청이 앞선 변경을 덮어쓰지 않도록 JPA `@Version` 기반 낙관적 락을 적용했습니다. 충돌이 드문 상품 관리 작업이라는 특성을 고려해 비관적 락보다 낙관적 락을 선택했으며, 충돌한 요청은 HTTP 409로 반환합니다. 재시도된 적재 요청은 `Idempotency-Key`와 DB 유일성 제약조건으로 중복 반영을 막았습니다.

---

## Commerce Event Pipeline

[Repository](https://github.com/kangwoul2/commerce-event-pipeline)

구매 API가 분석과 알림까지 모두 동기 처리하면 후속 기능이 늘어날수록 응답이 느려지고 장애 범위도 커집니다. 구매 수신과 후속 집계를 Kafka로 분리해 각 기능이 독립적으로 처리될 수 있도록 구성했습니다.

Kafka의 최소 한 번 전달 방식에서는 같은 이벤트가 다시 전달될 수 있습니다. 이벤트 ID에 유일성 제약조건을 걸고 중복 확인과 매출 집계를 하나의 트랜잭션으로 묶어, 재전달되더라도 결과가 한 번만 반영되게 했습니다. 동시에 같은 지역의 매출을 갱신할 때 발생하는 갱신 손실은 DB의 원자적 증가 연산으로 막았습니다.

---

## HR Data Pipeline

[Repository](https://github.com/kangwoul2/hr-data-pipeline)

노트북에서 한 번 수행하던 HR 분석을 같은 입력에서 같은 결과를 다시 만들 수 있는 파이프라인으로 확장했습니다. 필수 열, 중복 직원 ID, 허용 범위를 벗어난 값을 적재 전에 검사하고, 검사를 통과한 데이터만 PostgreSQL에 저장합니다.

`to_sql(replace)`로 테이블을 다시 만들면 기본키와 외래키 같은 제약조건이 사라질 수 있어 기존 구조를 유지한 채 하나의 트랜잭션에서 재적재합니다. Airflow는 품질 검사, 변환, 적재 순서를 관리하고 실패한 실행을 확인하기 위해 사용했습니다. 현재 데이터는 1,470행 규모이므로 분산 처리 도구는 사용하지 않았습니다.

---

## Market Prediction Research

[Repository](https://github.com/kangwoul2/market-prediction-research)

Bitcoin의 다음 날 움직임을 상승, 횡보, 하락으로 분류하는 연구입니다. 기존 실험을 다시 검토하면서 전체 데이터로 전처리 기준을 계산하고 데이터를 무작위로 나눈 과정에서 미래 정보가 들어갈 수 있음을 확인했습니다.

개선한 실험은 시간 순서대로 학습, 검증, 테스트 구간을 나누고 전처리 기준도 학습 데이터에서만 계산합니다. 동일한 평가 구간에서 다수 클래스를 반복 출력하는 기준 모델의 정확도는 29.30%였고, BTC 변수만 사용한 ExtraTrees 모델은 정확도 40.00%와 Macro F1 38.44%를 기록했습니다. 이 결과를 투자 수익 가능성으로 확대하지 않으며, 거래비용과 매매 규칙을 포함한 백테스트가 없다는 한계를 명시했습니다.

---

## Technical Focus

| 분야 | 다룬 내용 |
|---|---|
| Java 백엔드 | Java 17, Spring Boot, Spring Data JPA, REST API, Maven |
| Python 백엔드 | Python 3.11, FastAPI, asyncio, httpx |
| 데이터 저장 | PostgreSQL, JPA, SQLAlchemy, Flyway, Redis |
| 정합성과 동시성 | 트랜잭션, 멱등성, 낙관적 락, 경쟁 상태, DB 제약조건 |
| 비동기 처리 | Kafka, 비동기 I/O, 세마포어, 대기열, 재시도, 제한시간 |
| 데이터와 AI | ETL, Airflow, 데이터 품질, RAG, 시계열 모델 평가 |
| 검증 | JUnit, pytest, GitHub Actions, 처리량, p95와 p99 지연시간, Macro F1 |

---

## Experience

### Gi-ant

창립 및 운영, 2022.09.01부터 2023.06.16까지

- 약 40명 규모의 경제 기업분석 동아리를 창설하고 운영했습니다.
- DCF, 산업 분석, IR 자료 분석을 주제로 팀별 연구와 발표 과정을 기획했습니다.

### 제이투모로우원

CUOP 산업체 인턴, 2023.12.18부터 2024.03.01까지

- AWS 기반 웹사이트 제작과 프론트엔드 구현에 참여했습니다.
- 데이터 수집과 기술 자료 작성을 수행하고 대학교 서버 점검을 보조했습니다.

### Infonet AI / Blockchain Graduate Research Lab

연구 인턴, 2024.06.24부터 2025.03.07까지

- 온체인 데이터와 소셜 데이터를 결합한 Bitcoin 방향 예측을 연구했습니다.
- VADER 감성 정보와 정량 변수의 결합을 실험하고 모델 재현성을 검토했습니다.
- 여러 LLM의 성능을 조사해 기술 보고서로 정리했습니다.

### 프로그래머스 데이터 분석 데브코스

프로젝트 리더, 2025.08.07부터 2025.12.03까지, 총 564시간

- ETL, SQL 데이터 모델링, 머신러닝, 시계열 분석을 학습했습니다.
- 무역 데이터 기반 공행성 분류와 무역량 예측 프로젝트를 이끌었습니다.

### 삼성 청년 SW AI 아카데미

Java 트랙, 2026.07부터 현재까지

- Java 알고리즘과 Spring 기반 백엔드 개발 역량을 강화하고 있습니다.

---

## Education

### 광주과학기술원

학사, 2019.03부터 2025.08.14까지

### 대구과학고등학교

2016.03부터 2019.02까지

---

측정하지 않은 성능은 성과로 적지 않습니다. 구현한 범위와 아직 검증하지 못한 부분을 구분해 기록합니다.

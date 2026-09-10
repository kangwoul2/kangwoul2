<div align="center">

# Han Youngseo

### Backend Engineer · AI Backend · Data Systems

I build backend systems by starting from a measurable problem, comparing alternatives, and validating the final design with tests, failure cases, and metrics.

![Java](https://img.shields.io/badge/Java-17%2B-007396?style=flat-square&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-Backend-6DB33F?style=flat-square&logo=springboot&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=flat-square&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-Async%20API-009688?style=flat-square&logo=fastapi&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Data%20Integrity-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Kafka](https://img.shields.io/badge/Kafka-Event%20Streaming-231F20?style=flat-square&logo=apachekafka&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-Cache%20%26%20Coordination-DC382D?style=flat-square&logo=redis&logoColor=white)

</div>

---

## About

백엔드 개발자로서 **정확한 데이터 처리, 동시성 제어, 장애에 강한 API, 비동기 I/O, 데이터 정합성, 그리고 성능을 수치로 검증하는 과정**에 관심이 있습니다.

기술을 먼저 고르기보다 다음 순서로 문제를 해결하려고 합니다.

```text
Problem
  ↓
Baseline
  ↓
Metric Definition
  ↓
Alternative Comparison
  ↓
Implementation
  ↓
Failure / Load Test
  ↓
Measurement
  ↓
Trade-off and Decision
```

데이터 분석과 금융·블록체인 연구에서 시작해 Java/Spring 기반 서비스 개발과 LLM/RAG 백엔드로 확장했습니다. 현재 포트폴리오는 프로젝트마다 다른 백엔드 문제를 담당하도록 구성했습니다.

---

## Portfolio Verification

| Project | Verification | Core problem |
|---|---|---|
| Reliable LLM Chatbot Backend | ![CI](https://github.com/kangwoul2/Reliable-LLM-Chatbot-Backend/actions/workflows/ci.yml/badge.svg) | Async I/O, RAG grounding, queue/backpressure, streaming |
| Loan Refinance Service | ![CI](https://github.com/kangwoul2/Pay_Off_Loan/actions/workflows/backend-ci.yml/badge.svg) | Financial correctness, transaction, optimistic locking, idempotency |
| Commerce Event Pipeline | ![CI](https://github.com/kangwoul2/SQL_EXCEL_EDA/actions/workflows/service-ci.yml/badge.svg) | Kafka delivery, duplicate event, idempotent projection |
| HR Data Pipeline | ![CI](https://github.com/kangwoul2/Python_Data_Analysis_HR_DATA/actions/workflows/ci.yml/badge.svg) | ETL reproducibility, data quality, transactional load |
| Market Prediction Research | ![Research](https://github.com/kangwoul2/Graduation_Paper/actions/workflows/research-v2.yml/badge.svg) | Temporal leakage, baseline, walk-forward, reproducibility |

README에서 주장하는 핵심 구현은 코드·테스트·GitHub Actions 또는 저장소 내 원본 결과로 다시 확인할 수 있게 구성했습니다. 실측하지 않은 성능 개선 수치는 성과처럼 기재하지 않습니다.

---

## Featured Projects

### 1. Reliable LLM Chatbot Backend

[Repository](https://github.com/kangwoul2/Reliable-LLM-Chatbot-Backend)

**AI Backend / Async I/O / RAG / Reliability / Streaming**

외부 LLM API를 사용하는 서비스에서 발생하는 I/O 대기, 동시 요청, 과부하, 응답 전달, 근거 없는 응답 문제를 하나의 백엔드 시스템 문제로 다룹니다.

```text
Client
  ↓
FastAPI
  ↓
Request Context / Correlation ID
  ↓
Routing
  ├─ General Chat
  └─ Grounded Knowledge Chat
        ↓
     Retrieval
        ↓
   Context Guard
        ↓
       LLM
        ↓
  Polling / SSE / WebSocket
```

핵심 구현:

- End-to-End Async I/O
- shared HTTP connection pool
- Semaphore 기반 downstream concurrency control
- bounded Queue와 backpressure
- Request ID 기반 background job
- Polling / SSE / WebSocket 전달 경로
- retry / timeout / idempotency
- BM25 retrieval / no-context guard
- Prometheus HTTP latency / in-flight LLM / grounded outcome metric
- Locust 기반 throughput, p95/p99, error rate 실험 구조

이 프로젝트는 포트폴리오의 메인 프로젝트이며 **AI 모델 자체보다 AI 서비스를 안정적으로 운영하기 위한 백엔드 구조**를 중심으로 설명합니다.

---

### 2. Loan Refinance Service

[Repository](https://github.com/kangwoul2/Pay_Off_Loan)

**Java / Spring Boot / PostgreSQL / Transaction / Concurrency / Financial Domain**

대환대출에서 금리 차이만 보지 않고 중도상환수수료, 대환 비용, 상환 방식을 함께 계산하는 금융 의사결정 서비스입니다. 기존 Next.js 자산을 보존하면서 Spring Boot backend를 추가해 서버 측 정합성 문제를 분리했습니다.

핵심 구현:

- Java `BigDecimal` 기반 금융 계산
- Strategy 기반 상환 방식 분리
- Spring Data JPA / PostgreSQL
- Flyway migration
- transaction boundary
- JPA `@Version` optimistic locking
- stale update → HTTP 409
- `Idempotency-Key` 기반 import 중복 방어
- cacheable read / mutation 시 cache eviction
- 계산 unit test 및 concurrency/cache experiment harness

이 프로젝트에서는 **정확성 → 정합성 → 동시성 → 캐시** 순으로 설계 의사결정을 설명합니다.

---

### 3. Commerce Event Pipeline

[Repository](https://github.com/kangwoul2/SQL_EXCEL_EDA)

**Spring Boot / Kafka / PostgreSQL / Event-driven Architecture**

기존 E-commerce 분석을 구매 이벤트가 생성되는 backend까지 확장했습니다.

```text
Purchase API
    ↓ 202 Accepted
Kafka purchase-events
    ↓
Consumer
    ↓
processed_events dedup
    ↓
transactional projection
    ↓
PostgreSQL regional aggregate
```

핵심 구현:

- event ID를 `Idempotency-Key`로 수신
- Kafka publish / consumer
- region 기반 message key
- at-least-once duplicate 재전달 가정
- PostgreSQL unique constraint 기반 idempotent consumer
- dedup insert + projection update를 같은 transaction으로 처리
- atomic aggregate update로 Lost Update 방어
- duplicate replay experiment

이 프로젝트에서는 Kafka 자체보다 **failure와 duplicate delivery를 정상 경로로 가정한 설계**를 중심으로 설명합니다.

---

### 4. HR Data Pipeline

[Repository](https://github.com/kangwoul2/Python_Data_Analysis_HR_DATA)

**Python / ETL / Airflow / PostgreSQL / Data Quality / FastAPI**

기존 IBM HR 분석을 재현 가능한 데이터 파이프라인으로 확장했습니다. 저장소 내 원본 집계에서 Sales 부서 이직률은 약 20.63%, Human Resources는 19.05%, Research & Development는 13.84%로 확인됩니다.

핵심 구현:

- required schema / duplicate / invalid value quality gate
- employee dimension / attrition fact / department aggregate
- PostgreSQL PK/FK/CHECK constraint
- `to_sql(replace)` 대신 schema를 보존하는 transactional reload
- Airflow DAG
- FastAPI analytics API
- `EXPLAIN ANALYZE` index experiment
- quality / transform unit test

이 프로젝트에서는 **분석 결과보다 분석이 다시 만들어질 수 있는 데이터 계약과 적재 무결성**을 중심으로 다룹니다.

---

### 5. Market Prediction Research

[Repository](https://github.com/kangwoul2/Graduation_Paper)

**Financial Time Series / Evaluation Design / Reproducible ML**

학사 연구의 LSTM / GRU / Transformer 실험을 보존하고, 원본 평가 과정에서 발견한 temporal evaluation 문제를 Research V2에서 다시 검증했습니다.

```text
Original Study
    ↓
Evaluation Audit
    ↓
Chronological Split
    ↓
Train-only Preprocessing
    ↓
Dummy Baseline
    ↓
Cross-market Ablation
    ↓
Walk-forward Validation
    ↓
Selective Prediction
```

GitHub Actions 검증 결과:

- leakage-safe extended 3-class: **Accuracy 0.4000 / Macro F1 0.3844** (`BTC-only + ExtraTrees`)
- majority dummy: Accuracy 0.2930 / Macro F1 0.1511
- actionable direction: **Accuracy 0.5459 / Macro F1 0.5451** (`BTC + cross-market + RandomForest`)
- high-confidence actionable-move slice: coverage 23.1%에서 **Accuracy 64.63% / Macro F1 64.20%**

원래 GRU Accuracy 37.39%와 새 40.00%를 동일 조건의 개선율로 주장하지 않습니다. 평가 protocol이 다르기 때문입니다. 이 프로젝트의 핵심은 **좋은 숫자를 만드는 것이 아니라 더 엄격한 평가를 설계하고 실패한 가설도 남기는 것**입니다.

---

## Technical Focus

| Area | Stack / Concepts |
|---|---|
| Java Backend | Java 17, Spring Boot, Spring Data JPA, REST API, Maven |
| Python Backend | Python 3.11, FastAPI, asyncio, httpx |
| Persistence | PostgreSQL, SQLAlchemy, JPA, Flyway, Supabase experience |
| Concurrency | Async I/O, Semaphore, Queue, Race Condition, Optimistic Lock |
| Reliability | Transaction, Idempotency, Retry, Timeout, Backpressure, DB Constraint |
| Event Systems | Kafka, Partition Key, Consumer Group, At-least-once, Idempotent Consumer |
| Data Engineering | Pandas, ETL, Airflow, Data Quality, Index, EXPLAIN ANALYZE |
| AI / Research | RAG, Retrieval, Grounding, Time Series, Model Evaluation |
| Infrastructure | Docker, Docker Compose, GitHub Actions, Linux, AWS internship experience |
| Measurement | Throughput, p50/p95/p99, Error Rate, Queue Wait, Balanced Accuracy, Macro F1 |

Redis, Kafka, Airflow 같은 기술은 모든 프로젝트에 반복해서 넣지 않고 **각 기술이 해결하는 문제가 실제로 존재하는 프로젝트에만 배치**했습니다.

---

## Experience & Activities

### Gi-ant
**Founder & Lead · 2022.09.01 – 2023.06.16**

- 경제·기업분석 동아리 창설 및 운영
- 약 40명 규모 구성원 선발
- DCF, 산업 분석, IR 자료 분석 활동 기획
- 팀별 리서치와 발표 중심 학습 체계 운영

### 제이투모로우원 · CUOP Industry Internship
**Intern · 2023.12.18 – 2024.03.01**

- AWS 기반 웹사이트 제작 업무 참여
- UI/UX 및 Frontend 구현 보조
- 서비스 및 사업 관련 데이터 수집과 자료 작성
- 대학교 서버 관리 보조, 시스템 점검 및 보안 관련 실무 경험

### Infonet AI / Blockchain Graduate Research Lab
**Research Intern · 2024.06.24 – 2025.03.07**

- 온체인 데이터와 소셜 데이터를 활용한 Bitcoin 가격 예측 연구
- VADER 기반 텍스트 감성 정보와 정량 데이터 결합 실험
- 하이퍼파라미터 조정 및 모델 재현성 검증
- LLaMA / GPT / DeepSeek 등 LLM 성능 조사와 기술 보고
- AI 문서화·라벨링 업무 및 영어 랩미팅 참여

### Programmers Data Analysis Devcourse
**Project Leader · 2025.08.07 – 2025.12.03 · 564h**

- ETL, SQL 데이터 모델링, 머신러닝·시계열 분석 학습
- 팀 프로젝트 리딩 및 분석 방향 수립
- 무역 데이터 기반 공행성 분류 및 무역량 예측 프로젝트 수행

### Samsung Software Academy For Youth (SSAFY)
**Java Track · 2026.07 – Present**

- Java 알고리즘 및 백엔드 개발 역량 강화
- Git 기반 협업 및 프로젝트 환경 학습
- Java/Spring 기반 서비스 개발 역량 확장

---

## Education

- **Gwangju Institute of Science and Technology (GIST)** · 2019.03 – 2025.08.14
- **Daegu Science High School** · 2016.03 – 2019.02

공개 GitHub에는 GPA, 자격증 점수, 어학 점수, 전화번호, 주소 등 불필요한 개인 평가지표와 개인정보를 기재하지 않습니다.

---

## Engineering Principles

### Technology follows the problem

Redis, Kafka, Queue, Lock을 먼저 넣지 않습니다. 문제가 단일 DB transaction으로 해결된다면 더 복잡한 분산 도구를 사용하지 않습니다.

### Correctness before scale

금융 계산의 정밀도, 데이터 무결성, retry 시 중복 방지처럼 잘못된 결과를 만드는 문제를 처리량 개선보다 먼저 해결합니다.

### Tail latency matters

평균 latency만으로 시스템을 평가하지 않습니다. 동시 요청이 증가할 때 p95/p99, error rate, queue wait를 함께 봅니다.

### Evaluation design matters

ML에서도 최고 accuracy 하나만 고르지 않습니다. temporal split, dummy baseline, Balanced Accuracy, Macro F1, coverage를 함께 봅니다.

### Every optimization needs a baseline

```text
Baseline
→ Hypothesis
→ One Change
→ Same Workload
→ Measurement
→ Interpretation
→ Trade-off
```

측정하지 않은 성능 수치는 포트폴리오 성과로 사용하지 않습니다.

---

<div align="center">

**Backend systems should be explainable not only by architecture diagrams, but also by failure cases, tests, and measured trade-offs.**

</div>

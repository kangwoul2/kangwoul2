<div align="center">

# Han Youngseo

### Backend Engineer · AI Backend · Data Systems

I build backend systems by starting from a measurable problem, comparing alternatives, and validating the final design with tests and metrics.

![Java](https://img.shields.io/badge/Java-17%2B-007396?style=flat-square&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-Backend-6DB33F?style=flat-square&logo=springboot&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=flat-square&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-Async%20API-009688?style=flat-square&logo=fastapi&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Data%20Modeling-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-Cache%20%26%20Coordination-DC382D?style=flat-square&logo=redis&logoColor=white)
![Kafka](https://img.shields.io/badge/Kafka-Event%20Streaming-231F20?style=flat-square&logo=apachekafka&logoColor=white)

</div>

---

## About

백엔드 개발자로서 **정확한 데이터 처리, 동시성 제어, 장애에 강한 API, 비동기 I/O, 데이터 정합성, 그리고 성능을 수치로 검증하는 과정**에 관심이 있습니다.

저는 기술을 먼저 고르기보다 다음 순서로 문제를 해결하려고 합니다.

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
Before & After
  ↓
Trade-off and Decision
```

데이터 분석과 금융·블록체인 연구에서 시작해 Java/Spring 기반 서비스 개발과 LLM/RAG 백엔드로 확장했습니다. 현재 포트폴리오는 각 프로젝트가 서로 다른 백엔드 문제를 담당하도록 구성하고 있습니다.

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

핵심 주제:
- End-to-End Async I/O
- HTTP Connection Pool
- Semaphore 기반 concurrency control
- Bounded Queue와 backpressure
- Request ID 기반 background job
- Polling / SSE / WebSocket 비교
- Retry / Timeout / Idempotency
- RAG retrieval / no-context guard
- Prometheus 기반 요청/오류/지연 관측
- throughput, p95/p99, queue wait를 기준으로 한 성능 검증

이 프로젝트는 포트폴리오의 메인 프로젝트이며 **AI 기능보다 AI 서비스를 안정적으로 운영하기 위한 백엔드 구조**를 중심으로 설명합니다.

---

### 2. Loan Refinance Service

[Repository](https://github.com/kangwoul2/Pay_Off_Loan)

**Java / Spring Boot / PostgreSQL / Transaction / Concurrency / Financial Domain**

대환대출 의사결정에서 금리 차이만 보지 않고 중도상환수수료, 인지세, 상환 방식, 대환 시점을 함께 계산하는 금융 서비스입니다.

V2에서는 기존 Next.js 시뮬레이터를 유지하면서 Spring Boot 백엔드를 추가해 다음 문제를 다룹니다.

- `BigDecimal` 기반 금융 계산 정밀도
- JPA / PostgreSQL 데이터 모델링
- Flyway migration
- Transaction boundary
- `@Version` optimistic locking
- stale update에 대한 HTTP 409 응답
- `Idempotency-Key` 기반 중복 요청 방어
- Redis cache 적용 가능 구간과 cache invalidation
- 금융 계산 unit test 및 동시성 실험 설계

이 프로젝트에서는 **정확성, 정합성, 트랜잭션, 동시성**을 핵심 주제로 다룹니다.

---

### 3. Commerce Event Pipeline

[Repository](https://github.com/kangwoul2/SQL_EXCEL_EDA)

**Spring Boot / Kafka / PostgreSQL / Event-driven Architecture**

기존 E-commerce 분석 데이터를 출발점으로 구매 이벤트를 실시간 집계하는 event-driven backend로 확장한 프로젝트입니다.

```text
Purchase API
    ↓
202 Accepted
    ↓
Kafka purchase-events
    ↓
Consumer Group
    ↓
Idempotent Projection
    ↓
PostgreSQL Aggregate
```

핵심 주제:
- Kafka partition / key 선택
- consumer group
- at-least-once delivery
- duplicate event replay
- idempotent consumer
- PostgreSQL `ON CONFLICT`
- projection consistency
- synchronous aggregation과 event-driven 처리의 trade-off

이 프로젝트에서는 **메시징과 이벤트 처리의 이유를 실제 중복/재처리 문제와 연결**합니다.

---

### 4. HR Data Pipeline

[Repository](https://github.com/kangwoul2/Python_Data_Analysis_HR_DATA)

**Python / ETL / Airflow / PostgreSQL / Data Quality / FastAPI**

기존 IBM HR 분석을 재현 가능한 데이터 파이프라인으로 확장한 프로젝트입니다.

- raw CSV validation
- duplicate / null / invalid value quality gate
- employee dimension / attrition fact / aggregate 생성
- PostgreSQL schema constraint
- transaction 기반 reload
- Airflow DAG
- analytics API
- `EXPLAIN ANALYZE` 기반 index 실험

이 프로젝트에서는 **데이터 품질과 재현 가능한 ETL**을 중심으로 다룹니다.

---

### 5. Market Prediction Research

[Repository](https://github.com/kangwoul2/Graduation_Paper)

**Time Series / Financial Data / LSTM / GRU / Transformer / Reproducibility**

Bitcoin 및 금융시장 데이터를 대상으로 기술적 지표와 시계열 모델을 비교한 학부 연구입니다.

기존 실험 결과만 강조하지 않고, 낮은 baseline 성능의 원인을 분석하고 다음 재실험 구조를 명시적으로 추가하고 있습니다.

```text
Original Baseline
  ↓
Label / Class Balance Audit
  ↓
Leakage-safe Split
  ↓
Class-weight / Threshold Tuning
  ↓
Feature Ablation
  ↓
Walk-forward Validation
  ↓
Model Comparison
  ↓
Error Analysis
```

연구 프로젝트는 절대적인 정확도보다 **실험 설계, 재현성, 지표 선택, 모델별 trade-off를 설명할 수 있는 근거**로 사용합니다.

---

## Technical Focus

| Area | Stack / Concepts |
|---|---|
| Backend | Java, Spring Boot, Python, FastAPI, REST API |
| Persistence | PostgreSQL, MySQL, JPA, SQLAlchemy, Supabase |
| Concurrency | Async I/O, Semaphore, Queue, Race Condition, Optimistic Lock |
| Reliability | Transaction, Idempotency, Retry, Timeout, Backpressure |
| Event Systems | Kafka, Consumer Group, Offset, At-least-once, Idempotent Consumer |
| Data | Pandas, ETL, Airflow, Data Quality, Index, EXPLAIN ANALYZE |
| AI Backend | RAG, Retrieval, Embedding, LangChain, LangGraph, ChromaDB |
| Infrastructure | Docker, GitHub Actions, Redis, Linux, AWS experience |
| Measurement | Throughput, p50/p95/p99 Latency, Error Rate, Queue Wait, Cache Hit Ratio |

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

### 1. Technology follows the problem

Redis, Kafka, Queue, Lock을 먼저 넣지 않습니다. 문제가 단일 DB transaction으로 해결된다면 더 복잡한 분산 도구를 사용하지 않습니다.

### 2. Correctness before scale

금융 계산의 정밀도, 데이터 무결성, retry 시 중복 방지처럼 잘못된 결과를 만드는 문제를 처리량 개선보다 먼저 해결합니다.

### 3. Tail latency matters

평균 latency만으로 시스템을 평가하지 않습니다. 동시 요청이 증가할 때 p95/p99와 error rate, queue wait가 어떻게 변하는지 함께 봅니다.

### 4. Every optimization needs a baseline

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

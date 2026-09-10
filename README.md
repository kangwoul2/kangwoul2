<div align="center">

# Han Youngseo · Backend Engineer

**Reliable APIs · Concurrent Systems · LLM Backend · Data-Driven Services**

I build backend systems by starting from a measurable problem, comparing alternatives, and validating the trade-off with tests and metrics.

![Java](https://img.shields.io/badge/Java-17%2B-007396?style=flat-square&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-Learning%20%26%20Building-6DB33F?style=flat-square&logo=springboot&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=flat-square&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-Async%20API-009688?style=flat-square&logo=fastapi&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Data%20Modeling-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Git](https://img.shields.io/badge/Git-Version%20Control-F05032?style=flat-square&logo=git&logoColor=white)

</div>

---

## 👋 About Me

백엔드 개발자로서 **정확한 데이터 처리, 동시성 제어, 장애에 강한 API 설계, 성능을 수치로 검증하는 과정**에 관심이 있습니다.

최근에는 LLM/RAG 서비스를 단순 챗봇으로 구현하는 데서 끝내지 않고, 외부 API 대기와 동시 요청을 실제 백엔드 문제로 바라보며 `Sync → Async → Concurrency Control → Queue → Streaming → Scale-out`으로 구조를 진화시키는 프로젝트를 진행하고 있습니다.

데이터 분석, 금융·블록체인 연구, 웹 서비스 제작 경험을 거치며 **데이터를 이해하고 서비스 구조로 연결하는 역량**을 쌓아왔습니다.

> **Engineering principle**  
> 기술을 먼저 선택하지 않습니다. 문제를 재현하고, 가장 단순한 해결책부터 비교한 뒤, 성능·정합성·복잡도의 trade-off를 측정해 선택합니다.

---

## 🚀 Featured Backend Project

### LLM Backend Performance Lab · Main Project

> **LLM 서비스의 I/O 병목과 동시성 문제를 재현하고, 비동기 처리·Queue·전송 방식 최적화를 정량 비교하는 백엔드 실험 프로젝트**

`FastAPI` · `AsyncIO` · `HTTP Connection Pool` · `Semaphore` · `PostgreSQL` · `Redis` · `SSE` · `WebSocket` · `RAG`

**What I am validating**

- 동기 LLM 호출과 End-to-End Async 구조의 `Throughput / p95 / Error Rate` 비교
- HTTP connection 재사용이 latency와 connection creation overhead에 미치는 영향
- 무제한 concurrency와 Semaphore/Queue 기반 backpressure 비교
- Request ID 기반 비동기 Job의 `Queue Wait / Processing Time` 분리 측정
- Polling / SSE / WebSocket의 요청 수·전송량·결과 전달 지연 비교
- PostgreSQL 기반 Conversation/Message 영속화와 동시 요청 정합성 설계
- Redis Shared State / Distributed Lock을 도입해야 하는 경계 조건 검증
- RAG Grounding과 No-context Guard를 통한 근거 없는 응답 억제

```text
Client
  │
  ▼
FastAPI Async API
  ├── Fast Path ─────────────────────▶ Immediate Response
  │
  └── Slow Path
        │
        ▼
   Concurrency Control
   (Semaphore / Queue)
        │
        ▼
   Connection Pool
        │
        ▼
      LLM / RAG
        │
        ├── Polling
        ├── SSE
        └── WebSocket
```

**Performance-first documentation**

모든 개선은 `Baseline → Hypothesis → Change → Same Load Test → Before/After → Decision` 순서로 기록하며, 실측 전에는 개선 수치를 기재하지 않습니다.

> Repository: **LLM-Backend-Performance-Lab** *(public repository publishing in progress)*

---

## 🧩 Other Projects

### 💳 Loan Refinance Simulator
[Pay_Off_Loan](https://github.com/kangwoul2/Pay_Off_Loan)

금리 차이만 비교하는 단순 계산기가 아니라 **중도상환수수료·인지세·상환 방식·대환 시점을 함께 반영해 손익분기점(BEP)을 계산하는 금융 의사결정 서비스**입니다.

- TypeScript + Big.js 기반 금융 계산 정밀도 관리
- Supabase(PostgreSQL) 기반 금융 상품 데이터 모델링
- Python/Selenium 데이터 수집 파이프라인과 웹 런타임 분리
- 유지 / 즉시 대환 / 수수료 면제 후 대환 시나리오 비교

### 📊 HR Attrition Analysis
[Python_Data_Analysis_HR_DATA](https://github.com/kangwoul2/Python_Data_Analysis_HR_DATA)

IBM HR 데이터를 기반으로 부서·직무·근무환경별 이직 패턴을 분석하고, **Sales 인력 추가 고용 시나리오를 데이터 기반으로 설계**한 프로젝트입니다.

- Pandas 기반 데이터 정제·집계
- 직무/부서별 이직률·업무 강도 비교
- 가설 → 분석 → 시뮬레이션 → 조직 전략 제안 흐름

### 🛒 E-commerce SQL & Excel Analytics
[SQL_EXCEL_EDA](https://github.com/kangwoul2/SQL_EXCEL_EDA)

구매·구독·지역·기후 데이터를 결합해 **상품/지역별 판매 전략을 도출한 SQL 기반 분석 프로젝트**입니다.

- SQL 집계 및 조건 분석
- Excel 기반 분석 모델/시각화
- 위치·기온 데이터를 결합한 아우터 판매 전략 분석

### 📚 Research Repository
[Graduation_Paper](https://github.com/kangwoul2/Graduation_Paper)

금융·온체인·소셜 데이터를 활용한 가격예측 연구 과정의 코드와 데이터 정리 자료를 보관한 연구 저장소입니다.

---

## 🧠 Technical Focus

<table>
<tr>
<td valign="top" width="33%">

### Backend
- Java
- Spring / Spring Boot *(current focus)*
- Python
- FastAPI
- REST API
- Async I/O
- HTTP / Connection Pool
- Concurrency Control

</td>
<td valign="top" width="33%">

### Data & AI Backend
- SQL
- MySQL
- PostgreSQL / Supabase
- Pandas / ETL
- LangChain
- LangGraph
- ChromaDB
- RAG / Embedding

</td>
<td valign="top" width="33%">

### Systems & Infra
- Git / GitHub
- Linux CLI
- AWS experience
- Redis *(performance lab)*
- Kafka *(event-driven lab)*
- Docker *(current expansion)*
- Load Testing / Benchmarking

</td>
</tr>
</table>

### Concepts I care about

`Transaction` · `Idempotency` · `Race Condition` · `Optimistic/Pessimistic Lock` · `Distributed Lock` · `Backpressure` · `Retry / Timeout` · `p95/p99 Latency` · `Throughput` · `Event-driven Architecture`

---

## 💼 Experience & Activities

### Infonet AI / Blockchain Graduate Research Lab
**Research Intern · 2024.06.24 – 2025.03.07**

- 온체인 데이터와 소셜 데이터를 활용한 Bitcoin 가격 예측 연구
- VADER 기반 텍스트 감성 정보와 정량 데이터 결합 실험
- 하이퍼파라미터 조정 및 모델 재현성 검증
- LLaMA / GPT / DeepSeek 등 LLM 성능 조사 및 기술 보고
- AI 문서화·라벨링 업무 및 영어 랩미팅 참여

### J2Tomorrow1 · CUOP Industry Internship
**Intern · 2023.12.18 – 2024.03.01**

- AWS 기반 웹사이트 제작 업무 참여
- UI/UX 및 Frontend 구현 보조
- 서비스/사업 관련 데이터 수집 및 자료 작성
- 대학교 서버 관리 보조, 시스템 점검 및 보안 관련 실무 경험

### Programmers Data Analysis Devcourse
**Project Leader · 2025.08.07 – 2025.12.03 · 564h**

- ETL, SQL 데이터 모델링, 머신러닝·시계열 분석 학습
- 팀 프로젝트 리딩 및 분석 방향 수립
- 무역 데이터 기반 공행성 분류 및 무역량 예측 프로젝트 수행

### Gi-ant
**Founder & Lead · 2022.09.01 – 2023.06.16**

- 경제·기업분석 동아리 직접 창설
- 약 40명 규모 구성원 선발 및 운영
- DCF, 산업 분석, IR 자료 분석 활동 기획·운영
- 팀별 리서치와 발표 중심의 학습 체계 구축

### Samsung Software Academy For Youth (SSAFY)
**Java Track · 2026.07 – Present**

- Java 알고리즘 및 백엔드 개발 역량 강화
- Git 기반 협업·프로젝트 환경 학습
- 현재 Java/Spring 기반 서비스 개발 역량을 집중적으로 확장 중

---

## 🎓 Education

- **Gwangju Institute of Science and Technology (GIST)** · 2019.03 – 2025.08.14
- **Daegu Science High School** · 2016.03 – 2019.02

> 공개 프로필에는 학업 성적, 자격증 점수, 어학 점수 등 불필요한 개인 평가지표를 기재하지 않습니다.

---

## 🔬 How I Approach Backend Problems

```text
1. Reproduce the problem
2. Define the metric
3. Build the simplest baseline
4. Compare 2–3 alternatives
5. Measure the trade-off
6. Document why the final design was selected
```

예를 들어 LLM Backend Performance Lab에서는 단순히 “비동기가 더 좋다”라고 결론 내리지 않습니다.

- I/O-bound인지 CPU-bound인지 먼저 분류
- sync baseline의 saturation point 측정
- async 전환 후 동일 부하 재측정
- concurrency를 무한정 늘리지 않고 Semaphore로 제어
- Queue 도입 시 Queue Wait 증가와 Error Rate 감소의 trade-off 평가
- Polling/SSE/WebSocket을 네트워크 비용까지 포함해 비교

이 과정을 코드와 실험 결과로 설명할 수 있는 개발자가 되는 것이 목표입니다.

---

<div align="center">

### Currently Building

**LLM Backend Performance Lab**  
Async I/O · Queue · Streaming · Consistency · Load Testing

</div>

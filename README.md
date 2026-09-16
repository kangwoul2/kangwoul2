<div align="center">

# 한영서

### 백엔드 개발 · AI 서비스 · 데이터 시스템

문제를 먼저 정의하고, 대안을 비교한 뒤, 테스트와 장애 상황을 통해 선택한 설계를 검증합니다.

![Java](https://img.shields.io/badge/Java-17%2B-007396?style=flat-square&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-6DB33F?style=flat-square&logo=springboot&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=flat-square&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Kafka](https://img.shields.io/badge/Kafka-231F20?style=flat-square&logo=apachekafka&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)

</div>

---

## 소개

백엔드 개발자로서 **데이터 정합성, 동시성 제어, 비동기 처리, 장애 대응, 트랜잭션, 성능 검증**에 관심이 있습니다.

프로젝트와 면접에서는 아래 순서로 설명을 통일합니다.

```text
문제
  ↓
원인
  ↓
대안 비교
  ↓
선택
  ↓
단점과 한계
  ↓
테스트와 검증
```

기술 이름을 많이 나열하기보다 **왜 필요한지, 어떤 문제를 막는지, 다른 방법과 비교했을 때 무엇을 포기했는지**를 설명하는 것을 중요하게 생각합니다.

---

## 주요 프로젝트

| 프로젝트 | 검증 | 핵심 문제 |
|---|---|---|
| [LLM 채팅 백엔드](https://github.com/kangwoul2/llm-chat-platform) | ![CI](https://github.com/kangwoul2/llm-chat-platform/actions/workflows/ci.yml/badge.svg) | 비동기 I/O, 동시 요청 제한, 대기열, RAG, 응답 스트리밍 |
| [대환대출 서비스](https://github.com/kangwoul2/loan-refinance-service) | ![CI](https://github.com/kangwoul2/loan-refinance-service/actions/workflows/backend-ci.yml/badge.svg) | 금융 계산 정확성, 트랜잭션, 낙관적 락, 멱등성, 캐시 |
| [구매 이벤트 처리 파이프라인](https://github.com/kangwoul2/commerce-event-pipeline) | ![CI](https://github.com/kangwoul2/commerce-event-pipeline/actions/workflows/service-ci.yml/badge.svg) | Kafka, 중복 이벤트, 멱등 소비자, 원자적 집계 |
| [HR 데이터 파이프라인](https://github.com/kangwoul2/hr-data-pipeline) | ![CI](https://github.com/kangwoul2/hr-data-pipeline/actions/workflows/ci.yml/badge.svg) | ETL, 데이터 품질, 트랜잭션 적재, 재현성 |
| [시장 방향 예측 연구](https://github.com/kangwoul2/market-prediction-research) | ![Research](https://github.com/kangwoul2/market-prediction-research/actions/workflows/research-v2.yml/badge.svg) | 시계열 데이터 누수, 기준 모델, 시간 순 검증, 재현성 |

README에 적는 성능 수치는 코드·테스트·GitHub Actions 또는 저장소의 실제 결과로 다시 확인할 수 있는 값만 사용합니다.

---

## 1. LLM 채팅 백엔드

[저장소 바로가기](https://github.com/kangwoul2/llm-chat-platform)

**FastAPI / 비동기 I/O / RAG / 동시성 제어 / 응답 스트리밍**

외부 LLM API를 사용하는 서비스에서 발생하는 **긴 I/O 대기, 동시 요청 급증, 외부 API 과부하, 긴 작업 대기, 근거 없는 응답**을 백엔드 문제로 다룹니다.

```text
사용자 요청
  ↓
FastAPI
  ↓
요청 추적 ID
  ↓
일반 대화 / 문서 기반 대화
  ↓
관련 문서 검색
  ↓
근거 확인
  ↓
LLM 호출
  ↓
폴링 / SSE / WebSocket
```

핵심 구현:

- 요청 대기 중 다른 작업을 처리하는 비동기 I/O
- HTTP 연결 재사용을 위한 연결 풀
- 세마포어를 이용한 외부 LLM 동시 호출 수 제한
- 크기가 제한된 대기열을 이용한 과부하 제어
- 요청 ID 기반 비동기 작업 조회
- 일시적 오류만 대상으로 한 재시도와 제한시간
- BM25 문서 검색과 근거 부족 시 응답 차단
- Prometheus 기반 요청 지연시간과 LLM 동시 처리 수 관찰
- Locust 기반 처리량, p95/p99 지연시간, 오류율 측정 구조

핵심 설명은 **“비동기로 바꾸면 한 요청이 빨라지는 것이 아니라, I/O 대기 동안 서버가 다른 요청을 처리할 수 있다. 대신 외부 API로 요청이 한꺼번에 몰릴 수 있어 동시 호출 제한과 대기열이 필요하다.”**로 통일합니다.

---

## 2. 대환대출 서비스

[저장소 바로가기](https://github.com/kangwoul2/loan-refinance-service)

**Java / Spring Boot / PostgreSQL / 트랜잭션 / 동시성 / 금융 계산**

금리 차이뿐 아니라 중도상환수수료, 대환 비용, 상환 방식까지 포함해 대환 여부를 비교하는 서비스입니다. 기존 화면과 계산 로직을 보존하면서 Spring Boot 서버를 추가해 서버 측 데이터 정합성 문제를 다뤘습니다.

핵심 구현:

- `BigDecimal`을 이용한 금융 계산 정밀도 확보
- 상환 방식별 계산 로직 분리
- Spring Data JPA / PostgreSQL
- Flyway 기반 DB 변경 이력 관리
- 짧은 트랜잭션 경계
- JPA `@Version`을 이용한 낙관적 락
- 오래된 버전의 갱신 요청은 HTTP 409로 처리
- `Idempotency-Key` 기반 중복 요청 방지
- 상품 조회 캐시와 변경 시 캐시 무효화
- 계산 테스트, 동시 갱신 실험, 캐시 실험

핵심 설명은 **“금융 서비스에서는 처리량보다 잘못된 금액과 잘못된 동시 갱신을 먼저 막아야 한다.”**로 통일합니다.

---

## 3. 구매 이벤트 처리 파이프라인

[저장소 바로가기](https://github.com/kangwoul2/commerce-event-pipeline)

**Spring Boot / Kafka / PostgreSQL / 이벤트 기반 처리**

기존 전자상거래 데이터 분석 프로젝트를 구매 이벤트가 실제로 생성되고 집계되는 백엔드 구조까지 확장했습니다.

```text
구매 API
  ↓ 202 Accepted
Kafka 구매 이벤트
  ↓
소비자
  ↓
처리된 이벤트 ID 확인
  ↓
하나의 트랜잭션으로 지역별 매출 집계
  ↓
PostgreSQL
```

핵심 구현:

- 이벤트 ID를 `Idempotency-Key`로 사용
- Kafka 생산자와 소비자
- 지역을 기준으로 한 파티션 키
- 최소 한 번 전달에서 발생할 수 있는 중복 재전달 고려
- PostgreSQL 유일성 제약조건 기반 멱등 소비자
- 중복 확인과 실제 집계를 하나의 트랜잭션으로 처리
- DB 원자적 증가 연산으로 갱신 손실 방지
- 동일 이벤트 반복 전송 실험

핵심 설명은 **“Kafka가 중복 전달을 완전히 없애는 것을 기대하지 않고, 같은 이벤트가 다시 와도 최종 DB 결과가 한 번만 반영되도록 멱등성을 보장했다.”**로 통일합니다.

---

## 4. HR 데이터 파이프라인

[저장소 바로가기](https://github.com/kangwoul2/hr-data-pipeline)

**Python / ETL / Airflow / PostgreSQL / 데이터 품질 / FastAPI**

IBM HR 데이터를 이용한 분석을 반복 실행 가능한 데이터 파이프라인으로 확장했습니다. 원본 집계 기준 Sales 부서 이직률은 약 20.63%, Human Resources는 19.05%, Research & Development는 13.84%입니다.

핵심 구현:

- 필수 열, 중복 행, 잘못된 값에 대한 데이터 품질 검사
- 직원 / 이직 / 부서 집계 테이블 분리
- PostgreSQL 기본키·외래키·CHECK 제약조건
- 테이블 구조를 보존하는 트랜잭션 기반 재적재
- Airflow 작업 흐름
- FastAPI 분석 조회 API
- `EXPLAIN ANALYZE`를 이용한 인덱스 전후 비교
- 데이터 품질 및 변환 테스트

핵심 설명은 **“분석 결과 하나보다 같은 원본 데이터에서 같은 결과를 반복해서 만들 수 있는 데이터 계약과 적재 무결성을 먼저 보장했다.”**로 통일합니다.

---

## 5. 시장 방향 예측 연구

[저장소 바로가기](https://github.com/kangwoul2/market-prediction-research)

**금융 시계열 / 머신러닝 평가 / 재현 가능한 실험**

학사 연구의 LSTM, GRU, Transformer 실험을 보존하면서 기존 평가 과정의 데이터 누수 가능성을 다시 점검하고, 시간 순서를 보존한 평가 방식으로 재검증했습니다.

```text
기존 연구
  ↓
평가 방식 점검
  ↓
시간 순 학습/검증/테스트 분리
  ↓
학습 데이터만 이용한 전처리
  ↓
단순 기준 모델 비교
  ↓
외부 시장 변수 비교
  ↓
시간 이동 검증
  ↓
확신도가 높은 구간만 별도 평가
```

GitHub Actions 검증 결과:

- 3분류 BTC 단독 모델: **정확도 40.00% / Macro F1 38.44%**
- 다수 클래스 기준 모델: 정확도 29.30% / Macro F1 15.11%
- ±1% 이상 움직인 날의 방향 분류: **정확도 54.59% / Macro F1 54.51%**
- 큰 움직임 예측에서 확신도 0.65 이상만 선택할 경우: 전체의 23.1% 구간에서 **정확도 64.63% / Macro F1 64.20%**

기존 GRU 정확도 37.39%와 새 40.00%는 평가 방식이 다르기 때문에 직접적인 개선율로 주장하지 않습니다.

핵심 설명은 **“좋은 숫자를 만드는 것보다 미래 정보가 학습 과정에 들어가지 않도록 평가 방식을 고치고, 단순 기준 모델보다 실제로 나은지를 확인했다.”**로 통일합니다.

---

## 기술 역량

| 분야 | 사용 기술과 핵심 개념 |
|---|---|
| Java 백엔드 | Java 17, Spring Boot, Spring Data JPA, REST API, Maven |
| Python 백엔드 | Python 3.11, FastAPI, asyncio, httpx |
| 데이터 저장 | PostgreSQL, SQLAlchemy, JPA, Flyway, Supabase |
| 동시성 | 비동기 I/O, 세마포어, 대기열, 경쟁 상태, 낙관적 락 |
| 안정성 | 트랜잭션, 멱등성, 재시도, 제한시간, 과부하 제어, DB 제약조건 |
| 이벤트 처리 | Kafka, 파티션 키, 소비자 그룹, 최소 한 번 전달, 멱등 소비자 |
| 데이터 엔지니어링 | Pandas, ETL, Airflow, 데이터 품질, 인덱스, EXPLAIN ANALYZE |
| AI·연구 | RAG, 문서 검색, 근거 기반 응답, 시계열, 모델 평가 |
| 인프라 | Docker, Docker Compose, GitHub Actions, Linux, AWS |
| 성능 측정 | 처리량, p50/p95/p99 지연시간, 오류율, 대기시간, Balanced Accuracy, Macro F1 |

---

## 경험 및 활동

### Gi-ant
**창립 및 운영 · 2022.09.01 – 2023.06.16**

- 경제·기업분석 동아리 창설 및 운영
- 약 40명 규모 구성원 선발
- DCF, 산업 분석, IR 자료 분석 활동 기획
- 팀별 리서치와 발표 중심 학습 체계 운영

### 제이투모로우원 · CUOP 산업체 인턴
**인턴 · 2023.12.18 – 2024.03.01**

- AWS 기반 웹사이트 제작 업무 참여
- UI/UX 및 프론트엔드 구현 보조
- 서비스 및 사업 관련 데이터 수집과 자료 작성
- 대학교 서버 관리 보조, 시스템 점검 및 보안 관련 실무 경험

### Infonet AI / Blockchain Graduate Research Lab
**연구 인턴 · 2024.06.24 – 2025.03.07**

- 온체인 데이터와 소셜 데이터를 활용한 Bitcoin 가격 예측 연구
- VADER 기반 텍스트 감성 정보와 정량 데이터 결합 실험
- 하이퍼파라미터 조정 및 모델 재현성 검증
- LLaMA / GPT / DeepSeek 등 LLM 성능 조사와 기술 보고
- AI 문서화·라벨링 업무 및 영어 랩미팅 참여

### 프로그래머스 데이터 분석 데브코스
**프로젝트 리더 · 2025.08.07 – 2025.12.03 · 564시간**

- ETL, SQL 데이터 모델링, 머신러닝, 시계열 분석 학습
- 팀 프로젝트 리딩 및 분석 방향 수립
- 무역 데이터 기반 공행성 분류 및 무역량 예측 프로젝트 수행

### 삼성 청년 SW·AI 아카데미(SSAFY)
**Java 트랙 · 2026.07 – 현재**

- Java 알고리즘 및 백엔드 개발 역량 강화
- Git 기반 협업 및 프로젝트 환경 학습
- Java/Spring 기반 서비스 개발 역량 확장

---

## 학력

- **광주과학기술원(GIST)** · 2019.03 – 2025.08.14
- **대구과학고등학교** · 2016.03 – 2019.02

---

## 설계 원칙

### 기술보다 문제를 먼저 봅니다

Redis, Kafka, 대기열, 락을 먼저 넣지 않습니다. 단일 DB 트랜잭션으로 해결되는 문제라면 더 복잡한 분산 도구를 사용하지 않습니다.

### 처리량보다 정확성을 먼저 봅니다

금융 계산의 정밀도, 데이터 무결성, 재시도 시 중복 처리처럼 잘못된 결과를 만드는 문제를 먼저 해결합니다.

### 평균보다 느린 요청을 함께 봅니다

평균 지연시간만으로 시스템을 평가하지 않습니다. 동시 요청이 증가할 때 p95/p99 지연시간, 오류율, 대기시간을 함께 봅니다.

### 머신러닝도 평가 방식을 먼저 봅니다

최고 정확도 하나만 고르지 않습니다. 시간 순 데이터 분리, 단순 기준 모델, Balanced Accuracy, Macro F1, 예측 범위를 함께 확인합니다.

### 성능 개선은 같은 조건에서 비교합니다

```text
기준 상태
→ 가설
→ 한 가지 변경
→ 같은 부하
→ 측정
→ 해석
→ 단점 정리
```

측정하지 않은 성능 수치는 포트폴리오 성과로 사용하지 않습니다.

---

<div align="center">

**설계는 그림보다 장애 상황, 테스트, 측정 결과로 설명할 수 있어야 한다고 생각합니다.**

</div>
좋아. 네가 원하는 건 “문서 종류별 설명”이 아니라, **상황 → 써야 할 문서 → 문서 안에 들어갈 컴포넌트 → 필요한 다이어그램**을 한 번에 고를 수 있는 표지?

아래처럼 정리하면 실무에서 바로 판단하기 좋아.

# 1. 상황별 문서 선택 테이블

| 상황            | 써야 할 문서                       | 목적           | 핵심 질문              | 산출물 수준        |
| ------------- | ----------------------------- | ------------ | ------------------ | ------------- |
| 문제가 애매함       | Problem Brief                 | 문제 정의        | 진짜 문제가 무엇인가?       | 짧은 조사/정의 문서   |
| 기능 범위 정리 필요   | PRD / 요구사항 문서                 | 제품 요구사항 합의   | 무엇을 왜 만들 것인가?      | 기획/개발/QA 기준   |
| 기술 방향 논의 필요   | RFC                           | 기술 방향 검토     | 이 방향으로 가도 되는가?     | 리뷰/의견 수렴 문서   |
| 구현 설계 필요      | Technical Design Doc          | 실제 구현 설계     | 어떻게 만들 것인가?        | 개발 기준 문서      |
| 중요한 선택 기록 필요  | ADR                           | 의사결정 기록      | 왜 이 선택을 했는가?       | 짧은 결정 기록      |
| 작업 실행/추적 필요   | Jira Ticket / User Story      | 작업 단위 관리     | 누가 무엇을 언제까지 하는가?   | 실행 티켓         |
| API 계약 필요     | API Spec                      | 클라이언트/서버 계약  | 어떤 요청/응답을 주고받는가?   | API 명세        |
| 이벤트/메시지 계약 필요 | Event Spec / Message Contract | 시스템 간 비동기 계약 | 어떤 이벤트를 발행/소비하는가?  | 메시지 명세        |
| DB 변경 필요      | Data Design / Migration Plan  | 데이터 구조/변경 설계 | 데이터는 어떻게 바뀌는가?     | ERD/마이그레이션 계획 |
| 배포 준비 필요      | Release Plan                  | 안전한 배포       | 어떻게 배포하고 되돌릴 것인가?  | 배포 체크리스트      |
| 운영 절차 필요      | Runbook                       | 운영/장애 대응     | 문제가 나면 어떻게 처리하는가?  | 절차 문서         |
| 장애 발생         | Incident Report / Postmortem  | 원인 분석/재발 방지  | 왜 터졌고 어떻게 막을 것인가?  | 장애 회고 문서      |
| 작업 완료 후 학습    | Retrospective / Learning Note | 개선점 축적       | 다음에는 어떻게 더 잘할 것인가? | 회고 문서         |

---

# 2. 문서 종류별 내부 컴포넌트 테이블

| 문서 종류                | 반드시 들어갈 컴포넌트                                                                                                                                        | 상황에 따라 추가                                             | 자주 쓰는 다이어그램                                              |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------- | -------------------------------------------------------- |
| Problem Brief        | Background, Problem, Impact, Evidence, Goal, Non-goal, Next Step                                                                                    | 사용자 인터뷰, 로그 분석, 지표 캡처                                 | Scope Diagram, 간단한 Current Flow                          |
| PRD                  | Summary, Problem, Target User, Goals, Non-goals, User Stories, Requirements, Acceptance Criteria, Metrics                                           | UX Flow, Edge Cases, Policy Rules, Rollout Scope      | User Flow, Activity Diagram, State Diagram               |
| RFC                  | Summary, Context, Problem, Proposal, Alternatives, Trade-offs, Impact, Risks, Questions for Reviewers                                               | Migration Plan, Compatibility, Security Review        | System Context, Container Diagram, Data Flow             |
| Technical Design Doc | Summary, Requirements, Current Architecture, Proposed Architecture, Runtime Flow, Data Design, Error Handling, Rollout, Rollback, Monitoring, Risks | Security, Performance, Migration, Cost, Capacity Plan | Context, Container, Component, Sequence, ERD, Deployment |
| ADR                  | Status, Context, Decision, Options Considered, Decision Drivers, Consequences, Related Links                                                        | Superseded By, Revisit Date                           | 보통 없음. 필요하면 간단한 구조도                                      |
| Jira Ticket          | Why, What, Acceptance Criteria, Technical Notes, Dependencies, Test, Links                                                                          | UX Screenshot, API Contract, Rollback Note            | 거의 없음. 필요하면 작은 Sequence                                  |
| API Spec             | Endpoint, Method, Auth, Request, Response, Error Codes, Validation, Examples                                                                        | Rate Limit, Pagination, Versioning, Idempotency       | API Flow, Sequence Diagram                               |
| Event Spec           | Event Name, Producer, Consumer, Trigger, Payload, Schema, Delivery Semantics, Retry, DLQ                                                            | Ordering, Deduplication, Versioning                   | Data Flow, Sequence Diagram                              |
| Data Design          | Entities, Tables, Columns, Relationships, Indexes, Constraints, Migration, Backfill, Rollback                                                       | Retention, Partitioning, Archiving, Data Quality      | ERD, Data Flow, Migration Flow                           |
| Release Plan         | Scope, Schedule, Pre-checks, Deployment Steps, Post-checks, Rollback, Monitoring, Communication                                                     | Feature Flag Plan, Canary Plan, Owner List            | Deployment Diagram, Rollout Flow                         |
| Runbook              | Situation, Symptoms, Impact, Diagnosis, Mitigation, Resolution, Escalation, Dashboards, Logs                                                        | SQL Queries, CLI Commands, Known Issues               | Failure Flow, Observability Diagram                      |
| Postmortem           | Summary, Impact, Timeline, Root Cause, Detection, Response, What Went Well, What Went Wrong, Action Items                                           | Customer Communication, Preventive Measures           | Timeline, Failure Scenario, Dependency Diagram           |
| Retrospective        | Result, What Worked, What Didn’t Work, Lessons Learned, Next Time, Reusable Pattern                                                                 | Metrics Review, Team Feedback                         | 보통 없음                                                    |

---

# 3. 설계 문서 내부 컴포넌트 상세 테이블

Technical Design Doc 하나만 놓고 보면 내부는 이렇게 계층화돼.

| 설계 문서 섹션              | 목적     | 포함할 내용                                | 적합한 다이어그램                               | 판단 기준                    |
| --------------------- | ------ | ------------------------------------- | --------------------------------------- | ------------------------ |
| Summary               | 전체 요약  | 무엇을 왜 어떻게 바꾸는지 3~5줄                   | 없음                                      | 바쁜 사람이 이것만 읽어도 방향을 알아야 함 |
| Goals / Non-goals     | 범위 정의  | 이번에 할 것, 안 할 것                        | Scope Diagram                           | 범위 논쟁이 생길 가능성이 있으면 필요    |
| Context               | 배경 설명  | 현재 구조, 문제, 외부 의존성                     | System Context Diagram                  | 시스템 경계가 중요하면 필요          |
| Current Architecture  | 기존 구조  | 현재 서비스, DB, 연동 구조                     | Container Diagram, Deployment Diagram   | 기존 구조 이해가 필요하면 필요        |
| Proposed Architecture | 제안 구조  | 변경 후 서비스 구성, 책임 분리                    | Container Diagram, Component Diagram    | 구조 변경이 있으면 필수            |
| Runtime Behavior      | 실행 흐름  | 요청/응답, 이벤트 처리, 트랜잭션 흐름                | Sequence Diagram, Activity Diagram      | 호출 순서나 조건 분기가 중요하면 필요    |
| Data Design           | 데이터 설계 | 테이블, 이벤트, API 스키마, 정합성                | ERD, Data Flow Diagram, Schema          | DB/API/Event 변경 시 필수     |
| Error Handling        | 실패 처리  | 타임아웃, 재시도, fallback, 중복 방지            | Failure Scenario Diagram                | 외부 연동/결제/비동기 처리면 중요      |
| Security              | 보안     | 인증, 권한, 개인정보, 암호화, 접근 제어              | Trust Boundary Diagram, Network Diagram | 민감 데이터나 외부 노출이 있으면 필요    |
| Performance           | 성능     | latency, throughput, cache, index, 병목 | Data Flow, Capacity Diagram             | 트래픽/성능 영향이 있으면 필요        |
| Deployment            | 배포 구조  | 배포 위치, 인프라, 스케일링                      | Deployment Diagram                      | 신규 서비스/인프라 변경 시 필요       |
| Rollout Plan          | 적용 계획  | 단계적 배포, feature flag, canary          | Rollout Flow                            | 점진적 적용이 필요하면 필수          |
| Rollback Plan         | 복구 계획  | 되돌리는 조건과 절차                           | Rollback Flow                           | 장애 시 빠르게 되돌려야 하면 필수      |
| Observability         | 관측     | 로그, 메트릭, 트레이스, 알림                     | Observability Diagram                   | 운영 대상 서비스면 거의 필수         |
| Test Plan             | 검증     | unit, integration, e2e, load test     | Test Matrix                             | 영향 범위가 넓으면 필요            |
| Risks / Trade-offs    | 리스크    | 선택의 장단점, 남는 문제                        | 없음 또는 Risk Matrix                       | 리뷰어 설득에 중요               |
| Open Questions        | 미결정 사항 | 아직 답이 없는 질문                           | 없음                                      | 리뷰 과정에서 정리               |

---

# 4. 다이어그램 종류별 사용 시점 테이블

| 다이어그램                    | 언제 쓰나            | 보여주는 것                                    | 문서 위치                           | 주의점                          |
| ------------------------ | ---------------- | ----------------------------------------- | ------------------------------- | ---------------------------- |
| Scope Diagram            | 범위가 헷갈릴 때        | 포함/제외 범위                                  | Goals / Non-goals               | 너무 세부 구현을 넣지 말 것             |
| System Context Diagram   | 외부 시스템 관계가 중요할 때 | 사용자, 외부 시스템, 우리 시스템 경계                    | Context                         | 내부 컴포넌트까지 넣지 말 것             |
| Container Diagram        | 서비스 단위 구조를 설명할 때 | 앱, 서비스, DB, 캐시, 큐                         | Proposed Architecture           | 클래스/함수 수준과 섞지 말 것            |
| Component Diagram        | 서비스 내부 구조가 중요할 때 | Controller, Service, Repository, Client 등 | Component Design                | 너무 코드 수준으로 내려가지 말 것          |
| Sequence Diagram         | 처리 순서가 중요할 때     | 시간 순 호출 흐름                                | Runtime Behavior                | 정상 흐름과 예외 흐름을 분리할 것          |
| Activity Diagram         | 조건 분기가 복잡할 때     | 분기, 승인, 업무 절차                             | Runtime Behavior                | 호출 주체보다 흐름 자체에 집중            |
| State Machine Diagram    | 상태 전이가 중요할 때     | 상태와 이벤트 전이                                | Runtime Behavior / Domain Model | 불가능한 전이도 명확히 할 것             |
| ERD                      | DB 구조가 바뀔 때      | 테이블과 관계                                   | Data Design                     | 물리/논리 모델을 구분할 것              |
| Data Flow Diagram        | 데이터 이동이 중요할 때    | 데이터 생산자, 소비자, 저장소                         | Data Design                     | 동기/비동기 흐름을 구분할 것             |
| API Schema               | API 계약이 중요할 때    | request/response 구조                       | API Spec / Data Design          | 예시와 에러 응답을 포함할 것             |
| Message Schema           | 이벤트 계약이 중요할 때    | event payload, producer, consumer         | Event Spec / Data Design        | 버전 호환성과 중복 처리 고려             |
| Deployment Diagram       | 배포 구조가 중요할 때     | LB, 서버, Pod, DB, Cluster                  | Deployment                      | 논리 구조와 물리 구조를 혼동하지 말 것       |
| Network Diagram          | 보안/망 구성이 중요할 때   | subnet, firewall, security group          | Security / Infra                | 민감한 내부 정보 공유 범위 주의           |
| Observability Diagram    | 운영 감시가 중요할 때     | logs, metrics, traces, alerts             | Observability                   | 어떤 지표를 왜 보는지 써야 함            |
| Failure Scenario Diagram | 장애 대응이 중요할 때     | 실패 지점과 대응 흐름                              | Error Handling / Runbook        | retry, timeout, fallback을 명시 |
| Rollout Flow             | 단계적 배포가 필요할 때    | 배포 단계와 검증 포인트                             | Rollout Plan                    | 중단 조건과 rollback 조건 포함        |
| Timeline                 | 장애 분석할 때         | 시간순 사건                                    | Postmortem                      | 추측과 사실을 분리할 것                |

---

# 5. 상황별 “문서 + 내부 컴포넌트 + 다이어그램” 조합표

이 표가 제일 실전용이야.

| 상황             | 추천 문서                                  | 필수 내부 컴포넌트                                               | 추천 다이어그램                                      | 추가로 남기면 좋은 것             |
| -------------- | -------------------------------------- | -------------------------------------------------------- | --------------------------------------------- | ------------------------ |
| 작은 버그 수정       | Jira Ticket, PR Description            | Why, What, Test, Risk                                    | 없음 또는 작은 Before/After                         | 재발 가능하면 Postmortem Lite  |
| 단순 API 추가      | Jira Ticket, API Spec                  | Endpoint, Request, Response, Error, Acceptance Criteria  | Sequence Diagram, API Schema                  | 권한/validation 규칙         |
| 기존 API 변경      | API Spec, Design Note, ADR             | 변경 이유, 호환성, migration, error handling                    | Sequence, API Schema                          | Deprecation Plan         |
| 신규 기능 개발       | PRD, Design Doc, Tickets               | Problem, Goal, Requirements, AC, Architecture, Test Plan | User Flow, Context, Sequence, ERD             | Metrics, Rollout Plan    |
| 관리자 기능 개발      | PRD Lite, Design Note                  | Target User, Permission, Audit Log, Requirements         | User Flow, Sequence, ERD                      | 운영 권한 정책                 |
| 외부 시스템 연동      | RFC, Design Doc, API/Event Spec        | Contract, Auth, Retry, Timeout, Error Handling           | Context, Sequence, Failure Scenario           | SLA, 장애 대응 Runbook       |
| 비동기 이벤트 처리     | RFC, Event Spec, Design Doc            | Producer, Consumer, Schema, Retry, DLQ, Idempotency      | Data Flow, Sequence, Message Schema           | 중복/순서/재처리 전략             |
| DB 스키마 변경      | Data Design, Migration Plan            | ERD, 변경 컬럼, index, backfill, rollback                    | ERD, Migration Flow                           | 배포 순서, 락/성능 영향           |
| 데이터 마이그레이션     | Migration Plan, Runbook                | 대상 데이터, 절차, 검증, rollback, owner                          | Data Flow, Rollout Flow                       | 샘플 검증 쿼리                 |
| 성능 개선          | Design Note, Experiment Plan           | 병목, 목표 지표, 접근법, 측정 방법                                    | Current/Proposed Flow, Data Flow              | Before/After Metrics     |
| 캐시 도입          | Design Doc, ADR                        | 캐시 키, TTL, 무효화, fallback, consistency                    | Sequence, Failure Scenario                    | 캐시 장애 시 동작               |
| 신규 서비스 생성      | RFC, Design Doc, Release Plan, Runbook | 책임 범위, API, DB, deployment, monitoring                   | Context, Container, Deployment, Observability | ADR, SLO                 |
| MSA 분리         | RFC, Design Doc, Migration Plan        | Boundary, Ownership, Data Ownership, Migration           | Context, Container, Data Flow, Sequence       | Strangler Plan, Rollback |
| 공통 모듈/라이브러리 도입 | RFC, ADR, Usage Guide                  | 사용 목적, API, 제약, 버전 정책                                    | Component Diagram                             | Migration Guide          |
| 인증/권한 변경       | Design Doc, Security Review            | auth flow, role, permission, audit, threat               | Sequence, State, Trust Boundary               | 보안 검토 체크리스트              |
| 결제/주문 상태 변경    | PRD, Design Doc, State Spec            | 상태 정의, 전이 조건, 실패 처리                                      | State Machine, Sequence, Failure Scenario     | 운영 보정 Runbook            |
| 배치 작업 추가       | Design Note, Runbook                   | schedule, input/output, retry, alert, failure handling   | Data Flow, Failure Scenario                   | 수동 재실행 절차                |
| 배포 방식 변경       | Release Plan, Runbook                  | 단계, pre-check, post-check, rollback                      | Deployment, Rollout Flow                      | 커뮤니케이션 계획                |
| 장애 발생          | Incident Report, Postmortem            | impact, timeline, root cause, action items               | Timeline, Failure Scenario                    | Runbook 업데이트             |
| 반복 운영 작업       | Runbook                                | symptoms, diagnosis, commands, escalation                | Flowchart, Observability                      | 체크리스트                    |
| 의사결정만 남김       | ADR                                    | context, decision, options, consequences                 | 보통 없음                                         | 관련 RFC/PR 링크             |

---

# 6. 문서 크기별 최소 세트

| 작업 크기           | 문서 세트                                                              | 내부 컴포넌트                                               | 다이어그램                                                    |
| --------------- | ------------------------------------------------------------------ | ----------------------------------------------------- | -------------------------------------------------------- |
| XS: 1일 이내 작은 변경 | Jira Ticket + PR Description                                       | Why, What, Test                                       | 없음                                                       |
| S: 며칠짜리 변경      | Jira Ticket + Design Note                                          | Problem, Approach, Risk, Test                         | Sequence 또는 API Schema                                   |
| M: 기능 단위 개발     | PRD Lite + Design Doc + Tickets                                    | Requirements, AC, Architecture, Data, Error, Test     | Context, Sequence, ERD/API Schema                        |
| L: 여러 팀 영향      | RFC + PRD + Design Doc + ADR + Release Plan                        | Alternatives, Trade-offs, Impact, Rollout, Monitoring | Context, Container, Sequence, Data Flow, Failure         |
| XL: 플랫폼/아키텍처 변경 | RFC + Design Doc + ADRs + Migration Plan + Runbook + Postmortem 준비 | Ownership, Migration, Compatibility, Rollback, SLO    | Context, Container, Deployment, Data Flow, Observability |

---

# 7. 개인 업무용 최소 판단표

너 혼자 실무에서 쓸 거면 이 정도로 단순화하면 돼.

| 내가 마주친 상황     | 바로 쓸 것         | 최소로 적을 것                          |
| ------------- | -------------- | --------------------------------- |
| 요청이 애매하다      | Problem Brief  | 문제, 영향, 목표, 제외 범위                 |
| 구현 방법을 정해야 한다 | Design Note    | 현재 구조, 제안 구조, 흐름, 리스크             |
| 중요한 선택을 했다    | ADR            | 배경, 선택, 대안, 결과                    |
| 개발 티켓을 만든다    | Jira Ticket    | Why, What, Done, Test             |
| PR을 올린다       | PR Description | Why, Change, Risk, Test, Rollback |
| 운영 절차가 반복된다   | Runbook        | 증상, 확인법, 조치, 에스컬레이션               |
| 문제가 터졌다       | Postmortem     | 영향, 타임라인, 원인, 액션아이템               |

---

# 8. 최종 치트시트

| 질문                | 선택할 문서/컴포넌트                                     |
| ----------------- | ----------------------------------------------- |
| “왜 해야 하지?”        | Problem Brief, PRD의 Problem/Goal                |
| “뭘 만들어야 하지?”      | PRD, Requirements, Acceptance Criteria          |
| “어떻게 만들지?”        | Design Doc, Architecture, Sequence, Data Design |
| “이 선택이 맞나?”       | RFC, Alternatives, Trade-offs                   |
| “왜 이걸 선택했지?”      | ADR                                             |
| “누가 무엇을 하지?”      | Jira Ticket, Action Items                       |
| “무엇을 주고받지?”       | API Spec, Event Spec, Schema                    |
| “데이터는 어디에 있지?”    | ERD, Data Flow                                  |
| “어떻게 배포하지?”       | Release Plan, Deployment Diagram                |
| “망가지면 어떻게 하지?”    | Failure Scenario, Runbook, Rollback Plan        |
| “잘 돌아가는지 어떻게 알지?” | Observability, Metrics, Alerts                  |
| “다음엔 어떻게 개선하지?”   | Postmortem, Retrospective                       |

정리하면 이렇게 보면 돼.

**상황이 문제 정의 단계면 Problem 계열 문서.**
**합의가 필요하면 PRD/RFC.**
**구현이 필요하면 Design Doc/API Spec/Data Design.**
**실행은 Ticket/PR.**
**운영은 Release Plan/Runbook.**
**사후 학습은 Postmortem/Retrospective.**

그리고 설계 문서 내부에서는 항상:

**Context → Architecture → Runtime Flow → Data → Failure → Deployment → Observability**

이 순서로 컴포넌트를 쌓으면 된다.

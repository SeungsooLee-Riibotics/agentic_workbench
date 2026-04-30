# agentic_workbench

개인적으로 쓰는 AI 작업 자산을 모아두는 저장소다.
다만 현재는 단순 메모 모음이 아니라, 아래 두 축이 함께 들어 있는 workspace에 가깝다.

- AI 작업용 instruction / operating guide / template 모음
- `ai_request_orchestrator` 관련 설계 문서와 서비스 자산

이 문서는 루트 진입점 역할을 한다.
처음 들어왔을 때 어디부터 읽어야 하는지 빠르게 가를 수 있게 정리한다.

---

## 이 저장소에 무엇이 있는가

### 1. 작업 가이드와 운영 문서

- 경로: [`instructions/`](./instructions/)
- 역할: PR 작성, PR 설명, PR 전 정리, 질문 우선 원칙, 설계 요청, 리뷰 intake 같은 작업용 지침을 모아둔 영역
- 진입점: [`instructions/README.md`](./instructions/README.md)

instruction 문서가 대화를 통해 어떻게 진화해야 하는지에 대한 상위 정책과 proposal 템플릿은 [`instructions/instruction_evolution_policy.md`](./instructions/instruction_evolution_policy.md), [`instructions/instruction_update_proposal_template.md`](./instructions/instruction_update_proposal_template.md) 에 둔다.

이 영역은 코드 구현 그 자체보다, **어떻게 요청하고, 정리하고, 리뷰하고, 판단할지**를 다룬다.

### 2. AI Request Orchestrator 설계 / 서비스 자산

- 설계 문서: [`ai-request-orchestrator.md`](./ai-request-orchestrator.md)
- 서비스 자산: [`services/ai_request_orchestrator/`](./services/ai_request_orchestrator/)
- 서비스 실행 안내: [`services/ai_request_orchestrator/README.md`](./services/ai_request_orchestrator/README.md)

이 영역은 내부 AI 요청 오케스트레이터의 아키텍처, 운영 원칙, 서비스 자산을 담는다.

---

## 어디부터 읽으면 되는가

### PR / 리뷰 / 문서화 작업이 목적이면

1. [`instructions/README.md`](./instructions/README.md)
2. 필요한 문서로 이동
   - PR 구조를 잡고 싶으면 [`instructions/PR_GUIDE.md`](./instructions/PR_GUIDE.md)
   - PR 설명 품질을 높이고 싶으면 [`instructions/pr_description_guide.md`](./instructions/pr_description_guide.md)
   - PR 열기 전 한 번 더 정리하고 싶으면 [`instructions/refine_before_pr.md`](./instructions/refine_before_pr.md)
   - 리뷰어 입장에서 문서만으로 판단 가능한지 가르고 싶으면 [`instructions/pr_review_intake_gate.md`](./instructions/pr_review_intake_gate.md)
   - 요구사항이 애매해서 먼저 질문해야 하면 [`instructions/ask.md`](./instructions/ask.md)
   - 설계 요청 템플릿이 필요하면 [`instructions/design_request_guide.md`](./instructions/design_request_guide.md)
   - instruction 문서 자체를 어떻게 발전시킬지 정하고 싶으면 [`instructions/instruction_evolution_policy.md`](./instructions/instruction_evolution_policy.md)
   - instruction 변경안을 바로 써보고 싶으면 [`instructions/instruction_update_proposal_template.md`](./instructions/instruction_update_proposal_template.md)

### Orchestrator 설계나 서비스가 목적이면

1. 설계 개요: [`ai-request-orchestrator.md`](./ai-request-orchestrator.md)
2. 실행/운영: [`services/ai_request_orchestrator/README.md`](./services/ai_request_orchestrator/README.md)

---

## 다른 workspace에서 이 repo를 읽게 하는 방법

권장 방식은 각 workspace의 tracked `AGENTS.md`에 개인 로컬 경로를 직접 쓰지 않고,
gitignore 처리된 local overlay 파일을 optional로 읽게 만드는 것이다.

이 방식이 적절한 이유는 다음과 같다.

- 팀 repo에는 개인 홈 디렉터리나 checkout 경로가 남지 않는다.
- local overlay 파일이 없으면 다른 개발자는 영향을 받지 않는다.
- 개발자마다 자기 `agentic_workbench` 경로를 다르게 둘 수 있다.
- 팀 규칙은 tracked `AGENTS.md`에 남고, 개인 작업 방식은 local overlay로 분리된다.

workspace의 `AGENTS.md`에는 아래처럼 적는다.

```md
## Personal Agentic Workbench Overlay

If `.agents.local.md` exists at the workspace root, read it after this file.
Treat it as an additive personal overlay. Repository instructions take precedence
when they conflict with the local overlay.
```

workspace의 `.gitignore`에는 local overlay 파일을 추가한다.

```gitignore
.agents.local.md
```

그리고 각 개발자의 local overlay 파일에는 자기 환경에 맞는 경로를 적는다.

```md
# Local Agentic Workbench Overlay

Also read:

- /home/lss0815/git/agentic_workbench/README.md
- /home/lss0815/git/agentic_workbench/instructions/README.md
```

주의할 점은 local overlay가 팀 문서의 대체물이 아니라는 것이다. 프로젝트 전체가 따라야 하는
아키텍처 결정, API 계약, runbook은 해당 프로젝트의 tracked 문서에 남긴다. 이 repo에서
행동을 바꾸는 instruction은 [`instructions/instruction_evolution_policy.md`](./instructions/instruction_evolution_policy.md)
흐름에 따라 candidate change로 기록한 뒤 승인해서 반영한다.

---

## 현재 정보 구조 원칙

- 기존 문서를 당장 이동하거나 이름을 바꾸지 않는다.
- 먼저 **루트 진입점**과 **instructions 분류 체계**를 명확히 한다.
- 문서 간 역할이 인접하더라도, 파일 자체를 합치기 전에 **index와 cross-link**로 길을 잡는다.

이 원칙을 둔 이유는, 이미 일부 문서가 다른 문서를 직접 참조하고 있어서 성급한 rename/move가 불필요한 churn을 만들 수 있기 때문이다.

---

## 지금 상태에서의 추천 사용 순서

- 이 repo가 무엇인지 파악하려면: **이 README**
- 작업 가이드를 찾으려면: [`instructions/README.md`](./instructions/README.md)
- orchestrator 설계를 읽으려면: [`ai-request-orchestrator.md`](./ai-request-orchestrator.md)
- orchestrator 실행 자산을 보려면: [`services/ai_request_orchestrator/`](./services/ai_request_orchestrator/)
- 반복 작업과 subagent 설계 근거를 기록하려면: [`instructions/subagent_design_observation_log.md`](./instructions/subagent_design_observation_log.md)

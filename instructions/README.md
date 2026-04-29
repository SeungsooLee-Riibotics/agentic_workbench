# instructions

이 디렉터리는 작업용 instruction 문서를 모아두는 곳이다.
문서가 늘어나면서 PR 관련 파일들이 인접한 역할을 가지게 되었기 때문에, 이 문서는 **무슨 문서를 언제 읽어야 하는지**를 먼저 정리한다.

현재 저장소 전체는 크게 두 축으로 보아야 한다.

- 하나는 이 `instructions/` 디렉터리의 작업 가이드 문서군
- 다른 하나는 루트의 [`ai-request-orchestrator.md`](../ai-request-orchestrator.md) 와 [`services/ai_request_orchestrator/`](../services/ai_request_orchestrator/) 로 이어지는 orchestrator 설계/서비스 자산

즉 이 문서는 저장소 전체 index가 아니라, **두 축 중 instructions 쪽 entrypoint** 역할을 한다.

기본 원칙은 단순하다.

- 파일을 바로 합치거나 이름을 바꾸기보다 먼저 역할을 분류한다.
- 문서가 겹쳐 보여도, 실제 활성 시점이 다르면 separate guide로 유지한다.
- 처음 보는 사람이 “어디부터 읽지?”에서 멈추지 않게 entrypoint를 제공한다.

---

## 빠른 길찾기

### 1. PR을 어떻게 써야 할지 알고 싶다

- [`PR_GUIDE.md`](./PR_GUIDE.md)
- [`pr_description_guide.md`](./pr_description_guide.md)

`PR_GUIDE.md`는 PR의 전체 구조를 잡는 문서고, `pr_description_guide.md`는 PR 설명을 어떤 기준으로 채워야 하는지 더 자세히 다룬다.

### 2. PR 올리기 전에 한 번 더 정리하고 싶다

- [`refine_before_pr.md`](./refine_before_pr.md)

이 문서는 diff를 줄이고, 문서/설명/검증 근거를 정리해서 리뷰 비용을 낮추는 데 초점이 있다.

### 3. 리뷰어 입장에서 이 PR을 어떻게 받을지 판단하고 싶다

- [`pr_review_intake_gate.md`](./pr_review_intake_gate.md)

이 문서는 **문서만으로 판단 가능한 PR인지**, **코드 리뷰가 필요한지**, **문서 패키지가 부족한지**를 먼저 가르는 intake gate다.

### 4. 지금 바로 수정하지 말고 먼저 질문해야 하는 상황이다

- [`ask.md`](./ask.md)

요구사항, 범위, 목적이 불명확할 때 무엇을 확인해야 하는지 정리한 원칙 문서다.

### 5. 시스템 설계 요청 자체를 잘 만들고 싶다

- [`design_request_guide.md`](./design_request_guide.md)

설계 요청을 구조적으로 작성해서 더 좋은 설계 응답을 얻기 위한 템플릿이다.

### 6. CodeRabbit 리뷰 반영 워크플로우가 필요하다

- [`ai_pr_review_apply.md`](./ai_pr_review_apply.md)

자동 리뷰 코멘트를 triage하고, 어떤 항목을 적용/보류/스킵할지 다루는 특화 워크플로우다.

---

## 분류 체계

현재 문서는 아래 5개 범주로 보는 것이 가장 자연스럽다.

### A. PR 설명 기준과 템플릿

- [`PR_GUIDE.md`](./PR_GUIDE.md)
- [`pr_description_guide.md`](./pr_description_guide.md)

PR을 어떤 구조로 쓰고, 리뷰어가 코드를 보기 전에 무엇을 이해해야 하는지 다룬다.

### B. PR Intake / 질문 우선 / 리뷰 진입 판단

- [`pr_review_intake_gate.md`](./pr_review_intake_gate.md)
- [`ask.md`](./ask.md)

둘 다 “바로 진행하지 않고 먼저 가른다”는 공통점이 있다.
차이는 `ask.md`가 범용 원칙이고, `pr_review_intake_gate.md`는 PR 리뷰어용 intake gate라는 점이다.

### C. PR 전 정리와 문서화 hygiene

- [`refine_before_pr.md`](./refine_before_pr.md)

PR을 열기 직전, 불필요한 변경을 제거하고 설명/검증 근거를 정리하는 단계에 해당한다.

### D. 설계 요청 템플릿

- [`design_request_guide.md`](./design_request_guide.md)

PR 문서가 아니라, 설계 요청 자체를 더 잘 만들기 위한 문서다.

### E. 특화된 리뷰 자동화 워크플로우

- [`ai_pr_review_apply.md`](./ai_pr_review_apply.md)

일반 PR 가이드와 다르게, CodeRabbit 리뷰 반영이라는 특정 상황에 맞춘 운영 문서다.

---

## 자주 헷갈리는 경계

### `PR_GUIDE.md` vs `pr_description_guide.md`

- `PR_GUIDE.md`: PR 전체 구조와 섹션을 잡는 상위 가이드
- `pr_description_guide.md`: PR 설명의 품질 기준을 더 세밀하게 다루는 보조 가이드

즉 하나를 대체하는 관계보다, **PR_GUIDE가 뼈대고 pr_description_guide가 품질 기준**에 가깝다.

### `ask.md` vs `pr_review_intake_gate.md`

- `ask.md`: 범용적인 “질문 우선” 원칙
- `pr_review_intake_gate.md`: PR 리뷰어가 문서 충분성을 판정하는 특화 가이드

둘 다 멈추고 판단하는 문서지만, 적용 맥락이 다르다.

### `refine_before_pr.md` vs `pr_review_intake_gate.md`

- `refine_before_pr.md`: 작성자/정리자 입장에서 PR을 다듬는 문서
- `pr_review_intake_gate.md`: 리뷰어 입장에서 PR을 받을 준비가 되었는지 가르는 문서

둘은 반대편에서 같은 PR을 바라본다.

### `design_request_guide.md` vs PR 관련 문서들

`design_request_guide.md`는 PR을 설명하는 문서가 아니라, 설계 요청을 만들기 위한 템플릿이다.
그래서 PR 관련 문서군과 같은 폴더에 있더라도 목적은 별도다.

---

## 추천 읽기 순서

### 작성자 입장

1. [`ask.md`](./ask.md) — 애매하면 먼저 질문
2. [`design_request_guide.md`](./design_request_guide.md) — 설계 요청이 필요한 경우
3. [`PR_GUIDE.md`](./PR_GUIDE.md)
4. [`pr_description_guide.md`](./pr_description_guide.md)
5. [`refine_before_pr.md`](./refine_before_pr.md)

### 리뷰어 입장

1. [`pr_review_intake_gate.md`](./pr_review_intake_gate.md)
2. 필요 시 [`pr_description_guide.md`](./pr_description_guide.md)
3. 특수한 자동 리뷰 반영 상황이면 [`ai_pr_review_apply.md`](./ai_pr_review_apply.md)

### 설계 요청자 입장

1. [`ask.md`](./ask.md)
2. [`design_request_guide.md`](./design_request_guide.md)

---

## 현재 정리 원칙

- 문서 파일은 당분간 그대로 둔다.
- 먼저 이 index에서 역할과 읽는 순서를 분명히 한다.
- 실제로 merge / rename / move가 필요하다고 확인되기 전에는 경로를 흔들지 않는다.

이유는 일부 문서가 다른 문서를 직접 참조하고 있어서, 지금 단계에서는 구조를 바꾸는 것보다 **찾기 쉽게 만드는 것**이 더 이득이 크기 때문이다.

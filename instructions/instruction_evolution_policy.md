# Instruction Evolution Policy

이 문서는 `instructions/` 아래의 작업용 instruction 문서가 **대화를 통해 계속 개선되되, 무분별하게 drift 하지 않도록** 관리하는 기준을 정의한다.

핵심 원칙은 단순하다.

- 대화가 instruction을 **직접 덮어쓰지 않는다**.
- 대화에서 나온 통찰은 먼저 **candidate instruction change** 로 승격된다.
- behavior-changing instruction update는 반드시 **review gate** 를 거친다.
- 최종 반영된 instruction update는 **append-only + revision link** 관점으로 추적 가능해야 한다.

이 문서는 `ai-request-orchestrator.md`의 다음 원칙과 언어를 따른다.

- Decision > Memory
- Explicit Policy > Implicit Learning
- Human-in-the-loop
- Evidence before Output
- Append-only Governance

---

## 목적

이 정책의 목적은 다음 네 가지다.

1. 대화에서 나온 유용한 운영 통찰을 잃지 않는다.
2. 임시 맥락과 durable rule을 구분한다.
3. instruction 문서가 대화 한두 번으로 흔들리지 않게 한다.
4. 어떤 판단으로 instruction이 바뀌었는지 나중에 다시 추적할 수 있게 한다.

---

## 왜 템플릿이 필요한가

이 정책에서 template는 문서를 예쁘게 만들기 위한 도구가 아니다.
핵심 목적은 **대화에서 나온 통찰을 reviewable candidate change로 바꾸는 것**이다.

템플릿이 없으면 아래 문제가 반복된다.

- 무엇을 바꾸자는 것인지 요약이 빠진다.
- 왜 바꾸자는 것인지 rationale이 빠진다.
- 어떤 문서에 영향이 있는지 남지 않는다.
- 특정 세션의 임시 맥락과 일반 규칙 후보가 섞인다.
- review gate가 무엇을 검토해야 하는지 불명확해진다.

반대로 템플릿이 있으면 아래가 가능해진다.

- proposal을 candidate artifact로 남길 수 있다.
- target doc, rationale, evidence, risk를 함께 본다.
- review 결과를 approve / amend / reject / supersede 로 기록하기 쉬워진다.
- instruction drift를 줄일 수 있다.

즉, template는 생각을 더 많이 쓰기 위한 장치가 아니라, **후보 변경안을 검토 가능하게 만들기 위한 최소 형식**이다.

실제 작성 시에는 `instruction_update_proposal_template.md`를 사용한다.

---

## 적용 범위

이 정책은 `instructions/` 아래의 문서 전체에 적용한다.

예:

- `README.md`
- `ask.md`
- `PR_GUIDE.md`
- `pr_description_guide.md`
- `refine_before_pr.md`
- `pr_review_intake_gate.md`
- `ai_pr_review_apply.md`
- `pr_author_prepare_guide.md`

이 정책은 instruction 문서의 **운영 규칙, workflow gate, 역할 분리, 사용 조건, cross-link 구조** 같은 내용을 다룬다.

단순 참고 메모, 일회성 대화 내용, 현재 세션에서만 유효한 작업 상태는 이 정책의 직접 대상이 아니다.

---

## 핵심 구분

instruction 진화 정책을 운영할 때는 아래 4개를 구분해야 한다.

### 1. Conversation State

현재 대화에서만 유효한 임시 맥락이다.

예:

- 지금 무슨 문제를 논의 중인지
- 어떤 예시를 들고 있는지
- 아직 확정되지 않은 가설이 무엇인지

이 상태는 휘발성이며, instruction으로 직접 반영하지 않는다.

### 2. Memory

세션을 넘어 참고할 가치가 있는 정보다.

예:

- 자주 반복되는 사용자 선호
- 반복적으로 발생하는 ambiguity 패턴
- 과거에 유용했던 판단 기준

Memory는 durable할 수 있지만, 그 자체가 instruction은 아니다.

### 3. Candidate Instruction Change

instruction에 반영할 수도 있는 **후보 변경안**이다.

예:

- "요청이 둘 이상의 workflow로 해석되면 항상 lane 선택을 먼저 묻는다"
- "내 PR 준비와 reviewer intake를 한 요청으로 섞지 않는다"

대화에서 나온 통찰은 먼저 이 상태로 들어간다.

### 4. Approved Instruction Change

review를 통과해서 실제 instruction 문서에 반영된 변경이다.

이 상태가 되기 전에는 instruction 문서를 canonical truth로 바꾸지 않는다.

---

## 기본 정책

### 1. Conversation direct-write 금지

대화에서 나온 아이디어를 바로 instruction 문서에 반영하지 않는다.

항상 다음 흐름을 따른다.

`Conversation -> Candidate Instruction Change -> Review -> Approved Instruction Change -> Doc Update`

### 2. Explicit policy 우선

instruction 문서의 기준은 암묵적 감각이나 누적된 분위기가 아니라, **명시된 정책과 리뷰된 변경**이어야 한다.

### 3. Human-in-the-loop 우선

아래는 반드시 review를 거친다.

- workflow 선택 규칙 변경
- author / reviewer / post-PR feedback 경계 변경
- 금지/의무/항상/절대 같은 behavioral rule 변경
- memory 정책 변경
- escalation 조건 변경

### 4. Evidence before instruction update

instruction을 바꾸려면 최소한 다음이 있어야 한다.

- 어떤 문제가 있었는지
- 왜 현재 instruction이 부족했는지
- 어떤 대화/사례에서 반복적으로 드러났는지
- proposed change가 어떤 리스크를 줄이는지

즉, instruction update는 근거 없는 감각이 아니라 **evidence-backed proposal** 이어야 한다.

### 5. Append-only governance

instruction evolution 이력은 덮어쓰기만으로 끝내지 않는다.

가능하면 아래를 남긴다.

- 어떤 변경안이 제안되었는지
- 어떤 변경이 승인되었는지
- 어떤 변경이 어떤 이전 판단을 supersede 하는지

문서 내용은 최신 상태를 보여줄 수 있어도, governance 관점에서는 **revision trail** 이 있어야 한다.

---

## Candidate Instruction Change 조건

아래 조건을 만족할 때만 candidate로 승격한다.

1. 한 번의 우연한 대화가 아니라, **반복성** 또는 일반화 가능성이 있다.
2. 단순 취향이 아니라, 운영 규칙이나 workflow 품질에 영향을 준다.
3. 특정 세션의 임시 맥락이 아니라, 다른 요청에도 적용될 수 있다.
4. 이유와 근거를 짧게라도 설명할 수 있다.

아래는 candidate로 올리지 않는다.

- 현재 세션에서만 필요한 임시 처리
- 단순한 표현 선호 하나
- 아직 합의되지 않은 추측
- 특정 repo / 특정 케이스에만 맞는 과잉 일반화

---

## Candidate Instruction Change 최소 필드

후보 변경안은 최소한 아래 내용을 가져야 한다.

- `summary`: 무엇을 바꾸려는가
- `target_docs`: 어떤 문서에 영향을 주는가
- `rationale`: 왜 바꿔야 하는가
- `source_conversations`: 어떤 대화에서 나왔는가
- `risk_level`: low / medium / high
- `status`: candidate / approved / rejected / superseded

권장 예시:

```json
{
  "summary": "Add explicit workflow choice gate for ambiguous PR-related requests.",
  "target_docs": [
    "instructions/README.md",
    "instructions/pr_author_prepare_guide.md"
  ],
  "rationale": [
    "author-prep and reviewer-review requests were repeatedly mixed",
    "ask-first principle already exists",
    "wrong routing caused avoidable confusion"
  ],
  "source_conversations": ["session_xxx"],
  "risk_level": "medium",
  "status": "candidate"
}
```

이 필드를 매번 자유 형식으로 빼먹지 않기 위해, 실제 작업에서는 `instruction_update_proposal_template.md` 사용을 권장한다.

---

## 자동 반영 허용 범위

아래처럼 **behavior를 바꾸지 않는 low-risk change** 만 자동 반영 가능 대상으로 본다.

- 오탈자 수정
- 깨진 링크 수정
- 명백한 cross-link 누락 보완
- 제목/소제목 정리
- 이미 승인된 rule을 index에 반영하는 기계적 정합성 수정

아래는 자동 반영 금지다.

- 새 gate 추가
- 역할 경계 변경
- instruction lane 선택 규칙 추가/삭제
- 예외 처리 기준 변경
- escalation 조건 변경
- 의미상 금지/의무 규칙 추가

즉, **behavior-changing instruction update는 자동화 대상이 아니다**.

---

## Review Gate

Candidate Instruction Change는 아래 질문을 통과해야 한다.

1. 이 변경은 반복적으로 유용한가?
2. 특정 대화의 임시 맥락을 과잉 일반화한 것은 아닌가?
3. 기존 instruction과 충돌하지 않는가?
4. memory로 남기면 충분한데 instruction까지 올리려는 것은 아닌가?
5. wording이 너무 강하거나 너무 넓지 않은가?
6. 사용자나 agent routing에 unintended side effect를 만들지 않는가?

review 결과는 최소한 아래 중 하나여야 한다.

- `approve`
- `amend`
- `reject`
- `supersede`

---

## Approved change 반영 규칙

Approved change가 되면 그때 instruction 문서를 수정한다.

반영 시 권장 규칙:

1. target doc를 명확히 제한한다.
2. 관련 index 또는 entrypoint 문서도 함께 점검한다.
3. 기존 규칙을 대체한다면 supersede 관계를 남긴다.
4. 관련 문서 간 cross-link를 갱신한다.
5. 필요하면 최종 문구를 더 좁고 명확하게 다듬는다.

---

## Drift 방지 규칙

instruction drift를 막기 위해 아래를 유지한다.

1. direct auto-update 금지
2. candidate 없이 instruction 수정 금지
3. 고위험 변경은 human review 필수
4. memory와 instruction의 역할 혼합 금지
5. obsolete rule은 새 revision으로 supersede
6. 문서 분리는 index와 cross-link 우선, 성급한 rename/move 지양

이 정책은 instruction 문서를 살아 있게 유지하되, **대화의 흔적이 무비판적으로 정책이 되지 않게** 만드는 데 목적이 있다.

---

## Public API 문서화 정책

instruction 문서가 진화할 때는 **어떤 public API가 문서화되어야 하는지**에 대한 기준도 함께 유지해야 한다.

이 섹션에서 public API는 기본적으로 **사용자, 통합 대상, 다른 팀, 또는 downstream caller가 직접 의존할 수 있는 외부 노출 contract** 를 뜻한다. private helper, purely internal hook, 테스트 전용 구현 세부는 이 정책의 직접 대상이 아니다.

핵심 원칙은 아래 한 줄로 요약할 수 있다.

> 사용자가 추측하면 안 되는 public API contract는 문서에 들어가야 한다.

여기서 contract는 단순 함수 시그니처가 아니라, **호출 조건 / 상태 변화 / side effect / 오용 시 의미**를 포함한다.

### 문서화가 반드시 필요한 경우

아래 중 하나라도 해당하면 README 또는 동등한 사용자 문서와 header comment에 contract를 명시하는 것을 기본으로 본다.

1. 객체 lifecycle을 바꾼다.
2. reset / clear / shutdown / initialize semantics가 있다.
3. 호출 후 내부 상태 또는 외부 observable behavior가 크게 바뀐다.
4. 무엇이 유지되고 무엇이 초기화되는지 사용자가 추측하면 안 된다.
5. 호출 순서가 중요하다.
6. 잘못 이해하면 bug, crash, data loss, wrong routing으로 이어질 수 있다.
7. 다른 public method와 상호작용이 강하다.

예:

- reusable reset API
- terminal dispose API
- workflow gate attach/detach 함수
- configuration reload 함수
- publish state / registration state를 바꾸는 함수

### 문서에 반드시 들어가야 하는 내용

문서에는 최소한 아래 내용을 포함한다.

#### 1. 목적

- 왜 이 API가 존재하는가?
- 어떤 문제를 해결하는가?

#### 2. 사용 조건

- 언제 호출해야 하는가?
- 호출 전에 만족해야 하는 조건이 있는가?

#### 3. 상태 변화

- 호출 후 무엇이 바뀌는가?
- 무엇이 유지되고 무엇이 초기화되는가?

#### 4. Side effect

- publish / registration / callback / external state에 어떤 영향이 있는가?

#### 5. 오용 시 의미

- 잘못 호출하면 no-op 인가?
- error 인가?
- unsupported / undefined behavior 인가?

#### 6. thread-safety / ownership

- thread-safe 한가?
- lifetime 또는 ownership 가정이 있는가?

#### 7. 예시

- 최소 사용 예시 하나
- 오해하기 쉬운 경우가 있으면 짧은 주의 예시 하나

### 반대로 문서에 굳이 넣지 않아도 되는 것

아래는 public API 문서의 핵심 대상이 아니다.

- trivial getter / setter 수준의 자명한 동작
- private helper 구현 상세
- 바뀌기 쉬운 내부 자료구조 설명
- 코드가 이미 명확히 보여주는 저수준 구현 절차
- diff 재서술 수준의 설명

즉 문서는 code narration이 아니라 **usage contract** 중심이어야 한다.

### 문서 층위 원칙

public API contract는 가능하면 아래 3층으로 남긴다.

1. **header comment**
   - 가장 정확한 API contract
   - precondition / postcondition / invariant

2. **README 또는 사용자 문서**
   - 사용자 관점 설명
   - 언제 쓰는지
   - 어떤 상태 모델인지
   - 예시

3. **tests**
   - executable spec
   - 문서에서 약속한 contract를 고정

### 운영 규칙

public API가 아래 성격을 가지면, 해당 API의 문서화 상태를 review 항목으로 본다.

- lifecycle 의미가 있다
- reset/clear semantics가 있다
- state transition을 만든다
- observable side effect가 있다
- ordering / safety semantics가 중요하다

즉 instruction evolution candidate가 public API를 바꾸는 경우에는,
"코드가 맞는가" 뿐 아니라 **"문서 contract가 충분한가"** 도 함께 review 해야 한다.

### 실무 적용 요약

한 줄 요약:

> public API의 결과를 사용자가 추측하면 안 되면 문서화한다.

그리고 그 문서화는 가능하면 README + header + tests 세 층위로 맞춘다.

---

## 기존 instruction 문서와의 관계

- `ask.md`
  - ambiguity가 있으면 먼저 질문한다는 원칙을 제공한다.
- `pr_author_prepare_guide.md`
  - author flow와 reviewer flow 분리 규칙을 제공한다.
- `pr_review_intake_gate.md`
  - reviewer 입장의 판정 gate 예시를 제공한다.
- `ai_pr_review_apply.md`
  - 이미 열린 PR에서 post-PR feedback을 처리하는 특화 workflow를 제공한다.

즉, 이 문서는 개별 workflow 자체를 설명하는 문서가 아니라, **그 workflow 문서들이 어떻게 진화해야 하는지**를 설명하는 상위 정책 문서다.

---

## 실무 적용 요약

한 줄로 요약하면 아래와 같다.

> conversation은 instruction을 직접 바꾸지 않고, instruction change proposal을 만든다.

그리고 그 proposal은

> evidence-backed candidate -> review -> approved change

를 거친 뒤에만 instruction 문서를 바꾼다.

이 원칙을 지키면 instruction은 계속 발전할 수 있지만, 동시에 governance와 auditability도 유지된다.

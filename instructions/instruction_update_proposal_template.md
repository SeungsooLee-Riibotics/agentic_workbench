# Instruction Update Proposal Template

이 문서는 `instructions/` 아래의 instruction 문서를 바꾸고 싶을 때 사용하는 **최소 proposal 템플릿**이다.

이 템플릿의 목적은 단순하다.

- 대화에서 나온 통찰을 바로 instruction에 반영하지 않고
- 먼저 **reviewable candidate change** 로 만들고
- 이후 `instruction_evolution_policy.md`의 review gate를 거쳐 승인할 수 있게 한다.

아주 작은 low-risk 수정(오탈자, 링크 수정, 기계적 정합성 수정)에는 이 템플릿이 꼭 필요하지 않을 수 있다.
하지만 behavioral rule, workflow gate, 역할 경계, escalation 조건처럼 **instruction 의미를 바꾸는 수정**에는 이 템플릿 사용을 권장한다.

이 템플릿은 `design_request_guide.md`처럼 새로운 시스템 설계를 요청하는 문서가 아니다.
목적은 **기존 instruction 문서에 대한 변경 제안(candidate change)** 을 reviewable artifact로 남기는 데 있다.

---

## 복붙용 최소 템플릿

```md
## Instruction Update Proposal

### 1. Summary
- 무엇을 바꾸려는가?

### 2. Target Docs
- 어떤 instruction 문서에 영향을 주는가?
- 예: `instructions/README.md`, `instructions/pr_author_prepare_guide.md`

### 3. Problem Signal
- 어떤 문제/혼란/반복 패턴이 있었는가?
- 이 수정이 왜 필요해졌는가?

### 4. Proposed Change
- 구체적으로 어떤 규칙/문구/구조를 바꾸려는가?
- 가능하면 before / after 형태로 적는다.

### 5. Rationale / Evidence
- 어떤 대화, 사례, 반복 패턴, 근거가 이 변경을 지지하는가?

### 6. Impact / Risk
- 이 변경이 줄이는 리스크는 무엇인가?
- 새로 생길 수 있는 side effect는 무엇인가?

### 7. Source Conversations
- 이 제안이 나온 대화/세션은 무엇인가?
- 예: `session_xxx`, `session_yyy`

### 8. Suggested Status
- `candidate`
- 필요하면 `risk_level`: low / medium / high
```

---

## 더 짧은 버전

아주 짧게 남기려면 아래 5개만 있어도 된다.

```md
- Summary:
- Target Docs:
- Problem Signal:
- Proposed Change:
- Rationale:
- Source Conversations:
```

---

## 대화형 확인 단계

proposal 초안이 만들어졌다면, agent는 target instruction 문서를 바로 수정하지 않는다.
먼저 사용자에게 짧은 확인 질문을 던져서 proposal을 어떻게 처리할지 정한다.

기본 질문은 아래 순서로 한다.

1. 이 proposal을 `candidate`로 남길지, 수정할지, 폐기할지 확인한다.
2. `Target Docs`, `Problem Signal`, `Proposed Change`, `Impact / Risk`가 맞는지 확인한다.
3. 사용자가 바로 review하길 원하면 `approve`, `amend`, `reject`, `supersede` 중 하나를 선택하게 한다.
4. `approve` 또는 `amend`가 명확해진 뒤에만 target instruction 문서 수정을 제안하거나 수행한다.

질문은 길게 하지 않는다.
가능하면 아래처럼 한 번에 답하기 쉬운 형태로 묻는다.

```md
이 proposal을 어떻게 처리할까요?

- `candidate`: 후보로만 남김
- `amend`: 내용 수정 후 다시 보여줌
- `approve`: 승인하고 target docs 반영으로 진행
- `reject`: 폐기
```

사용자가 아직 판단하지 않았으면 기본값은 `candidate`다.
이 단계의 목적은 proposal 생성을 canonical instruction update와 분리해서, review gate를 대화 안에서 명확히 남기는 것이다.

---

## 작성 예시

```md
## Instruction Update Proposal

### 1. Summary
- PR 관련 요청이 author flow / reviewer flow / post-PR feedback flow로 동시에 해석될 수 있으면 먼저 workflow lane 선택을 묻게 한다.

### 2. Target Docs
- `instructions/README.md`
- `instructions/pr_author_prepare_guide.md`
- `instructions/instruction_evolution_policy.md`

### 3. Problem Signal
- "PR 좀 봐줘" 같은 요청이 반복적으로 애매했고, 준비 단계와 리뷰 단계가 섞여 잘못된 instruction으로 시작되는 경우가 있었다.

### 4. Proposed Change
- ambiguous request는 instruction을 바로 attach하지 않고 workflow choice gate를 먼저 거치게 한다.

### 5. Rationale / Evidence
- `ask.md`는 모호하면 먼저 질문하라고 정의한다.
- 최근 author/reviewer/post-PR feedback 문서를 분리하면서 lane ambiguity가 실제로 드러났다.

### 6. Impact / Risk
- 잘못된 instruction routing을 줄인다.
- 대신 아주 짧은 요청에서는 한 번 더 선택을 묻는 friction이 생길 수 있다.

### 7. Source Conversations
- `session_xxx`

### 8. Suggested Status
- `candidate`
- `risk_level`: medium
```

---

## 사용 규칙

1. 이 템플릿은 **conversation을 바로 instruction으로 바꾸는 용도**가 아니다.
2. 이 템플릿으로 남긴 내용은 기본적으로 `candidate instruction change` 다.
3. canonical instruction update는 별도 review 후에만 반영한다.
4. wording이 강할수록(`항상`, `절대`, `반드시`, `금지`) rationale을 더 명확히 적는다.
5. `Suggested Status`는 기본적으로 `candidate`를 사용한다. 이후 review 결과는 정책 문서 기준으로 `approve`, `amend`, `reject`, `supersede` 중 하나로 정리한다.
6. public API를 바꾸는 제안이라면, `instruction_evolution_policy.md`의 **Public API 문서화 정책** 기준에 맞춰 header comment / README / tests까지 함께 점검한다.

자세한 정책은 `instruction_evolution_policy.md`를 따른다.

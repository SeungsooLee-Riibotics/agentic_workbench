# Subagent Design Observation Log

이 문서는 며칠간 실제 작업을 하면서 어떤 작업이 반복되고, 어떤 판단이 독립 subagent나
instruction 개선으로 분리될 가치가 있는지 기록하는 evidence log다.

## 역할

- canonical instruction이 아니다.
- 특정 대화의 임시 memory도 아니다.
- 반복 작업, 판단 기준, 병목, 위임 가능성을 모아두는 관찰 로그다.
- 충분한 사례가 쌓이면 `instruction_update_proposal_template.md` 형식의 proposal이나
  subagent role 설계로 승격할지 판단한다.

## 기록 기준

작업이 끝났을 때 아래 중 하나라도 해당하면 짧게 남긴다.

- 비슷한 판단이 반복됐다.
- agent가 오래 탐색했지만 결과적으로 명확한 gate나 checklist가 있었다.
- 별도 subagent가 병렬로 처리하면 도움이 될 만한 독립 작업이 있었다.
- memory에 남길 개인 선호와 canonical instruction으로 승격할 규칙이 헷갈렸다.
- PR 리뷰, 문서화, 설계 검토에서 재사용 가능한 관점이 드러났다.

## Entry Template

```md
### YYYY-MM-DD - <workspace / task>

- Context:
- Goal:
- What happened:
- Decision made:
- Friction:
- Reusable pattern:
- Candidate update:
- Evidence:
```

## Current Hypotheses To Validate

- PR 리뷰 시작 전에 README/PR description만으로 판단 가능한지 가르는 intake reviewer가 유용할 수 있다.
- package boundary, data flow, external control surface가 바뀌는 PR은 문서화 gate를 별도로 두는 편이 리뷰 비용을 줄일 수 있다.
- memory는 개인 선호와 임시 context에, instruction proposal은 반복 가능한 행동 규칙에 써야 한다.
- subagent는 "작업 전체"보다 병렬로 분리 가능한 증거 수집, 문서 충분성 판단, 테스트 검증 같은 좁은 역할일 때 효과가 클 가능성이 높다.

## Observations

### 2026-04-30 - calibration_modules PR 11 README review

- Context: 다른 작성자의 `Riibotics/calibration_modules` PR 11을 코드 리뷰하기 전에 README 문서화가 충분한지 먼저 확인했다.
- Goal: 코드 리뷰에 들어가기 전에 외부 사용자와 calibration manager 관점에서 문서가 충분한지 판단한다.
- What happened: PR 설명에는 Action/Service 제어 흐름이 비교적 잘 적혀 있었지만 README에는 반영되어 있지 않았다. Data Flow도 pipeline input adaptor가 lidar 데이터를 받는다는 수준에서 멈춰 있었고, 이후 데이터가 calibration module까지 어떻게 전달되는지와 calibration manager/module 패키지 경계가 충분히 드러나지 않았다.
- Decision made: 코드 리뷰를 계속하기 전에 README에 Data Flow와 동작 다이어그램을 먼저 보강해 달라고 요청했다.
- Friction: PR 설명과 README가 서로 다른 수준의 정보를 담고 있어서, 외부 사용자가 실제 README만 보고 시스템을 이해할 수 있는지 별도로 확인해야 했다.
- Reusable pattern: package boundary, data flow, Action/Service 같은 external control surface가 바뀌는 PR은 README에 사용자 관점의 데이터 흐름과 동작 흐름이 반영되어야 한다.
- Candidate update: [`instruction_update_proposal_pr_review_data_flow_docs.md`](./instruction_update_proposal_pr_review_data_flow_docs.md)
- Evidence: README-level documentation gate는 full code review 전에 독립적으로 판단 가능한 작업이므로, future subagent 후보가 될 수 있다.

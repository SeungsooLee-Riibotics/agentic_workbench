# Instruction Update Proposal: PR Review Data Flow Documentation Gate

## 1. Summary

- Add an explicit reviewer gate for PRs that change package boundaries, data flow, or external control surfaces.
- Require README-level documentation of data flow and operation flow before the PR can be treated as documentation-complete.

## 2. Target Docs

- `instructions/pr_review_intake_gate.md`
- Potential follow-up: `instructions/pr_description_guide.md`
- Potential follow-up: `instructions/pr_author_prepare_guide.md`

## 3. Problem Signal

- During review of `Riibotics/calibration_modules` PR #11, the PR body explained the new stage/action/service flow, but the README did not give an external user enough information to understand the runtime system.
- The code split `calibration_manager` and `calibration_modules` into clearer package responsibilities, but the docs did not clearly explain what data crosses that package boundary.
- The README mentioned `PipelineInputAdaptor` receiving LiDAR data, but did not explain where the data flows next, what `calibration_modules` receives, or what the action result contains.
- Action and service based control were described in the PR, but not carried into the durable project README.

## 4. Proposed Change

Add a review gate to `pr_review_intake_gate.md`:

- If a PR changes a package boundary, module boundary, or cross-package contract, the documentation package must identify:
  - producer and consumer packages/modules,
  - data structures crossing the boundary,
  - direction of flow,
  - ownership of transformation/adaptation logic,
  - expected outputs.
- If a PR changes an external control surface such as ROS action, service, lifecycle, CLI, API endpoint, or launch behavior, the README or equivalent user document must include an operation flow that an external user can follow.
- For stage/pipeline/control-flow changes, prefer a maintainable diagram or sequence-style section in durable docs, not only in the PR body.

Suggested reviewer rule:

```md
If the PR introduces or changes a public runtime flow, package boundary, or external control surface, mark the documentation package as insufficient unless README-level docs explain both:

1. Data Flow: where external inputs enter, how they cross package/module boundaries, which data structures are exchanged, and what outputs are produced.
2. Operation Flow: how an external user controls the system through action/service/lifecycle/API/CLI surfaces, including the point where the flow completes or hands off to the next stage.
```

## 5. Rationale / Evidence

- Durable docs should serve the next external user/operator, not only the current reviewer reading the PR description.
- Package boundary changes are high-friction to recover from by reading code alone; the integration contract should be visible in README-level docs.
- Runtime control flows often fail in practice when the completion trigger or service/action sequence is implicit.
- This aligns with the existing `instruction_evolution_policy.md` public API documentation policy: users should not have to guess public contracts.

## 6. Impact / Risk

- Reduces review ambiguity for architecture and integration PRs.
- Helps reviewers request targeted README updates instead of broad "please document more" feedback.
- Adds some documentation burden to PR authors, so wording should be scoped to public/runtime/package-boundary changes, not every internal refactor.

## 7. Source Conversations

- 2026-04-30 review discussion around `Riibotics/calibration_modules` PR #11.

## 8. Suggested Status

- `candidate`
- `risk_level`: medium

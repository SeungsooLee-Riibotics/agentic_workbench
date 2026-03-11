## CodeRabbit PR Triage Workflow

Use this workflow when the user asks to handle CodeRabbit feedback in a pull request.

1. Identify target repository and PR
- Treat PR target as either root repo or one submodule repo.
- Accept user input in one of these forms:
  - `<pr-number>` for current repo context.
  - `<owner>/<repo>#<pr-number>` for explicit repo.
  - `<submodule-path>#<pr-number>` for explicit submodule path (for example `software/riibotics_nav#123`).
- If target is ambiguous, ask user to choose one repo/path and stop.
- If target is submodule, run git operations in that submodule (`git -C <submodule-path> ...`) and run `gh` with explicit repo (`gh ... --repo <owner>/<repo>`).
- If PR number is missing, detect from current branch context:
  - root repo: `gh pr status` / `gh pr view`
  - submodule repo: `git -C <submodule-path> ...` plus `gh ... --repo <owner>/<repo>`
- If still unknown, ask the user for PR number and stop.

2. Collect CodeRabbit comments
- Query PR review comments/threads with `gh`.
- Filter to author `coderabbitai` or `coderabbitai[bot]`.
- Prioritize unresolved threads. If unresolved status is unavailable, treat all filtered comments as pending.
- Summarize each item with: `id`, `file:line` (if present), issue summary, suggested fix.

3. Require per-item user decision
- Ask the user for each item: `apply`, `skip`, or `hold`.
- Do not change code for any item without explicit `apply`.

4. Implement approved items only
- Make minimal targeted edits.
- Run relevant checks/tests when possible.
- If checks fail, report failure and proposed fix before proceeding.

5. Commit and push with explicit final approval
- Show a concise change summary before commit.
- Ask one final confirmation before running commit/push.
- Commit message default: `fix(pr): address approved CodeRabbit review comments`.
- Commit and push in the same repository that owns the PR:
  - root PR: commit/push in root repo
  - submodule PR: commit/push inside the submodule repo path
- Push to the PR head branch and report the branch/PR URL.

6. Report outcome
- List `applied`, `skipped`, and `held` comment IDs in the final report.
- Only post a PR comment via `gh pr comment` if the user explicitly asks for it.

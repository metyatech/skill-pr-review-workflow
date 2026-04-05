---
name: pr-review-workflow
description: Use when addressing PR review feedback, re-requesting reviews, handling GitHub review bot re-reviews (Codex, Copilot), or managing GitHub notifications. Do not use for creating new PRs or general development.
---

# PR review feedback workflow

## Addressing review feedback

- After addressing PR review feedback, resolve the corresponding review thread(s) before concluding; if you lack permission, state it explicitly.
- Before re-requesting review after addressing feedback, run the relevant verification suite and summarize results (commands + outcomes) in the PR comment/description.
- For feedback caused by an agent-originated miss or a reusable reasoning gap, explicitly assess whether recurrence prevention belongs in tests/checks, skills, or rules. When a rule/skill update is warranted, make that update as part of the same change set; when it is not warranted, state briefly why the earliest reusable gate is elsewhere.

## Re-requesting reviews

- After pushing fixes for PR review feedback, re-request review only from reviewer(s) who posted the addressed feedback in the current round.
- Do not re-request review from reviewers (including AI reviewers) who did not post addressed feedback, or who already indicated no actionable issues.
- If no applicable reviewer remains, ask who should review next.

## AI review bot re-review

When Codex and/or Copilot review bots are configured for the repo, trigger re-review only for the bot(s) that posted addressed feedback.

- For Codex re-review (only when applicable): comment `@codex review` on the PR.
- For Copilot re-review (only when applicable): use the GitHub API to remove and re-request the bot reviewer `copilot-pull-request-reviewer[bot]` (do not rely on UI-based reviewer assignment).
  - Remove: `DELETE /repos/{owner}/{repo}/pulls/{pr}/requested_reviewers` with body `{"reviewers":["copilot-pull-request-reviewer[bot]"]}`
  - Add: `POST /repos/{owner}/{repo}/pulls/{pr}/requested_reviewers` with body `{"reviewers":["copilot-pull-request-reviewer[bot]"]}`

## GitHub notification management

- Use `DELETE /notifications/threads/{id}` (HTTP 204) to mark notifications as **done** (removes from inbox/moves to Done tab).
- Do NOT use `PATCH /notifications/threads/{id}` (marks as read but leaves in inbox).
- After processing notifications, bulk-delete any remaining read-but-not-done notifications with the same DELETE API.

## Post-merge cleanup

- After completing a PR, merge it, sync the target branch, and delete the PR branch locally and remotely.

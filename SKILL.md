---
name: pr-review-workflow
description: Use when addressing PR review feedback, re-requesting reviews, handling GitHub review bot re-reviews (Codex, Copilot), or managing GitHub notifications. Do not use for creating new PRs or general development.
---

# PR review feedback workflow

## Addressing review feedback

- After addressing PR review feedback, the agent MUST resolve the
  corresponding review threads before concluding. If the agent
  lacks permission, the agent MUST state it explicitly.
- Before re-requesting review, the agent MUST run the relevant
  verification suite and MUST summarize results (commands and
  outcomes) in the PR comment or description.
- For feedback caused by an agent-originated miss or a reusable
  reasoning gap, the agent MUST assess whether recurrence
  prevention belongs in tests, checks, skills, or rules. When a
  rule or skill update is warranted, the agent MUST make the
  update as part of the same change set. When it is not
  warranted, the agent MUST briefly state why the earliest
  reusable gate is elsewhere.

## Re-requesting reviews

- After pushing fixes, the agent MUST re-request review only from
  reviewers who posted addressed feedback in the current round.
- The agent MUST NOT re-request review from reviewers (including
  AI reviewers) who did not post addressed feedback or who
  already indicated no actionable issues.
- If no applicable reviewer remains, the agent MUST ask who
  should review next.

## AI review bot re-review

When Codex or Copilot review bots are configured for the
repository, the agent MUST trigger re-review only for the bot(s)
that posted addressed feedback.

- For Codex re-review: comment `@codex review` on the PR.
- For Copilot re-review: use the GitHub API to remove and
  re-request the bot reviewer
  `copilot-pull-request-reviewer[bot]`. The agent MUST NOT rely
  on UI-based reviewer assignment.
  - Remove:
    `DELETE /repos/{owner}/{repo}/pulls/{pr}/requested_reviewers`
    with body
    `{"reviewers":["copilot-pull-request-reviewer[bot]"]}`.
  - Add:
    `POST /repos/{owner}/{repo}/pulls/{pr}/requested_reviewers`
    with body
    `{"reviewers":["copilot-pull-request-reviewer[bot]"]}`.

## GitHub notification management

- The agent MUST use `DELETE /notifications/threads/{id}`
  (HTTP 204) to mark notifications as **done** (removes the
  notification from the inbox and moves it to the Done tab).
- The agent MUST NOT use `PATCH /notifications/threads/{id}`,
  which marks as read but leaves the notification in the inbox.
- After processing notifications, the agent MUST bulk-delete any
  remaining read-but-not-done notifications using the same
  DELETE API.

## Post-merge cleanup

- After completing a PR, the agent MUST merge it, sync the target
  branch, and delete the PR branch locally and remotely.

# PR Review Workflow Scenarios

This file contains example scenarios and expected actions for the `pr-review-workflow` skill. Use these to verify that an agent follows the skill's logic correctly.

## Scenario 1: Addressing review feedback from multiple reviewers

**Context:**

- PR #123 has reviews from `@user-a` (Changes Requested) and `@user-b` (Commented).
- Both reviewers have open threads.
- `@copilot-pull-request-reviewer[bot]` also has a comment.

**User Prompt:**

"I've addressed all the feedback. Please resolve the threads and re-request review."

**Expected Agent Actions:**

1. Resolve Threads: For each addressed thread, call `PATCH /repos/{owner}/{repo}/pulls/comments/{comment_id} {"resolved":true}`.
2. Verify: Run `npm run verify` or similar to ensure the fixes are correct.
3. Summarize: Post a summary of verification results in a PR comment.
4. Selective Re-request:
   - Re-request `@user-a` (because they requested changes).
   - Do NOT re-request `@user-b` (if they only commented and didn't request changes, unless the agent determines their comment requires re-review).
5. Trigger Bot:
   - For `@copilot-pull-request-reviewer[bot]`, remove and re-request if its feedback was addressed.

## Scenario 2: Handling Copilot re-review

**Context:**

- PR #123 has a `CHANGES_REQUESTED` review from `@copilot-pull-request-reviewer[bot]`.
- All feedback has been addressed.

**Expected Agent Actions:**

1. Remove Reviewer: `DELETE /repos/{owner}/{repo}/pulls/123/requested_reviewers {"reviewers":["copilot-pull-request-reviewer[bot]"]}`.
2. Add Reviewer: `POST /repos/{owner}/{repo}/pulls/123/requested_reviewers {"reviewers":["copilot-pull-request-reviewer[bot]"]}`.
3. Result: This triggers the bot to re-evaluate the PR.

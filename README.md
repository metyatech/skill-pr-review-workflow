# skill-pr-review-workflow

Agent skill for handling PR review feedback loops, re-requesting reviews, and managing GitHub notifications.

## What it covers

- Resolving review threads after addressing feedback
- Re-requesting reviews (selective reviewer logic)
- AI review bot re-review (Codex, Copilot)
- GitHub notification management (done vs read)

## Installation

```sh
npx skills add metyatech/skill-pr-review-workflow
```

## Usage

### Claude Code

```text
/pr-review-workflow
```

### Codex

```text
$pr-review-workflow
```

## Testing

See [tests/scenarios.md](tests/scenarios.md) for example scenarios and expected agent actions. These can be used to manually verify the skill's logic or as a reference for implementing automated tests.

## License

MIT

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

## Development

### Verification

Run the full verification suite (linting and formatting checks):

```sh
npm run verify
```

### Formatting

Format all files:

```sh
npm run format
```

### Linting

Lint markdown files:

```sh
npm run lint
```

## License

MIT

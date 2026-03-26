<p align="center">
  <img src="logo.png" width="64" height="64" alt="InariWatch" />
</p>

<h1 align="center">InariWatch Risk Assessment</h1>

<p align="center">
  AI-powered pre-deploy risk assessment on every pull request.<br/>
  Analyzes your PR diff and posts a risk score comment.
</p>

<p align="center">
  <a href="https://github.com/marketplace/actions/inariwatch-risk-assessment">Marketplace</a> ·
  <a href="https://inariwatch.com">Website</a> ·
  <a href="https://inariwatch.com/docs">Docs</a>
</p>

## Usage

```yaml
# .github/workflows/inariwatch.yml
name: InariWatch Risk Assessment
on:
  pull_request:
    types: [opened, synchronize]

jobs:
  risk:
    runs-on: ubuntu-latest
    permissions:
      pull-requests: write
      contents: read
    steps:
      - uses: actions/checkout@v4
      - uses: orbita-pos/inariwatch-action@v1
        with:
          ai-key: ${{ secrets.AI_KEY }}
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## What it does

On every PR, InariWatch:

1. Reads the diff and file changes
2. Sends them to your AI provider for analysis
3. Posts a risk assessment comment on the PR

The comment includes a risk level and specific findings:

> ## InariWatch Risk Assessment
>
> **Risk Level:** 🟢 Low
>
> ### Summary
> Documentation-only changes with no impact on production code.
>
> ### Findings
> - No specific risks identified
>
> ### Recommendations
> - No additional checks needed

When you push new commits, the existing comment is updated (no spam).

## Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `ai-key` | Yes | — | AI API key (Claude, OpenAI, Grok, DeepSeek, or Gemini) |
| `model` | No | auto-detected | AI model to use |
| `min-lines` | No | `5` | Minimum lines changed to trigger assessment |

## Outputs

| Output | Description |
|--------|-------------|
| `risk-level` | `low`, `medium`, or `high` |

## Supported AI Providers

| Provider | Key prefix | Default model |
|----------|-----------|---------------|
| Claude (Anthropic) | `sk-ant-` | claude-sonnet-4-5 |
| OpenAI | `sk-` | gpt-4o-mini |
| Grok (xAI) | `xai-` | grok-beta |
| DeepSeek | `sk-` (pass `model` input) | deepseek-chat |
| Gemini (Google) | `AIza` | gemini-2.0-flash |

## How it works

- Uses `GITHUB_TOKEN` to read PR data and post comments
- Calls your AI provider directly (BYOK — your key, your costs)
- Typical cost: ~$0.001 per PR assessment with GPT-4o-mini
- No data sent to InariWatch servers — everything stays between GitHub and your AI provider

## Part of InariWatch

This action is part of [InariWatch](https://inariwatch.com) — an AI-powered monitoring platform that detects production errors, writes code fixes, and opens PRs automatically.

- [Website](https://inariwatch.com)
- [Documentation](https://inariwatch.com/docs)
- [Capture SDK](https://www.npmjs.com/package/@inariwatch/capture)
- [GitHub](https://github.com/orbita-pos/inariwatch)

## License

MIT

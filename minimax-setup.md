# Using MiniMax with Claude Code

MiniMax provides an Anthropic-compatible API endpoint, allowing you to use MiniMax models (e.g. M2.7) as a drop-in replacement for Claude models in Claude Code — at a fraction of the cost.

## Prerequisites

- Claude Code installed (`npm install -g @anthropic-ai/claude-code`)
- A MiniMax API key from the [MiniMax Developer Platform](https://platform.minimax.io)

## Setup

### 1. Clear Existing Anthropic Environment Variables

Before configuring, ensure you clear any existing Anthropic-related environment variables to avoid conflicts:

```bash
unset ANTHROPIC_AUTH_TOKEN ANTHROPIC_API_KEY
```

Also remove them from your shell profile (`~/.bashrc`, `~/.zshrc`, etc.) if they're set there.

### 2. Configure Claude Code

Edit or create the Claude Code configuration file at `~/.claude/settings.json`:

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.minimax.io/anthropic",
    "ANTHROPIC_AUTH_TOKEN": "<YOUR_MINIMAX_API_KEY>",
    "API_TIMEOUT_MS": "3000000",
    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": "1",
    "ANTHROPIC_MODEL": "MiniMax-M2.7",
    "ANTHROPIC_SMALL_FAST_MODEL": "MiniMax-M2.7",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "MiniMax-M2.7",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "MiniMax-M2.7",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "MiniMax-M2.7"
  }
}
```

> **Note:** For users in China, use `https://api.minimaxi.com/anthropic` as the base URL instead.

### 3. Restart Terminal and Launch

Open a fresh terminal and run:

```bash
claude
```

Verify the model is active by typing `/status` or `/model`.

## Switching Back to Official Claude

If you have a Claude Pro subscription, you can switch back anytime:

1. Type `/model` inside Claude Code
2. Select the default Claude model (e.g. Opus 4.6)

## Pricing

| | Input (per M tokens) | Output (per M tokens) |
|---|---|---|
| **MiniMax M2.7** | $0.30 | $1.20 |
| **Claude Opus 4.6** | $5.00 | $25.00 |

## Tips

- If `~/.claude` folder doesn't exist yet, run `claude` once, then quit with `Ctrl+C` twice — the folder will be created automatically.
- Environment variables `ANTHROPIC_AUTH_TOKEN` and `ANTHROPIC_BASE_URL` take priority over `settings.json` — make sure they're not set in your shell profile if you want the config file to take effect.

# Zainab Builds – Claude plugin marketplace

Plugins by Zainab Ali. Website: https://zainab-builds-networking.netlify.app/

## Install

In Claude Code:

```bash
claude plugin marketplace add zainy155/zainab-builds-marketplace
claude plugin install networking@zainab-builds
```

Or inside a session: `/plugin marketplace add zainy155/zainab-builds-marketplace`, then `/plugin install networking@zainab-builds`.

Then run `/networking` to start.

## Plugins

| Plugin | What it does |
|---|---|
| `networking` | Intent-driven company and contact research, with a chat answer plus a Word brief. Reads public web data only and never sends messages. |

## Updating

Bump `version` in both `plugins/networking/.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json`, commit, and push. Users get it with `claude plugin marketplace update zainab-builds`.

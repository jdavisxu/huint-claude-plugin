# Huint Claude Plugin

A Claude Code plugin marketplace that connects Claude to Huint's hosted MCP
server at `https://mcp.huint.io/mcp` for exact-location photo task operations.

Huint sends real people to a GPS coordinate to photograph something and verifies
the result before releasing a bounty. This plugin gives Claude the hosted MCP
config (Supabase OAuth), a Huint skill, and slash commands.

## Install (Claude Code)

```bash
claude plugin marketplace add JDavisxu/Huint-claude-plugin
```

Then `/plugin` → install **huint** → approve the browser OAuth prompt. No API
key is required. First, create or sign into an operator account at
`https://portal.huint.io/login`.

For Claude Desktop / claude.ai, add a custom connector pointing at
`https://mcp.huint.io/mcp` (the plugin is Claude Code; the connector is the
Desktop/web path to the same server and account).

## What's in the plugin

- `plugins/huint/.mcp.json` — hosted Streamable-HTTP MCP server over OAuth.
- `plugins/huint/skills/huint/SKILL.md` — connection checks, location resolution,
  task creation, and submission review, with money/safety guardrails.
- `plugins/huint/commands/` — `/connect`, `/create-task`, `/review`.

## Verify

Ask Claude: **"Check my Huint MCP connection."** It calls `get_connection_status`
and should return `ok: true` for your operator.

`get_capabilities` is Huint's live domain reference (spec version, policy,
rejection codes), not required for tool discovery. Public docs:
`https://huint.io/docs`.

## Layout

```
.claude-plugin/marketplace.json        # marketplace entry
plugins/huint/
  .claude-plugin/plugin.json           # plugin manifest
  .mcp.json                            # hosted MCP server (OAuth)
  skills/huint/SKILL.md                # Huint domain skill
  commands/connect.md · create-task.md · review.md
```

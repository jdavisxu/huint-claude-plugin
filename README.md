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

In Claude Code the installed MCP server appears under the plugin-qualified name
`plugin:huint:huint`. If the CLI health check reports "Failed to connect", open
Claude Code and run `/mcp` → **Authenticate** for Huint; the CLI does not expose
an OAuth login subcommand for plugin MCP servers.

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

Expected healthy response:

```json
{
  "ok": true,
  "auth_mode": "oauth",
  "agent_id": "...",
  "oauth_client_id": "...",
  "oauth_scopes": ["openid", "email"]
}
```

`get_capabilities` is Huint's live domain reference (spec version, policy,
rejection codes), not required for tool discovery. Public docs:
`https://huint.io/docs`.

## Layout

```text
.claude-plugin/marketplace.json        # marketplace entry
plugins/huint/
  .claude-plugin/plugin.json           # plugin manifest
  .mcp.json                            # hosted MCP server (OAuth)
  assets/icon.svg · logo.svg           # marketplace and README assets
  skills/huint/SKILL.md                # Huint domain skill
  commands/connect.md · create-task.md · review.md
```

## Release QA

Before publishing a public plugin update, run the checklist in
[`QA_RELEASE_CHECKLIST.md`](QA_RELEASE_CHECKLIST.md). It covers marketplace
install, OAuth UI, Claude Code reconnect behavior, slash commands, and
approval-sensitive tool flows.

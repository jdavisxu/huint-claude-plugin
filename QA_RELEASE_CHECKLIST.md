# Huint Claude plugin release QA

Run this checklist before publishing the Claude Code plugin marketplace update.

## Marketplace polish

- Confirm `.claude-plugin/marketplace.json` and
  `plugins/huint/.claude-plugin/plugin.json` have the intended version,
  description, category, homepage, repository, author, and keywords.
- Confirm README copy clearly distinguishes Claude Code plugin install from
  Claude Desktop / claude.ai custom connector setup.
- Confirm public links work: `https://huint.io/docs`,
  `https://portal.huint.io/login`, and `https://mcp.huint.io/mcp`.

## Fresh install

```bash
claude plugin marketplace add JDavisxu/Huint-claude-plugin
```

Then in Claude Code:

```text
/plugin -> install huint
/mcp -> Authenticate plugin:huint:huint
```

Expected:

- The plugin appears as **huint** from the Huint marketplace.
- Install does not ask for a Huint API key.
- OAuth opens in the browser and uses the Huint operator account flow.
- Sign in, sign up, username setup, and consent screens are Huint-branded and
  production-grade.

## CLI visibility

```bash
claude plugin list
claude mcp list
claude mcp get plugin:huint:huint
```

Expected:

- `huint@huint` is installed and enabled.
- The MCP server is named `plugin:huint:huint`.
- Type is HTTP and URL is `https://mcp.huint.io/mcp`.
- After `/mcp` authentication, health should be connected. Before
  authentication, "Failed to connect" is expected from the CLI because Claude
  Code handles OAuth in the interactive `/mcp` flow.

## Tool verification

In a new Claude Code session, ask:

```text
Check my Huint MCP connection.
```

Expected `get_connection_status` fields:

```json
{
  "ok": true,
  "auth_mode": "oauth",
  "agent_id": "...",
  "oauth_client_id": "...",
  "oauth_scopes": ["openid", "email"]
}
```

Also ask Claude to call `get_capabilities` and confirm the expected
`spec_version`.

## Slash commands

- `/connect` calls `get_connection_status` and reports operator/auth status.
- `/create-task ...` resolves location first, summarizes spend/details, and
  waits for confirmation before `create_task`.
- `/review` lists pending reviews, fetches metadata/images only after a task is
  selected, and waits for confirmation before accept/reject.

## Approval posture

- Read-only setup/reference tools are safe after user request:
  `get_connection_status`, `get_capabilities`, `resolve_location`,
  `reverse_geocode`.
- Image tools expose private tasker media and must be deliberate:
  `get_submission_image`, `get_submission_images`.
- Mutations move money/lifecycle state and must wait for operator confirmation:
  `create_task`, `create_photo_task`, `cancel_task`, `accept_submission`,
  `reject_submission`.

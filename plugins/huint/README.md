# huint (Claude plugin)

Connects Claude to Huint's hosted MCP server at `https://mcp.huint.io/mcp`.

## What Claude gets

- Hosted Streamable-HTTP MCP configuration with Supabase OAuth (no API key).
- A Huint skill for connection checks, location resolution, task creation, and
  submission review, with explicit money/safety guardrails.
- Slash commands: `/connect`, `/create-task`, `/review`.

## Normal workflow

1. Create or sign into a Huint operator account at
   `https://portal.huint.io/login`.
2. `claude plugin marketplace add JDavisxu/Huint-claude-plugin`, then `/plugin`
   → install **huint** → approve OAuth in the browser.
3. Ask Claude: `Check my Huint MCP connection.` → expect `ok: true`.
4. To create a task: resolve the location first, confirm spend, then create with
   the resolver payload unchanged.
5. To review: list pending reviews, fetch images, then explicitly confirm accept
   or reject.

## Tool safety

Mutations (`create_task`, `cancel_task`, `accept_submission`,
`reject_submission`) move real money/lifecycle state — Claude confirms before
calling them. Image tools expose private tasker media. Read-only connection,
capability, geocoding, status, list, and metadata tools are safe to run on
request. Tool annotations (`readOnlyHint` / `destructiveHint`) are advertised by
the server so Claude can apply the right approval posture.

## QA checklist

1. Operator account created/signed in.
2. Fresh `marketplace add` + `/plugin` install; OAuth completes without API
   secrets.
3. `get_connection_status` → `ok: true`.
4. `get_capabilities` → expected `spec_version`.
5. Read-only tools run without mutation prompts; mutations prompt first.
6. Re-auth works after disconnect (`/mcp` → Authenticate), then
   `get_connection_status`.

Public docs: `https://huint.io/docs`.

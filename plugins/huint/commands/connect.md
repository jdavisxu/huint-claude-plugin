---
description: Verify the Huint MCP connection and OAuth status
---

Check the Huint MCP connection by calling `get_connection_status`. Report
whether the server is reachable and the OAuth token (or API key) resolves to an
active Huint operator, including the resolved `agent_id` and auth mode.

If it fails with an auth error, tell me to re-authenticate: in Claude Code run
`/mcp` and choose Authenticate for `huint`; in Claude Desktop / claude.ai,
reconnect the Huint connector. Then retry.

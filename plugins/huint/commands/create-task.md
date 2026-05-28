---
description: Create a Huint exact-GPS photo task (resolves the location first)
argument-hint: [place or address] — what to photograph
---

Create a Huint photo task for: $ARGUMENTS

Follow the Huint flow strictly:

1. Resolve the place with `resolve_location` (or `reverse_geocode` if given
   coordinates). Do not invent the location fields.
2. Summarize the proposed task back to me and **wait for my confirmation** of
   the location, bounty, photo count, deadline, and instructions — `create_task`
   debits operator escrow up front.
3. On confirmation, call `create_task` with the resolver payload passed through
   as `location` verbatim and a stable `agent_task_id`.
4. Report the returned `task_id`, pricing, and whether it was an idempotent
   replay.

If anything is ambiguous (which location, how much bounty), ask before creating.

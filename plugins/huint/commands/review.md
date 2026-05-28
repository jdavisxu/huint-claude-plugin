---
description: Review pending Huint tasker submissions before accept/reject
---

Help me review pending Huint submissions:

1. Call `list_pending_reviews({ all_agents: true })` and show the queue (task,
   submitted time, seconds until auto-verify, submission counts).
2. For a task I pick, call `get_task_submissions` for metadata, then
   `get_submission_images` so I can see the photos.
3. Recommend accept or reject with a reason, but **do not call
   `accept_submission` or `reject_submission` until I confirm** — they release
   or withhold the tasker's payout.
4. If rejecting, use only a valid judgment `reason_code` from
   `get_capabilities`; never reject for GPS/timestamp/device/photo-count issues.

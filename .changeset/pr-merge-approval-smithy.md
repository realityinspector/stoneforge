---
"@stoneforge/smithy": minor
---

Add GitHub PR merge approval flow for Approve preset. When `merge.requireApproval` is true, the merge steward creates GitHub PRs instead of auto-merging, adds `awaiting_approval` merge status, and polls for PR merge completion.

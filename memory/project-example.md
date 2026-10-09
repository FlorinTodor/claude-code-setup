---
name: project-billing-v2
description: Billing refactor on feat/billing-v2, due end of October 2026
type: project
---

The billing refactor replaces `billing_legacy/` with `src/billing/`. Target: end of October 2026.

**Why:** the old module has no tests and bills twice on retry.

**How to apply:** do not touch `billing_legacy/` except to delete it at the end; new code goes in `src/billing/`. Related: [[feedback-testing]].

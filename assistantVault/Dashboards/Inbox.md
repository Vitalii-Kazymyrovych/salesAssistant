---
type: dashboard
---
# Inbox Dashboard

```dataview
TABLE status, client, created
FROM "Диалоги"
WHERE status = "waiting_for_me"
SORT created ASC
```

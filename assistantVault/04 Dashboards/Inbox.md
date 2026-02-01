---
type: dashboard
---
# Inbox Dashboard

```dataview
TABLE status, customer, created
FROM "Dialogs"
WHERE status = "waiting_for_me"
SORT created ASC
```

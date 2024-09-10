---
tags: 
alias:
---

### LIST

```dataview
LIST 
FROM "book"
WHERE contains(file.name, "深入")
SORT FILE.ctime
```

```dataview
LIST
FROM ""
WHERE contains(tags, "book")
AND contains(file.name, "深入")
```
## 列表


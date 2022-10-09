---
title: to do
date: 2022-10-09T22:16:20+09:00
last_modified_at: 2022-10-10T01:30:05+09:00
---

```dataview
TABLE WITHOUT ID  file.link AS title, tags
FROM -"templates"
SORT file.mtime, tags desc
WHERE contains(tags, "todo")
```

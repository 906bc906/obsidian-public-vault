---
title: to do dataview
date: 2022-10-09T22:16:20+09:00
last_modified_at: 2022-11-22T20:09:20+09:00
---

```dataview
TABLE WITHOUT ID  file.link AS title, tags
FROM -"templates"
SORT file.mtime desc, tags
WHERE contains(tags, "todo")
```
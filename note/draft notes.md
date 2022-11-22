---
title: draft notes
date: 2022-11-17T02:42:36+09:00
last_modified_at: 2022-11-22T20:09:22+09:00
---

```dataview
TABLE WITHOUT ID  file.link AS title
FROM -"templates"
SORT file.mtime desc, tags
WHERE draft
```
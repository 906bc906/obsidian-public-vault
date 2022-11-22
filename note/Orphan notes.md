---
title: Orphan notes
date: 2022-10-10T01:13:57+09:00
last_modified_at: 2022-11-22T20:09:21+09:00
---

```dataview
TABLE WITHOUT ID  file.link AS title
FROM -"templates"
SORT file.mtime desc
WHERE length(file.inlinks)=0
```
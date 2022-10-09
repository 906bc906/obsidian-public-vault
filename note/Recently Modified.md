---
title: Recently Modified
date: 2022-10-10T01:16:16+09:00
last_modified_at: 2022-10-10T01:29:53+09:00
---

```dataview
TABLE WITHOUT ID file.link as "Recently Modified", choice(none(tag),"_",join(tag)) as tag
SORT file.mtime desc
LIMIT 10
```

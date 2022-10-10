---
date: 2022-10-10T01:16:28+09:00
last_modified_at: 2022-10-10T19:11:59+09:00
title: 모든 노트 All notes
---
# 모든 노트 All notes

```dataview
TABLE WITHOUT ID file.link as "Recently Modified", choice(none(tag),"_",join(tag)) as tag
SORT file.mtime desc
```
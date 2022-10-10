---
title: Orphan notes
date: 2022-10-10T01:13:57+09:00
last_modified_at: 2022-10-10T19:12:02+09:00
---

```dataview
list from "" where length(file.inlinks) =0 and length(file.outlinks) = 0
```

```dataview
list from "" where length(file.inlinks) =0 
```
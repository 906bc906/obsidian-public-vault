---
title: Recently Modified
date: 2022-10-10T01:16:16+09:00
last_modified_at: 2022-11-22T20:09:21+09:00
---

```dataview
TABLE WITHOUT ID file.link as "Recently Modified", choice(none(tags),"_",link("to do dataview.md", join(tags))) as tags, choice(draft, link("draft notes.md", "draft"), "") as draft, choice(length(file.inlinks) = 0, link("Typescript.md", "orp"), "") as orp
SORT file.mtime desc
```
```dataview
TABLE WITHOUT ID file.link as "Recently Modified", choice(none(tag),"_",join(tag)) as tag
SORT file.mtime desc
LIMIT 10
```
[tag::[[메인 Main]]]
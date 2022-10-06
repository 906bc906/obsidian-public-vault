```dataview
TABLE WITHOUT ID file.link as "Recently Modified", choice(none(tag),"_",join(tag)) as tag
SORT file.mtime desc
```
[tag::[[메인 Main]]]
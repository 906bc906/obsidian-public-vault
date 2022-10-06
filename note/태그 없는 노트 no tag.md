# 태그 없는 노트 no tag
```dataview
TABLE tags
FROM -"templates"
SORT file.mtime desc
WHERE none(tags)
```
[tag::[[메인 Main]]]

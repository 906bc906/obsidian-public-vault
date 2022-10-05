
=== multi-column-start: ID_y7xa
```column-settings
Number of Columns: 3
Largest Column: standard
Border: false
shadow: false
```

[[아직 안 읽은 책 not read yet]]
```dataview
TABLE WITHOUT ID list(file.link, "- "+join(filter(tags, (x) => all(x != [[책 Book]], x != [[아직 안 읽은 책 not read yet]])))) as ""
FROM -"templates"
SORT file.mtime desc
WHERE contains(tags, [[아직 안 읽은 책 not read yet]])
```

=== end-column ===

[[읽고 있는 책 reading]]
```dataview
TABLE WITHOUT ID list(file.link, "- "+join(filter(tags, (x) => all(x != [[책 Book]], x != [[읽고 있는 책 reading]])))) as ""
FROM -"templates"
SORT file.mtime desc
WHERE contains(tags, [[읽고 있는 책 reading]])
```

=== end-column ===

[[다 읽은 책 done]]
```dataview
TABLE WITHOUT ID list(file.link, "- "+join(filter(tags, (x) => all(x != [[책 Book]], x != [[다 읽은 책 done]])))) as ""
FROM -"templates"
SORT file.mtime desc
WHERE contains(tags, [[다 읽은 책 done]])
```


=== multi-column-end

[tag::[[메인 Main]]]
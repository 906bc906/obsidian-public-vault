<!--Basic Template V0.0.2 Start -->
```dataview
TABLE WITHOUT ID  file.link AS title, tags
FROM -"templates"
SORT file.mtime, tags desc
WHERE contains(tags, [[]])
```
<!--Basic Template V0.0.2 End -->
[tags::[[to do]]]

https://stackoverflow.com/questions/62082123/github-what-is-the-difference-between-template-and-fork-concepts-and-when-to-us

fork는 원본 리포에 종속적이고, template은 아예 새로 만드는 기능인듯, (boilerplate 느낌)
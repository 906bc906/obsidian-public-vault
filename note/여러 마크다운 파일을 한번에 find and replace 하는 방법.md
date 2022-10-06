# 여러 마크다운 파일을 한번에 find and replace 하는 방법
<!--Basic Template V0.0.2 Start -->
```dataview
TABLE WITHOUT ID  file.link AS title, tags
FROM -"templates"
SORT file.mtime, tags desc
WHERE contains(tags, [[]])
```
<!--Basic Template V0.0.2 End -->
[tags::[[obsidian 옵시디언]]]

https://stackoverflow.com/questions/37346481/how-do-i-find-and-replace-all-occurrences-in-all-files-in-visual-studio-code

Obsidian에서는 현재 가능한 방법이 없어 VS code를 이용해야 한다.
# 특정 브랜치만 pull 하기


<!--Basic Template V0.0.2 Start -->
```dataview
TABLE WITHOUT ID  file.link AS title, tags
FROM -"templates"
SORT file.mtime, tags desc
WHERE contains(tags, [[]])
```
<!--Basic Template V0.0.2 End -->
[tags::[[git 깃]]]

TARGET DECK
전체::개발::GIT

---

<!--ankiQ-->

origin remote 로부터 specific branch 만 pull 하려면 어떻게 해야되는가?

<!--ankiA-->

`git pull origin specific`

<!--ankiE-->
<!--ID: 1665034174803-->

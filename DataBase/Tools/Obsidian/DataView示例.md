---
tags: 
alias:
---

### LIST

```dataview
LIST 
FROM "book"
WHERE contains(file.name, "深入")
SORT FILE.ctime
```

```dataview
LIST
FROM ""
WHERE contains(tags, "book")
AND contains(file.name, "深入")
```
## 列表

```
TABLE 已读页数, 总页数, 

"<div style='border-style:solid; border-width:1px; border-color:#AAAAAA; display:flex;'>" + 
"<div align='center' style='padding:5px; min-width:10px; background-color:" +
	choice(percent < 50, "#d5763f", "#a8c373") + "; width:" +
	percent + "%; color:black'>" + 
choice(percent < 30, " </div><div style='padding:5px;'>", "") +
percent + "%</div></div>" AS 阅读进度

FROM #读书笔记 
FLATTEN round(100*已读页数/总页数) as percent
```


```
TABLE 
"<progress value="+ 已读页数 + " max=" + 总页数 + "></progress>" + round(已读页数/总页数 * 100, 2) + "%" AS 阅读进度
FROM #读书笔记 
```

```
"<progress value="+ 已读页数 + " max=" + 总页数 + "></progress>"
```

```
`=round(已读页数/全部页数 * 100, 2) + "%"`
```


---
tags: 
alias:
---
前进的道路永无止境！

```dataviewjs
// Render Title 
dv.span("**Daily Work**")

// Define Data
const calendarData = {
    year: 2023,
    colors: {
        oldGithubGreen11:[
            "hsl(65, 83%, 88%)",
            "hsl(70, 77%, 78%)",
            "hsl(80, 62%, 72%)",
            "hsl(95, 52%, 66%)",
            "hsl(112, 45%, 61%)",
            "hsl(125, 43%, 56%)",
            "hsl(132, 41%, 49%)",
            "hsl(132, 45%, 43%)",
            "hsl(132, 49%, 36%)",
            "hsl(132, 54%, 29%)", 
            "hsl(132, 59%, 24%)",
        ]
    },
    entries: [],
    showCurrentDayBorder: true,
    intensityScaleEnd: 50
}

// Get files data
for (let group of dv.pages().where(p=>p.file.cday).groupBy(p=>p.file.cday)) {
	calendarData.entries.push({
		date: group.key.toFormat("yyyy-MM-dd"),
		intensity: group.rows.length
	})
}

renderHeatmapCalendar(this.container, calendarData)


```



[[WorkDesk.canvas]]
https://github.com/labuladong/fucking-algorithm


https://juejin.cn/post/6876968255597051917
https://juejin.cn/post/6844904147712475149
https://juejin.cn/post/6979882374725107720

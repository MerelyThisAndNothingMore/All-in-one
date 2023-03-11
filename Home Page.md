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
    showCurrentDayBorder: true
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




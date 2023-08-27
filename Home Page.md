
```dataviewjs
await dv.view('/Resources/CustomView/Heatmap', {type: 'cday'})

```

Turing Complete

大学之道，在明明德，在亲民，在止于至善。
前进的道路永无止境！

- Chalase抓包配置

[[VAM]]
[[羽毛球]]
[[Swim]]
[[Articulation]]
[[工作]]
[[Unity]]
[[2023搬家事宜]]
- 财报学习
- 德州学习

[[WorkDesk.canvas]]

https://broadgeek.com/about/

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

<https://labuladong.github.io/algo/>

<https://mp.weixin.qq.com/s/a8yrhZ6Mg9n4tnTHO1N7IQ>

<https://github.com/xfhy/Android-Notes>

<https://github.com/labuladong/fucking-algorithm>

<https://juejin.cn/post/6876968255597051917>

<https://github.com/JsonChao/Awesome-Android-Exercise>

<https://juejin.cn/post/6844904106545414157>

<https://juejin.cn/post/6844903613865672718>

<http://liuwangshu.cn/tags/ClassLoader/>

<https://rengwuxian.com/tag/kotlin/>

<https://rengwuxian.com/tag/custom-view/>

<https://juejin.cn/post/6862548590092140558>

<https://juejin.cn/post/6914802148614242312>

<https://juejin.cn/post/6884505736836022280>

<https://blog.csdn.net/hzwailll/article/details/85339714>

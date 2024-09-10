---
tags:
  - obsidian插件
aliases:
---

## 定义

**Dataview** 是 [[Obsidian]] 插件之一，它允许用户在笔记中查询、显示和操作基于元数据（metadata）的数据。借助 Dataview，用户可以使用查询语言对笔记中的信息进行分类和显示，从而实现更为灵活和自动化的知识管理。

## 特点

1. **查询笔记内容**：通过 `Dataview` 查询语言，用户可以根据标签、文件名、日期等条件从笔记库中筛选数据。
2. **自动化展示**：Dataview可以将结果以表格、列表、日历等形式展示出来，自动化组织笔记。
3. **支持元数据操作**：它支持读取笔记中的元数据（如YAML头部），并基于元数据进行复杂的操作。
4. **高度定制化**：用户可以自定义查询、过滤条件，还可以结合 JavaScript 编写更加复杂的查询逻辑。

## 相似概念辨析

### Dataview vs Obsidian 本地搜索

- **Dataview**：Dataview 使用专门的查询语言，可以根据元数据和特定条件展示笔记，支持复杂的数据查询和处理。
- **Obsidian 本地搜索**：Obsidian 内置的搜索功能提供了全局的关键词查找，不具备自动化展示和数据处理功能。

### Dataview vs DataviewJS

- **Dataview**：使用的是较为简单的 Dataview 查询语言（DSL），方便不懂编程的用户使用。
- **DataviewJS**：允许用户结合 JavaScript 编写自定义脚本，能够实现复杂的查询和数据操作，适合有编程经验的用户。

### Dataview vs Templater

- **Dataview**：专注于数据查询和动态展示。
- **Templater**：用于生成模板内容，适合在新建笔记时使用固定格式和动态数据填充。

## 原理

Dataview 通过解析笔记中的元数据和内容，生成符合条件的动态视图。用户可以在笔记中插入查询语句，Dataview 插件会根据查询条件检索整个笔记库，将符合条件的笔记和数据提取出来。

它的核心功能是基于笔记头部的 **YAML 元数据**。例如，在笔记开头写下：

```yaml
---
tags: [工作, 项目]
完成时间: 2023-08-01
---
```

Dataview 可以查询并展示带有 `tags: [工作]` 的所有笔记，或者根据 `完成时间` 对笔记进行排序和筛选。

### 基本语法示例

```dataview
table 完成时间 as "完成日期"
from ""
where tags = "工作"
sort 完成时间 desc
```

这个查询会生成一个表格，显示带有 `工作` 标签的所有笔记，并按 `完成时间` 降序排列。

## 使用

### 1. 安装 Dataview 插件

首先在 Obsidian 的插件市场中找到 `Dataview` 插件并安装启用。

### 2. 使用 Dataview 语法

Dataview 支持 `table`、`list`、`task` 等不同的视图类型。

#### 生成表格

```dataview
table 文件名 as "笔记标题", tags as "标签"
from ""
where tags = "学习"
sort file.mtime desc
```

该语法会生成一个表格，列出所有带有 "学习" 标签的笔记，显示笔记标题和标签，并按最后修改时间排序。

#### 生成列表

```dataview
list from "项目"
where status = "未完成"
sort due desc
```

生成未完成项目的清单。

#### 查询任务

```dataview
task from ""
where completed = false
```

查询所有未完成的任务并生成任务列表。

### 3. 使用 DataviewJS

如果需要更复杂的查询逻辑，可以使用 JavaScript 来编写动态查询。

```dataviewjs
let pages = dv.pages("#学习")
  .where(p => p.progress > 50);
dv.table(["文件", "进度"], pages.map(p => [p.file.name, p.progress]));
```

## Q & A

### 1. **Dataview 的数据来源是什么？**
   Dataview 的数据主要来源于 Obsidian 笔记的内容和元数据，尤其是 YAML 头部的数据字段。

### 2. **Dataview 能动态更新查询结果吗？**
   是的，Dataview 会根据查询条件动态更新结果，任何笔记内容的修改都会影响查询结果的显示。

### 3. **Dataview 支持哪些数据类型？**
   支持多种数据类型，包括日期、字符串、数字、布尔值和列表等，可以根据需要定义不同的数据结构。

## 总结

Dataview 插件让 Obsidian 更加强大，能够自动化管理和展示笔记内容。它通过自定义查询语法让用户可以基于笔记的元数据生成表格、列表等视图，适合知识管理和任务跟踪。
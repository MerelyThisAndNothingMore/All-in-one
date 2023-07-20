---
tags: 
alias:
---
inflate()方法就是正式解析 xml 的，
-   解析 layout 文件xml 找到根节点的名称
-   通过映射 Class.forName() 创建根节点视图View，如果需要 attachToRoot，绑定到父视图， 就把根节点 View add 到父视图中
-   循环遍历子节点，找到子节点名称
-   通过映射 Class.forName（）方法创建子视图，然后加入到根节点中



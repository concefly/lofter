title: CSS绘制全宽度导航栏
date: 2015-09-26 
tags: CSS
---

分三层，ul>li>a，最基本的样式如下设置：

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
        ul {
            /*显示为全宽度表格样式*/
            display: table;
            width: 100%;
            /*清除左右边距*/
            margin-left: 0;
            margin-right: 0;
            padding-left: 0;
            padding-right: 0;
        }
        ul>li {
            /*与ul的display属性配合*/
            display: table-cell;
        }
        ul>li>a {
            /*设为block样式，以占据整个li空间*/
            display: block;
            /*链接文字样式*/
            text-align: center;
            text-decoration: none
        }
    </style>
</head>
<body>
    <header>
        <ul>
            <li><a href="#">Menu1</a></li>
            <li><a href="#">Menu2</a></li>
            <li><a href="#">Menu3</a></li>
            <li><a href="#">Menu4</a></li>
            <li><a href="#">Menu5</a></li>
        </ul>
    </header>
</body>
</html>
```

为方便观察，给每个a临时加上了border。显示效果如下：
![Alt text](/img/1443074503952.png)

-----------------------------------

这样的导航栏显然太丑了，我们进一步添加装饰

## 导航菜单项高亮
```
ul>li>a {
    /*....*/
    /*扩展样式*/
    color: #aaa;
    line-height: 40px;
}
ul>li>a.active {
    color: #000;
    border-bottom: 3px solid #000;
}
```

![Alt text](/img/1443075125920.png)
![Alt text](/img/1443075179387.png)

注意事项：
- 最好在最内层标签（a）中设置高度属性，如`line-height`、`height`等，以撑开外层容器
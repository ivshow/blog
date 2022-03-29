---
title: 3 行 CSS 代码实现日历界面
cover: "/img/to-learn-gbf6c2e2a9_640.jpg"
date: 2020-09-1 16:30:12
---

![编码](/img/20220324231453.png)
日历组件大家应该很熟悉了，但你想过如何实现日历的界面布局吗？最容易想到的方法是用 table 布局，毕竟它在视觉上跟表格类似。但是从现代 CSS 的角度来说，table 多用来展示列表数据，而不是用作布局方案。因为它需要很多额外的 DOM 元素，而且样式控制上也不够灵活。另一种方法是用 flex 布局，这也算是 CSS3 给我们带来的福利，让界面布局工作简化了许多。

本文打算介绍一种更简单的方法，只需要 3 行核心 CSS 代码即可实现日历界面！你可能猜到了，它就是 Grid 布局。

先看 HTML 部分：

```
<div class="calendar-wrapper">
  <h1>Decemeber</h1>
  <ul class="calendar">
    <li class="weekday">一</li>
    <li class="weekday">二</li>
    <li class="weekday">三</li>
    <li class="weekday">四</li>
    <li class="weekday">五</li>
    <li class="weekday">六</li>
    <li class="weekday">日</li>

    <li class="first-day">1</li>
    <li>2</li>
    <li>3</li>
    <!-- ... -->
    <li>29</li>
    <li>30</li>
    <li>31</li>
  </ul>
</div>
```

为简单起见，这里把一周七天和日期全都放进一个列表里了。CSS 代码更简单了：

```
.calendar {
  display: grid; // 1
  grid-template-columns: repeat(7, 1fr);  // 2
}
.first-day {
  grid-column-start: 2; // 3
}
```

稍微解释下，第一行就是将列表声明为 grid 容器。第二行的属性 grid-template-columns 用来设置每列的宽度。一周有 7 天，因此需要 7 列。也可以这样写：

```
grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr 1fr;
```

或者

```
grid-template-columns: 40px 40px 40px 40px 40px 40px 40px;
```

是不是显得重复啰嗦？所以有了 repeat()简写方法，用于定义多个等宽的列就方便多了。这里的 1fr 是新的 CSS 弹性长度单位，具体用法可以参考 [https://www.w3.org/TR/css3-grid-layout/#fr-unit]。

最后，由于大部分月份的 1 号并不是周一，所以要用 grid-column-start 属性设置 1 号这个单元格的位置。

Bingo！一个极简日历就完成了！
![编码](/img/20220324231508.png)

当然了，头部的背景色还是需要额外代码的，但这不是关键所在。想要了解更多关于强大的 CSS Grid 布局的知识，推荐参考 MDN 文档 [https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout]

---
title: 移动端web开发总结
date: 2018-08-06 14:36:24
tags: [移动端, web]
# mp3: http://huxiaodo.com/files/music/01.m4a
cover: http://f.dooomi.com/image/2d9cc3e13e331af310eaccb0ce12b2e4.jpg
---

目前为止，互联网行业里，手机越来越智能化，移动端占有的比例越来越高，尤其实在电商，新闻，广告，游戏领域。用户要求越来越高，网站功能越来越好，效果越来越炫酷，这就要求我们产品质量越来越高，web前端开发而言是一个挑战，是一个难题，也是一个机遇。

###  1、Meta标签
- 页面在手机上显示时，增加这个meta可以让页面强制让文档的宽度与设备的宽度保持1:1，并且文档最大的宽度比例是1.0，且不允许用户通过点击或者缩放等操作对屏幕放大浏览。（这个在ios10以上的版本已经失效了，即使加了下面的meta，用户双击，缩放还是可以缩放页面。大家可以按照开发需求，参考下面的连接进行限制–ios10中禁止用户缩放页面）
    ```
    <meta content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0;" name="viewport"/>
    ```

- 禁止ios上自动识别电话
    ```
    <meta content="telephone=no" name="format-detection"/>
    ```
- 禁止android上自动识别邮箱
    ```
    <meta content="email=no" name="format-detection"/>
    ```
- 下面两个是针对ios上的safari上地址栏和顶端样式条的
    ```
    <!-- 听说在ios7以上版本就没效果了 -->
    <meta name="apple-mobile-web-app-capable" content="yes"/>
    
    <!-- 可选default、black、black-translucent 但是我都是用black-->
    <meta name="apple-mobile-web-app-status-bar-style" content="black"/>
    ```

### 2、打电话发短信
```
<a href="tel:10086">打电话给:10086</a>
<a href="10086">发短信给: 10086</a>
```

### 3、css3过渡动画开启硬件加速
```
.translate3d {
   -webkit-transform: translate3d(0, 0, 0);
   -moz-transform: translate3d(0, 0, 0);
   -ms-transform: translate3d(0, 0, 0);
   transform: translate3d(0, 0, 0);
}
```
建议：  
- 在手机上（其实PC也是一样）。CSS3动画或者过渡尽量使用transform和opacity来实现动画，不要使用left和top。  
- 动画和过渡能用css3解决的，就不要使用js。如果是复杂的动画可以使用css3+js（或者html5+css3+js）配合开发。

### 4、移动端click屏幕产生200-300 ms的延迟响应
click事件因为要等待确认是否是双击事件，会有300ms的延迟（两次点击事件间隔小于300ms就认为是双击），体验并不好。现在的解决方案，第一个就是采用touchstart或者touchend代替click。或者封装tap事件来代替click 事件，所谓的tap事件由touchstart事件+ touchmove（判断是否是滑动事件）+touchend事件封装组成。

### 5、图片优化
- base64编码图片替换url图片  
对于一些小图标（我这做法是把8K以下的图标都转换成base64）之类的，可以将图片用base64，来减少请求的发送。尤其是在移动端，请求显得特别珍贵，在网速的不好的情况下，请求就是珍贵中的珍贵。

- 图片压缩 
对于整个网站来说，图片是最占流量的资源之一，能不使用就不适用，图标可是使用base64编码，字体图标代替，SVG等来代替，使用就要选择最合适的格式，合适的尺寸，然后压缩–这里推荐腾讯推出的智图。

    PS：过度压缩图片大小影响图片显示效果，可能会使得图片变得模糊，一般来说，品质在60左右就差不多了！

- 图片懒加载  
首屏加载的快慢，直接影响用户的体验，建议将非首屏的图片资源放到用户需要时才加载。这样可以大大优化首屏加载，减少首屏加载所需要的时间！

    ps：懒加载要使用js频繁操作dom，期间会导致大量重绘渲染，影响性能。

- img还是background
图片的展示方式有两种，一种是以图片标签显示，一种是以背景图片显示！下面写了这两者的区别。

    img:html中的标签img是网页结构的一部分会在加载结构的过程中和其他标签一起加载。  
    background：以css背景图存在的图片background会等到结构加载完成（网页的内容全部显示以后）才开始加载，也就是说，网页会先加载标签img的内容，再加载背景图片background引用的图片。引入一张图片，那么在这个图片加载完成之前，img后的内容不会显示。而用background来引入同样的图片，网页结构和内容加载完成之后，才开始加载背景图片，网页内容能正常浏览，但是看不到背景图片。至于这两种，大家按照习惯，需求等权重因素选择！

### 6、快速回弹滚动
在ios上，如果存在局部滚动，就要加这个属性了！如果不加，滚动会很慢，看起来也会有一卡一卡的感觉。
```
-webkit-overflow-scrolling: touch;
```

### 7、ios系统中去掉元素被触摸时产生的半透明灰色遮罩
```
a, button, input, textarea { 
    -webkit-tap-highlight-color: rgba(0,0,0,0);
}
```

### 8、ios中去掉默认input默认样式
```
input, button, textarea {
    -webkit-appearance: none;
}
```
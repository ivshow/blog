---
title: CSS 样式书写规范
date: 2018-06-23 13:52:22
tags: [CSS]
# mp3: http://huxiaodo.com/files/music/01.m4a
cover: http://f.dooomi.com/image/974c662d0cec5264f1649fa1b22f45e5.jpg
---

可能不同团队都有各自的规范，又或者很多人在写 CSS 的时候还是想到什么就写什么，不存在太多的约束。  
我觉得 CSS 代码规范还是有存在的必要的，尤其是在团队配合，多人协作下，规范就显得尤为重要。

###  1、编码设置
采用 UTF-8 编码，在 CSS 代码头部使用：
```
@charset "utf-8";
```

注意，必须要定义在 CSS 文件所有字符的前面（包括编码注释），@charset 才会生效。

###  2、命名空间规范
- 布局：以 g 为命名空间，例如：.g-wrap 、.g-header、.g-content。
- 状态：以 s 为命名空间，表示动态的、具有交互性质的状态，例如：.s-current、s-selected。
- 工具：以 u 为命名空间，表示不耦合业务逻辑的、可复用的的工具，例如：u-clearfix、u-ellipsis。
- 组件：以 m 为命名空间，表示可复用、移植的组件模块，例如：m-slider、m-dropMenu。
- 钩子：以 j 为命名空间，表示特定给 JavaScript 调用的类名，例如：j-request、j-open。

###  3、命名空间思想  
没有选择 BEM 这种命名过于严苛及样式名过长过丑的规则，采取了一种比较折中的方案。
- 不建议使用下划线 _ 进行连接
节省操作，输入的时候少按一个 shift 键，能良好区分 JavaScript 变量命名。
- 字符小写
定义的选择器名，属性及属性值的书写皆为小写。

###  4、选择器
当一个规则包含多个选择器时，每个选择器独占一行。  

、+、~、> 选择器的两边各保留一个空格。  
```
.g-header > .g-header-des,  
.g-content ~ .g-footer {

}
```
命名短且语义化良好  
对于选择器的命名，尽量简洁且具有语义化，不应该出现 g-abc 这种语义模糊的命名。

### 5、规则声明块
- 当规则声明块中有多个样式声明时，每条样式独占一行。
- 在规则声明块的左大括号 { 前加一个空格。
- 在样式属性的冒号 : 后面加上一个空格，前面不加空格。
- 在每条样式后面都以分号 ; 结尾。
- 规则声明块的右大括号 } 独占一行。
- 每个规则声明间用空行分隔。
- 所有最外层引号使用单引号 ‘ 。
- 当一个属性有多个属性值时，以逗号 , 分隔属性值，每个逗号后添加一个空格，当单个属性值过长时，每个属性值独占一行。

完整示例如下：
```
.g-footer,
.g-header {
    position: relative;
}

.g-content {
    background:
        linear-gradient(135deg, deeppink 25%, transparent 25%) -50px 0,
        linear-gradient(225deg, deeppink 25%, transparent 25%) -50px 0,
        linear-gradient(315deg, deeppink 25%, transparent 25%),
        linear-gradient(45deg, deeppink 25%, transparent 25%);
}

.g-content::before {
    content: '';
}
```

### 6、数值与单位
- 当属性值或颜色参数为 0 – 1 之间的数时，省略小数点前的 0 。color: rgba(255, 255, 255, 0.5)color: rgba(255, 255, 255, .5);
- 当长度值为 0 时省略单位。margin: 0px automargin: 0 auto
- 十六进制的颜色属性值使用小写和尽量简写。color: #ffcc00color: #fc0

### 7、样式属性顺序
单个样式规则下的属性在书写时，应按功能进行分组，并以 Positioning Model > Box Model > Typographic > Visual 的顺序书写，提高代码的可读性。

- 如果包含 content 属性，应放在最前面；
- Positioning Model 布局方式、位置，相关属性包括：position / top / right / bottom / left / z-index / display / float / …
- Box Model 盒模型，相关属性包括：width / height / padding / margin / border / overflow / …
- Typographic 文本排版，相关属性包括：font / line-height / text-align / word-wrap / …
- Visual 视觉外观，相关属性包括：color / background / list-style / transform / animation / transition / …

Positioning 处在第一位，因为他可以使一个元素脱离正常文本流，并且覆盖盒模型相关的样式。盒模型紧跟其后，因为他决定了一个组件的大小和位置。其他属性只在组件内部起作用或者不会对前面两种情况的结果产生影响，所以他们排在后面。

### 8、合理使用使用引号
在某些样式中，会出现一些含有空格的关键字或者中文关键字。

- font-family 内使用引号

    当字体名字中间有空格，中文名字体及 Unicode 字符编码表示的中文字体，为了保证兼容性，都建议在字体两端添加单引号或者双引号：
    ```
    body {
        font-family: 'Microsoft YaHei', '黑体-简', '\5b8b\4f53';
    }
    ```

- background-image 的 url 内使用引号

    如果路径里面有空格，旧版 IE 是无法识别的，会导致路径失效，建议不管是否存在空格，都添加上单引号或者双引号：
    ```
    div {
        background-image: url('...');
    }
    ```

### 9、避免使用 !important
除去某些极特殊的情况，尽量不要不要使用 !important。

!important 的存在会给后期维护以及多人协作带来噩梦般的影响。

当存在样式覆盖层叠时，如果你发现新定义的一个样式无法覆盖一个旧的样式，只有加上 !important 才能生效时，是因为你新定义的选择器的优先级不够旧样式选择器的优先级高。所以，合理的书写新样式选择器，是完全可以规避一些看似需要使用 !important 的情况的。
 
### 10、代码注释
- 单行注释

    星号与内容之间必须保留一个空格。
    ```
    /* 表格隔行变色 */
    ```

- 多行注释

    星号要一列对齐，星号与内容之间必须保留一个空格。
    ```
    /**
    * Sometimes you need to include optional context for the entire component. Do that up here if it's important enough.
    */
    ```

- 规则声明块内注释

    使用 // 注释，// 后面加上一个空格，注释独立一行。
    ```
    .g-footer {
        border: 0;
        // ....
    }
    ```

- 文件注释

    文件顶部必须包含文件注释，用 @name 标识文件说明。星号要一列对齐，星号与内容之间必须保留一个空格，标识符冒号与内容之间必须保留一个空格。
    ```
    /**
     * @name: 文件名或模块名
     * @description: 文件或模块描述
     * @author: author-name
     *          author-name2
     * @update: 2018-06-23 14:00
     */
    ```

@description为文件或模块描述。
@update为可选项，建议每次改动都更新一下。

当该业务项目主要由固定的一个或多个人负责时，需要添加@author标识，一方面是尊重劳动成果，另一方面方便在需要时快速定位责任人。
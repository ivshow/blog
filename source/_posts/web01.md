---
title: 前端如何高效的与后端协作开发
date: 2018-06-01 10:12:32
tags: [前后分离, 前端工程化, 单页面应用]
# mp3: http://huxiaodo.com/files/music/01.m4a
cover: http://f.dooomi.com/image/1c379ba17f76ab1038deb5367c8225d9.jpg
---

###  1、前后端分离
前端与后端的分离，能使前端的开发脱离后端的开发模式，拥有更大的自由度，以此便可做前端工程化、组件化、单页面应用等。  

可以参考：[前后端分离、web与static服务器分离](https://segmentfault.com/a/1190000015297319)

### 2、尽量避免后端模板渲染
web 应用的渲染方式分为服务器端渲染和客户端渲染，当下比较推荐的方式是客户端渲染，数据使用全 ajax 的方式进行交互。

除非在一些不得不使用服务器端渲染的情况下（如门户、电商等），应当尽量使用客户端渲染，因为客户端渲染更能使前后端分离（项目分离、代码解耦、协作分离、职责分离等），也能更好的做本地接口模拟开发，提升开发效率。

即使用服务器端渲染，在技术支持的条件下，可以使用 node 中间层（由前端人员开发），代替传统的后端模板渲染，这样可以使后端与前端完全解耦，后端与前端只有数据上的往来。

可以参考：[细说后端模板渲染、客户端渲染、node 中间层、服务器端渲染](https://segmentfault.com/a/1190000016704384)

### 3、 尽量避免大量的线上调试
做好本地接口模拟开发（包括后端模板渲染），避免大量的线上调试，因为线上调试很不方便，也很费事，并且每次更新代码，都需要重新构建，然后同步到服务器。

所以做好本地接口模拟开发，只要程序在本地运行是没问题的，一般线上就不会有太大的问题，这样就能大幅降低调试工作量，提升开发效率。

### 4、本地接口模拟开发
本地接口模拟就是在本地模拟一个与服务器差不多的环境，能够提供数据所需的接口，进行错误模拟处理等等。

本地接口模拟开发的意义就在于能够在本地完成几乎所有的开发与调试，尽量减少线上的调试，提高开发效率。

一些常用库：
- [browser-sync](https://github.com/BrowserSync/browser-sync)：能让浏览器实时、快速响应文件更改（ html、 js、css、 sass、 less 等）并自动刷新页面，并且可以同时在PC、平板、手机等设备下进行调试。
- [webpack-dev-middleware](https://github.com/webpack/webpack-dev-middlewar)：A development middleware for webpack。
- [webpack-hot-middleware](https://github.com/webpack-contrib/webpack-hot-middleware)：热更新本地开发浏览器服务。
另外，本地接口模拟开发需要后端开发人员有规范的接口文档。

可以参考：[本地化接口模拟、前后端并行开发](https://segmentfault.com/a/1190000015297352)

### 5、规范的接口文档
前端与后端协作提升开发效率的一个很重要的方法就是减少沟通：能够形成纸质的文档就不要口头沟通、能够把接口文档写清楚也不要口头沟通（参数、数据结构、字段含义等），特别是线上协作的时候，面对面交流是很困难的。

一个良好的接口文档应当有以下的几点要求与信息：
- 格式简洁清晰：推荐用 [API Blueprint](https://apiblueprint.org/)
- 分组：当接口很多的时候，分组就很必要了
- 接口名、接口描述、接口地址
- http 方法、参数、headers、是否序列化
- http 状态码、响应数据

接口文档可以用一些文档服务（如 [leanote](https://github.com/leanote/leanote)）来管理文档，也可以用 git 来管理；  
书写方式可以用 markdown，也可以 YAML、 JSON 等。

推荐使用 markdown 方式写文档，用 git 管理文档。

可以参考：[本地化接口模拟、前后端并行开发](https://segmentfault.com/a/1190000015297352)

### 6、运行时捕捉 js 脚本错误
当用户在用线上的程序时，怎么知道有没有出 bug；如果出 bug 了，报的是什么错；如果是 js 报错，怎么知道是那一行运行出了错？

所以，在程序运行时捕捉 js 脚本错误，并上报到服务器，是非常有必要的。

这里就要用到 window.onerror 了：
```
window.onerror = (errorMessage, scriptURI, lineNumber, columnNumber, errorObj) => {
    const data = {
        title: document.getElementsByTagName(
            'title'
        )[
            0
        ].innerText,
        errorMessage,
        scriptURI,
        lineNumber,
        columnNumber,
        detailMessage: (errorObj && errorObj.message) ||
            '',
        stack: (errorObj && errorObj.stack) ||
            '',
        userAgent: window.navigator.userAgent,
        locationHref: window.location.href,
        cookie: window.document.cookie,
    };
    post('url', data); // 上报到服务器
};
```
线上的 js 脚本都是压缩过的，需要用 sourcemap 文件与 [source-map](https://github.com/mozilla/source-map) 查看原始的报错堆栈信息。

可以参考：[webpack - devtool](https://webpack.js.org/configuration/devtool/)

### 7、移动端调试
因为移动端的开发无法像 pc 端开发一样使用 Chrome 的开发者调试工具，所以调试移动端需要一些额外的技巧。

移动端应用一般都运行在微信浏览器中、 webview 中、手机浏览器中。

- 远程调试（Remote Debugging）

> 远程调试就是通过 USB 连接、端口转发、搭建代理等方式，将一个设备的 web 页面映射到另一个设备上，比如将手机的 webview 映射到 pc 上，达到调试的目的。  
移动端 web 应用调试难题从一开始就有，不过后来浏览器厂商基本都推出自己的远程调试工具来解决这个问题，包括 OperaMobile、 iOSSafari、 ChromeforAndroid、UC 浏览器等，另外还有一些第三方开发的远程调试工具，比如 [weinre](http://people.apache.org/~pmuellr/weinre/docs/1.x/1.5.0/) 等。

> 以 Android 为例，可以将 webview、 ChromeforAndroid 中的页面映射到 pc 端的 ChromeDevTools，然后就可以在 pc 端调试移动端的页面了。

> 可以参考：[移动端Web开发调试之Chrome远程调试(Remote Debugging)](https://blog.csdn.net/freshlover/article/details/42528643/)

- vConsole

> 一个轻量、可拓展、针对手机网页的前端开发者调试面板（ chrome 开发者工具的便利实现）。  
这个是内嵌的页面当中的便捷调试器，基本上能够满足一般的需要远程调试的页面。 

> github：https://github.com/Tencent/vConsole  
demo：https://wechatfe.github.io/vconsole/demo.html

![Markdown](http://f.dooomi.com/image/20181204124208.jpg)

- TBS Studio

> 因为微信浏览器是定制的浏览器，一般的远程调试方式都不可用，需要配合特定的工具，如微信开发者工具。  
[TBS Studio](https://x5.tencent.com/tbs/guide.html) 是另一个可以像 Chrome 一样调试远程微信浏览器页面的强大工具。

> 可以参考：[tbs studio - 腾讯浏览服务-调试工具](https://x5.tencent.com/tbs/guide/debug/season1.html)

### 8、前端后并行开发
正常情况下，前端的开发在完成 UI 或者组件开发之后，就需要等后端给出接口文档才能继续进行，如果能做到前后端并行开发，也能提升开发效率。

前后端并行开发，就是说前端的开发不需要等后端给出接口文档就可以进行开发，等后端给出接口之后，再对接好后就基本上可以上线了。

在本地化接口模拟的实现下，就可以做到前后端并行开发，只是在代码层面需要对 ajax 进行封装。

可以参考：[本地化接口模拟、前后端并行开发](https://segmentfault.com/a/1190000015297352)
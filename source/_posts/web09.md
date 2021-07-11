---
title: 银川国际航空港综合交通枢纽
date: 2018-09-16 11:23:52
# mp3: http://huxiaodo.com/files/music/01.m4a
cover: http://f.dooomi.com/image/a185df6b7753d89bf04c04bbaba58d78.jpg
---

该系统主要用于根据各类型交通工具的变化，为旅客提供实时的动态信息，机场设施、商家、酒店、景点的信息查询，并提供精准的室内导航，为旅客呈现出导航路线、行走距离和时间。

### 技术实现
> 1、采用前后端分离的开发模式，通过Nginx反向代理，调用API接口，获取相关数据。  
2、采用flex + rem 实现弹性布局，适配移动端设备大小。  
3、使用vue-cli 脚手架搭建项目，并进行组件化开发。  
4、使用axios 实现后台数据请求，对http请求进行二次封装，实现请求和响应拦截器，并对接口地址统一管理。  
5、使用vue-Router实现路由跳转单页面切换、路由拦截等。  
6、使用vuex 实现中心化状态管理，组件之间数据共享、数据交互，数据管理。  
7、通过vue实例作为中央事件总线，进行非父子组件通信。  
8、使用vux、mint-ui、swiper实现底部导航栏切换、轮播图、提示框等组件开发。  
9、使用alloy_finger.js 手势库，进行地图单指拖放，双指缩放、旋转、双击放大等交互功能。  
10、通过调用微信JS-SDK，使用系统的拍照、选图功能，并上传图片至微信服务器，再由后端下载至文件服务器。  
11、使用webpack进行项目打包，实现代码优化功能。  
12、整体采用Vue+Vue-cli+Vue-Router+VueX+Axios+Webpack+ES6+Less+Vux+Mint-UI+Nginx技术方案实现。

### 项目展示
> 1、http://huxiaodo.com/TerminalWX  （请在微信中访问）  
2、http://huxiaodo.com/TerminalScreen  （请在触摸屏上访问）

### 展示图
![Markdown](http://f.dooomi.com/image/terminalScreen.gif)

---
title: Hexo + GitHub 搭建个人博客
date: 2018-05-25 13:12:14
tags: [Hexo, GitHub, GithubPages]
# mp3: http://huxiaodo.com/files/music/01.m4a
cover: http://f.huxiaodo.com/image/blog/6e5278055c8659e91488b827382f36c2.jpg
---

搭建个人博客的念头已经很久了，一直忙于其他事情，终于有时间了！
这篇文章就是记录一下搭建的过程，以及遇到的坑，将搭建步骤、问题汇总。
##  一、搭建方案
博客搭建的方案：
> 1. 自己购买服务器，如阿里云、腾讯云等，利用现用的主流博客开源框架，如WordPress，ZBlogPHP等，主流博客开源框架大都是php，然后就是基于go，python，nodejs，ruby语言相对都比较少，然后java方案就更少的可怜了；
> 2. 利用github的免费500M的gitpage空间搭建静态博客，加基于node.js语言的hexo方案搭建个人静态博客，并直接上传gitHub。

该篇文章选择的是第二种。

## 二、搭建本地静态博客
### 1. 下载nodejs  
> 访问：https://nodejs.org/en/download/ 网址，根据不同平台系统选择你需要的Node.js安装包，Windows系统下载安装包(.msi)文件格式就行  
32 位安装包下载地址 : https://nodejs.org/dist/v4.4.3/node-v4.4.3-x86.msi  
64 位安装包下载地址 : https://nodejs.org/dist/v4.4.3/node-v4.4.3-x64.msi  
然后就是一路next，install，finish常规操作就行，如果实在连安装软件的基础都没有可以看这条图文并茂的链接  
node.js安装教程：https://www.runoob.com/nodejs/nodejs-install-setup.html

### 2. 安装git  
> 访问下载官网：https://gitforwindows.org/ 点击download，下载完成以后选择安装，也是一路next，install，finish常规操作即可，不会安装软件的也可以访问下面的详细教程  
git安装教程：https://jingyan.baidu.com/article/9f7e7ec0b17cac6f2815548d.html

### 3. hexo的安装和启动  
> 做到这里，前期准备工作已经完成，现在开始正式搭建个人博客，在合适地方选择新建一个hexo文件夹，然后右键选择Git Bash Here。  
![Markdown](http://f.huxiaodo.com/image/hexo/0b7a91a059ae5a03s.png)  

然后在命令行输入： npm install hexo-cli -g 命令，如图所示，add 104 package in 77.155s，并且文件夹中也多了node_modules文件夹和package-lock.json文件就说明下载安装完成。  
![Markdown](http://f.huxiaodo.com/image/hexo/c1fb52ed7765e7e9.png)  
当然你也可以通过运行hexo -v检查一下是否安装成功，如果秩序命令返回版本信息也是安装成功了。  
现在我们可以开始实例化一个我们本地的博客样本了，这次我们要新建一个空白文件夹，比如在E盘，新建空白hexo文件夹，并同样的用右键git bash here ，在命令行输入hexo init，等待hexo实例化完成返回如图所示提示。  
![Markdown](http://f.huxiaodo.com/image/hexo/20181204092156.png)

如果成功返回start blogging with hexo说明，你已经完成了本次博客搭建，剩下来就是如何使用的问题了，你不用担心，使用相比安装其实要简单，所以到这里一百步已经走了九十步了。  
现在我们在命令行输入hexo s命令  
![Markdown](http://f.huxiaodo.com/image/hexo/204bbb6b7efbc366.png)

然后打开浏览器输入localhost:4000即可看到我们实例化的静态博客了，是不是很简单。

## 三、部署发布静态博客
### 1. 注册github账号  
> 如果你已经注册过github可以跳过这一步。  

> 访问github网站：http://github.com 这里简单介绍一下github网站，git是linux内核开发者，就是我们俗称的linux之父开发的用于管理分布式版本的控制系统，github就是支持且只支持git进行版本库托管，然后面向开源或私有项目的托管平台，任何人都可以注册账号发布你的开源代码在github免费托管，同时你也可以查到各种优秀的开源项目在github。  

> 打开网页是这样的，然后username填写自己的用户名，不可以和别人重名，email填写自己有效的邮件地址，然后是password密码，如果内容都是正确的后面会有绿色的小勾，然后点击sign up for github即可。  
![Markdown](http://f.huxiaodo.com/image/hexo/20181204091536.png)  

> 开始新建自己的仓库就是new repository，点击你的头像旁边的+号，选择new respository。  
![Markdown](http://f.huxiaodo.com/image/hexo/2b96df44d598cebf.png)  

> 然后给仓库起名字， 仓库的名字需要和你的账号对应，格式: yourname.github.io，然后create repository就可以创建成功了，接下来我们只要把本地的hexo博客发布到我们仓库，然后很快你就能在网页上访问到自己的博客了。  
<font color="red">注意</font>   
命名规则：你的github账号.github.io   
![Markdown](http://f.huxiaodo.com/image/hexo/a8ff966ba70438eb.png)    

> 成功create repository以后，会进到这个界面，我们选择http点击复制。  
![Markdown](http://f.huxiaodo.com/image/hexo/70186aecbd7a5f79.png)

### 2. hexo使用
> 目录结构 
```
    ├── .deploy #需要部署的文件  
    ├── node_modules #Hexo插件
    ├── public #生成的静态网页文件  
    ├── scaffolds #模板  
    ├── source #博客正文和其他源文件，404、favicon、CNAME 都应该放在这里  
        ├── _drafts #草稿  
        └── _posts #文章  
    ├── themes #主题  
    ├── _config.yml #全局配置文件  
    └── package.json  
```
> 全局配置 _config.yml
```
    title:  #标题
    subtitle:  #副标题
    description:  #站点描述，给搜索引擎看的
    author:  #作者
    email:  #电子邮箱
    language: zh-CN #语言
    url:  #网址
    root: / #根目录
    permalink: :year/:month/:day/:title/ #文章的链接格式
    tag_dir: tags #标签目录
    archive_dir: archives #存档目录
    category_dir: categories #分类目录
    code_dir: downloads/code
    permalink_defaults:
    source_dir: source #源文件目录
    public_dir: public #生成的网页文件目录
    new_post_name: :title.md #新文章标题
    default_layout: post #默认的模板，包括 post、page、photo、draft
    titlecase: false #标题转换成大写
    external_link: true #在新选项卡中打开连接
    filename_case: 0
    render_drafts: false
    post_asset_folder: false
    relative_link: false
    highlight: #语法高亮
    enable: true #是否启用
    line_number: true #显示行号
    tab_replace:
    default_category: uncategorized #默认分类
    category_map:
    tag_map:
        2: 开启分页
        1: 禁用分页
        0: 全部禁用
    archive: 2
    category: 2
    tag: 2
    port: 4000 #端口号
    server_ip: localhost #IP 地址
    logger: false
    logger_format: dev
    date_format: YYYY-MM-DD #参考http://momentjs.com/docs/#/displaying/format/
    time_format: H:mm:ss
    per_page: 10 #每页文章数，设置成 0 禁用分页
    pagination_dir: page
    disqus_shortname:
    theme: landscape-plus #主题
    exclude_generator:
    plugins: #插件，例如生成 RSS 和站点地图的
        - hexo-generator-feed
        - hexo-generator-sitemap
    deploy:
        type: git
        repo: 刚刚github创库地址.git
        branch: master
```
<font color="red">注意</font>   
- 配置文件的冒号“:”后面有一个空格
- repo: 刚刚github创库地址.git

> hexo命令行使用  

> 常用命令：
```
    hexo help #查看帮助
    hexo init #初始化一个目录
    hexo new "postName" #新建文章
    hexo new page "pageName" #新建页面
    hexo generate #生成网页，可以在 public 目录查看整个网站的文件
    hexo server #本地预览，'Ctrl+C'关闭
    hexo deploy #部署.deploy目录
    hexo clean #清除缓存，**建议每次执行命令前先清理缓存**
```
> 简写：
```
    hexo n == hexo new
    hexo g == hexo generate
    hexo s == hexo server
    hexo d == hexo deploy
```

### 3. 部署发布服务
> 第一条命令 npm install hexo-deployer-git –save  
  第二条命令 hexo clean && hexo g && hexo d  

等出现INFO deploy done：git提示的时候说明部署成功。

### 4. 发布成功
> 至此，博客已经部署成功，访问 “你的github账号.github.io”，就可以看到个人博客！
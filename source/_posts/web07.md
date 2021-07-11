---
title: 智慧航显（人脸识别）
date: 2018-11-19 16:57:02
# mp3: http://huxiaodo.com/files/music/01.m4a
cover: http://f.dooomi.com/image/7a60b742253cf72a8883d0d444e6d289.jpg
---

该系统通过人脸识别技术自动识别旅客身份，为旅客提供航班动态、精准的室内导航路线、路径距离和行走时间，目的地天气等实用信息，并根据旅客以往乘机记录，自动筛选数据，给出温馨提示。

### 项目上线机场

> 1、阿布扎比国际机场（国外）  
> 2、深圳宝安国际机场  
> 3、西安咸阳国际机场  
> 4、银川河东国际机场  
> 5、西宁曹家堡国际机场  
> 6、北京举行的首届民航科教创新成果展  
> 7、上海举行的“grow with the cloud”全联接大会  
> 8、西安咸阳国际机场举行的民航航班正常暨服务质量工作会  
> 9、苏州 OpenLab 行业数字化转型创新中心  
> 10、成都举行的“智慧民航”第十六届民航信息化发展论坛

### 技术实现

> 1、采用前后端分离的开发模式，通过 Nginx 反向代理，调用 API 接口，获取旅客的航班数据。  
> 2、通过制作的可视化界面操作，将设计的伪 3D 地图通过算法建立成二维坐标模型。  
> 3、采用人脸识别技术，判断旅客身份信息，并获取旅客当前位置。  
> 4、采用 A\*(A-Star)算法，在二维坐标模型中获取最佳路径。  
> 5、采用贝塞尔曲线，将获取到的路径坐标转化为一条光滑的曲线。  
> 6、采用三角函数及反三角函数计算出路径的偏向角度。  
> 7、采用递归算法将坐标点位，通过 canvas 绘画至前端页面，形成动态导航路线，并精准的计算路径距离和行走时间。  
> 8、采用 flex + rem 实现不同分辨率下自适应的弹性盒子布局。  
> 9、使用 animate.css 实现页面元素动画效果。  
> 10、使用 css3 动画属性实现“钟摆”滚动。  
> 11、整体采用 Vue + jQuery + Ajax + Canvas + CSS3 + Nginx 技术方案实现。

### 项目展示

> 1、2018 年 10 月 10 日-12 日上海举行的“grow with the cloud”全联接大会：

- http://app.cctv.com/special/cbox/detail/index.html?guid=e628e4adbba049618d2ac9cd6377d6b4&vsid=C11348&from=timeline&isappinstalled=0
- https://mp.weixin.qq.com/s/uT_2IAHm4wr5ySxI_sXFoQ

> 2、2018 年 6 月 13 日北京展览馆举行的首届民航科教创新成果展：

- https://mp.weixin.qq.com/s/l3pbsifwv-pzwMEdNf07Nw

> 3、2018 年 11 月 22 日成都举行主题为“智慧民航”的第十六届民航信息化发展论坛：

- https://mp.weixin.qq.com/s/nubiHl9pckxkvqePAkSkjg

> 4、助力银川河东国际机场被认定为国际航空运输协会（IATA）“白金机场”：

- https://mp.weixin.qq.com/s/ZRW7Zy9sin4N6Vz7Rab4AA

### 展示图

![Markdown](http://f.dooomi.com/image/zhhx.gif)

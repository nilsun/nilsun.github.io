---
layout: post
title: Apple Watch开发初探
description: 今天苹果发布了iOS8.2 SDK Beta版和Xcode6.2 Beta版，其中最大的亮点就是就是WatchKit，虽然Apple Watch还未发布，但是开发者已经可以用Xcode6.2开发Watch App了。本文基于苹果的开发文档，简单介绍一下相关的开发概念。
category: blog
published: ture
---

今天苹果发布了iOS8.2 SDK Beta版和Xcode6.2 Beta版，其中最大的亮点就是就是WatchKit，虽然Apple Watch还未发布，但是开发者已经可以用Xcode6.2开发Watch App了。本文基于苹果的开发文档，简单介绍一下相关的开发概念。

##目前苹果对第三方Watch App的定位

根据苹果文档的描述，第三方的Watch App依赖于与之对应的iPhone/iPad App，它更像是iPhone App的一个子集，为iPhone App提供增强功能比如通知，提醒，信息显示。Watch App只负责前端展示，所有的业务逻辑，都由iPhone App完成，它只是与iPhone App通讯来获取相关显示数据。这样看来App Watch是不是应该说妈妈再也不用担心我的内存和电池，因为代码全跑在iPhone上，可是你让没有iPhone的人情何以堪。

##用户和Watch App的交互几种基本方式

###1、Watch App
用户点击App直接进入主界面，Watch的导航类型有Hierarchical和Page-based两种，开发者只能选取其一。Hierarchical即层级导航的模式，例如下面的Todo list，用户点击摘要文字了，跳转页面看到更多信息；Page-based和Weather App类似，用户左右滑动来切换页面。

![list app](/images/tech/applewatch/list_app.png)

###2、Glances
Glances就是一个视图界面，只不过这个界面只读，不能和用户交互，用作信息展示，展示的内容为对应的iPhone App中即时，最重要的信息，比如出行应用中航班晚点信息，计步应用中的步行步数，速度，能量消耗。

![glance](/images/tech/applewatch/glance.png)

###3、Watch通知
开发者可以对通知栏进行自定义，比如在邮件Watch App的通知栏上加上 “标记已读/星标” 的按钮。

##Watch App 架构

###1、Watch App和iPhone App的基于WatchKit的通信
App for Watch包含两部分：Watch app 和 Watach Kit extention。 Watch app运行在Watch上，只包含Storyboard和资源文件；WatchKit extension运行在iPhone上，和对应的iPhone App在一起 。用户点击Watch App后，与Watch匹配的iPhone会启动WatchKit extension，然后和Watch建立连接，然后两者可以产生通信（如获取数据等）。

![watchkit communication](/images/tech/applewatch/watchkit_communication.png)

###2、Watch App的运行流程
在ViewController initWithContext方法里面去请求iPhone获取数据；willActivate代表界面已经出现了，必须要显示了，在这里可以做一些视图显示后的初始逻辑（类比iOS viewDidAppear）。

![watch app loop](/images/tech/applewatch/watch_app_loop.png)

###3、ViewController的生命周期

![viewcontroller loop](/images/tech/applewatch/viewcontroller_loop.png)

##iPhone/iPad App加入对Watch App的支持

###1、配置Xcode工程
Xcode6.2 File->New->Target: 选择Watch App，然后选择是否创建Glance或者通知

![watch app target](/images/tech/applewatch/watch_app_target.png)

在Targets中会发现多出了WatchKit Extension和Watch App；运行通过，接下来大家自己摸索了。

###2、App Target结构
![target structure](/images/tech/applewatch/target_structure.png)

##一些细节

Watch作为一款新的苹果设备，所运行App的无论是设计还是编码都应该尽量考虑到它的独特性。

比如说苹果将Watch上的系统字体改变成了”San Francisco”来提高小屏幕上的文字辨识度。同时苹果建议开发者不要在Watch app中请求位置权限，那样授权Tips弹在你的iPhone上，而你正使用的是Watch；不要在请求后台任务，因为目前Watch app并没有后台模式；不要在Watch App里运行繁重的任务......

本文还有很多内容没有覆盖到，希望起到抛砖引玉的作用。
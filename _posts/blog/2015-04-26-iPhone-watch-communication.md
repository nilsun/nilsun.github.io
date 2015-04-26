---
layout: post
title: Apple Watch 和 iPhone 通信实践
description: 本文主要从实践的角度分析 iPhone 和 Watch 的通信框架， 关于 Watch 开发的基本概念可以参考之前写的一篇文章《Apple Watch开发初探》。
category: blog
published: ture
---

本文主要从实践的角度分析 iPhone 和  Watch 的通信框架， 关于 Watch 开发的基本概念可以参考之前写的一篇文章[《Apple Watch开发初探》][1]。

##WatchKit Extension，iPhone App，Watch App关系

给 iPhone App 添加 Watch App 支持需要在 iPhone 工程中添加 Watch Extension，添加后工程中对应生成两个新的target：WatchKit Extension 和 WatchKit App，分别对应逻辑和界面。开发完成后，WatchKit Extension 和 iPhone App 被一起安装到 iPhone 中，如果 iPhone 有匹配的 Watch，系统会提示是否需要将 WatchKit App 安装到 Watch 上。

##WatchKit Extension

WatchKit Extension 是 iPhone App 和 Watch App 通信的桥梁，下图展示了三者间的通信过程：

![list app](/images/tech/iPhoneWatchCommunication/iphone_watch_communication.png)

###1、Extension 和 Watch App ( Host App )的通信 

在 Extension 中可以获取到 Watch App 界面元素的数据接口 WKInterfaceObject（非控件 View 本身，更接近于 Model），通过修改 WKInterfaceObject 的数据，来修改对应Watch App的界面。
   
另外通过Xcode将Watch App控件的响应事件绑定到Extension中，可以实现Watch操作响应逻辑的处理。
    
Extension和Watch App的通信在开发实现层面上看起来相对比较简单，开发者只需在接口上实现相关逻辑就可以了，而具体的通信过程，苹果进行了封装，对开发者是不可见的。

###2、Extension 和 iPhone App ( Containing App )的通信

Extension 和 iPhone App 之间的一种通信方式是读写同一块共享存储空间，达到数据交换的目的目的（见下图）。需要注意的是 Extension 和 iPhone App 属于不同的进程，要共享存储空间，需要在工程对应的 target 中同时打开 App Groups 的权限，并选择共享的组名（打开权限需要在 Xcode 中配置开发者账号和密码，同时本地需要有对应的开发证书）。
   
但是修改存储文件，并没有达到直接通信的目的。存在这样的问题：iPhone App 对文件进行了修改，Extension 并不知道文件发生了修改，很显然，我们需要通过通知机制告诉 Extension 文件发生了修改，你可以更新新的数据了，本文最后的 Demo 会实现这样的效果。

![list app](/images/tech/iPhoneWatchCommunication/share_container.png)

Extension 和 iPhone App 另外一种通信方式是 Extension 主动向 iPhone App 发起请求，进行某种操作，或者请求数据（场景：Watch 收到新邮件通知后，点击已读按钮，在 iPhone App 上置已读）。

Extension 发起请求：      

	+ (BOOL)openParentApplication:(NSDictionary *)userInfo
	                        reply:(void (^)(NSDictionary *replyInfo,
	                                        NSError *error))reply

iPhone App 回应请求：

	- (void)application:(UIApplication *)application
	handleWatchKitExtensionRequest:(NSDictionary *)userInfo
	              reply:(void (^)(NSDictionary *replyInfo))reply

##Demo

[MMWormhole][2]通过共享存储空间和通知机制实现 Extension 和 iPhone  App 的通信。利用了 CFNotificationCenter 中的 Darwin notification center 来实现进程间通信，即通知机制的实现。大家如果有兴趣可以看看代码。

[1]: http://nilsun.github.io/apple-watch/
[2]: https://github.com/mutualmobile/MMWormhole
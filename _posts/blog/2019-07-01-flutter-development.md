---
layout: post
title: Flutter 跨平台开发
description: Flutter 框架结构以及一些主流的跨平台框架设计思路。
category: blog
published: ture
---

## Flutter 的历史

Flutter 是 Google 发布的跨平台前端开发框架。前身为 15 年在 “Dart 开发峰会”上发布的 Sky。版本发布时间线如下：

* 2017.5 / Alpha 
* 2018.2 / Beta 1
* 2018.5 / Beta 3
* 2018.6 / Preview
* 2018.12 / 1.0 Stable
* 2019.5 / Flutter 1.5  (Flutter for Web)

目前国内公司在业务中使用到 Flutter 技术的应用有： Now 直播、[美团外卖](https://tech.meituan.com/2018/08/09/waimai-flutter-practice.html)、[阿里咸鱼](https://www.jianshu.com/u/cf5c0e4b1111)。在对 Flutter 进行预研的过程中出现频率较高的几大技术关键词: **混合开发、安装包瘦身、工程体系化**。

## Flutter 跨平台开发准备

- 安装 Flutter SDK，使用 flutter doctor 检测本机环境依赖
- 编辑器：Android Studio 或 VS Code
- iOS：XCode 7.2+
- Android: JDK、Android SDK、AVD；安装 Flutter & Dart 插件
- Mac OS：[Flutter desktop embedding](https://github.com/google/flutter-desktop-embedding)

## Flutter 框架

![list app](/images/tech/FlutterDevelop/flutter_framework.png)

参考上图，Flutter 的框架主要分三层：

- Framework 层：使用 Dart 实现的应用层框架，包含 Meterial 风格和 iOS （Cupertino）风格的 Widget ， 图片、按钮等基础 UI Widget；基于 Engine 提供的绘制接口的封装的绘制库 Painting 等。
- Engine 层：使用 C++/C、Dart 实现的平台无关的中间层逻辑：如用 C++ 实现 Skia 二维图形渲染引擎，Dart 实现的虚拟机、内存管理、线程配置管理 等，是 Flutter 跨平台的核心所在。
- Embedder 层：嵌入层，具有平台相关性，主要包含渲染 Surface 设置、事件处理、插件等。

## Flutter 开发实战

### Dart

Dart 由 Google 开发和维护，对标 JavaScript。基于 JS  的开发框架基本达成了全栈开发的目标（NodeJS、RN、React），从 [Dart 官网](https://dart.dev/platforms) 关于开发平台的介绍来看，Dart 的目标也希望为开发者提供一条龙式的解决方案。

与 Java 和 JavaScript 进行对比：相比 Java ，Dart 语法更简洁，在 Dart 2.0 以前，Dart 为弱类型语言，从语法上可以看到 JS 的影子，如 var 变量定义，async/await 语法糖，Promise 链式异步处理等。Dart 2.0 后，变更为强类型语言，相较 JS 更加规范，工程化。

实验下面代码块在 Dart 和 JS 中的执行结果：
```
var i;
i = '';
i++;
```

### Widget

在 Flutter 中，一切皆为 Widget：UI、手势、应用主题等 ...

![list app](/images/tech/FlutterDevelop/widget.png)

Flutter 应用的界面通过 Widget 中的 build 方法层层嵌套组合构建。

Widget 分为 StatelessWidget 和 StatefulWidget 两类，顾名思义，StatelessWidget 是静态的，不涉及状态的变化，StatefulWidget 的状态是可变的，随业务逻辑的改变进行刷新。StatefulWidget 的 build 构造逻辑在 State 中，当界面需要进行刷新时，可以在 State 的 setState 方法中对数据源进行修改，setState 会触发 Widget 的 rebuild，从而进行状态表现的刷新。

### 绘制、渲染流程

在 Framework 层，我们通常调用 setState 进行界面更新，在这个过程 Flutter 进行了哪些处理呢？

setState 被调用后，Widget 被标记为 “need build”，最终调用 scheduleFrame 通知 Engine 在下一帧进行重绘， Engine 在 GPU 发出 Vsync 信号后，调用 _drawFrame 通知 Framework 层进行 rebuild、layout、paint，最终生成 Layer Tree 交付 Engine 层在 GPU 中进行渲染。

![list app](/images/tech/FlutterDevelop/flutter_render_process1.png)

![list app](/images/tech/FlutterDevelop/flutter_render_process2.png)

### Hot Reload

Flutter 在 Debug 模式下使用 JIT 编译模式 （Release 使用 JIT），在运行时进行编译和解释执行。

![list app](/images/tech/FlutterDevelop/flutter_hotreload.png)

## Native 跨平台技术漫谈

- 基于浏览器技术： Hybird
- 基于桥接 Native 组件：RN、WEEX、C++（iOS 使用 OC混编、Android 使用 JNI）  ...
- 基于 平台无关中间层、统一渲染：Flutter、Cocos 2D-X
- 组合： JS or C++  结合  Flutter or Cocos 渲染引擎 ，参考 [《基于 Cocos 的高性能跨平台开发方案
》](https://www.hahack.com/codes/cocos-based-high-performance-cross-platform-app-developing/)，最近美团和微信也在尝试单独使用 Flutter 的渲染引擎提高绘制性能。

下图为 Cocos2D -X 的跨平台架构图：

![list app](/images/tech/FlutterDevelop/cocos2d-x.png)

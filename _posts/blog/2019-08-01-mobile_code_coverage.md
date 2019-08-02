---
layout: post
title: 移动端代码覆盖率统计
description: iOS、Android、RN 代码覆盖率统计的方案以及工程化思路
category: blog
published: ture
---

## 背景

移动应用线上问题（Crash 或 业务使用异常）的出现频率是衡量一个 APP 质量好坏的重要指标之一，如何降低线上问题的频率，是研发和测试都比较关心的话题。

除了把控研发流程中的重要环节（需求、技术方案评审、代码审核等），是否还有其他技术手段可以提升产品的线上质量？“移动端代码覆盖率统计” 是一个值得研究的方向，本文将从 iOS 代码覆盖率统计切入，探讨移动端代码覆盖率的工程化实践。

## iOS 代码覆盖率统计

关于 iOS 代码覆盖率检测原理，美团技术团队的 [这篇博客](https://www.jianshu.com/p/0431b23adba3) 已经进行了较为深入的探讨，归纳覆盖率报告的产生过程如下：

### 1、代码插桩，编译，获取 gcno

代码覆盖率检测，最终目的是获得代码的执行结果（是否执行、每段代码的执行次数）。

首先我们需要对代码进行插桩：即在源代码中插入对源代码执行结果进行统计的代码。

iOS 的插桩过程无需手动对源代码进行修改，只需要在 Xcode 中对编译选项进行修改，打开 `Instrument Program Flow` 与 `Generate Legacy Test Coverage Files` 选项，然后进行编译，在编译期 LLVM 会插入计数代码，实现插桩，编译完成后在 build 文件夹下会生成 `gcno` 文件。

gcno 包含了代码计数器和源码的映射关系，代码覆盖率的获得，我们已经完成了一半。

### 2、安装包，功能测试，获取 gcda

使用 1 中编译出的安装包进行功能测试，在测试过程中，插桩新增的计数代码被执行。这时候我们需要定时将执行结果写入文件，调用系统函数 `__gcov_flush`, 可以在配置目录下生成 gcda 文件，gcda 记录了每段代码的执行次数。

### 3、生成代码覆盖率报告

将 gcno 和 gcda 放在同一目录下，使用 [lcov](http://ltp.sourceforge.net/coverage/lcov.php) 进行文件处理，得到 info 中间文件

```
lcov -c -d . -o  coverage.info
```

这个文件已经包含了全量的代码覆盖率信息。最后一步，我们来生成可读性更好的 `html` 文件

```
genhtml -o html coverage.info
```

覆盖率统计 Demo 的网页展示如下：

![list app](/images/tech/mobileCoverage/coverage_report.png)

## 代码覆盖率统计工程化

上一节，我们简单总结了 iOS 覆盖率统计方法，但是实践过程中还有不少问题需要解决：

- 如何将代码覆盖率整合入常规研发流程

- 一个需求分支，如何只针对这个需求改动的代码输出覆盖率报告

- 一个需求分支，对同一份代码的安装包进行多次运行测试，如何对代码覆盖率报告进行合并

第一个问题，可以和 jenkins 打包流程结合起来，勾选覆盖率测试选项后，打包前，对工程编译选项进行临时修改，整体流程可以参考流程图：

![list app](/images/tech/mobileCoverage/engineer_flow.png)

第二个问题，在上图中，通过 Git Diff 获取单个需求的差异化代码，结合 info 文件进行文本筛选，可以输出单个需求的覆盖率报告。

第三个问题，在每次测试完成，上传 gcda 生成这次测试的 info 文件后，可以使用 lcov 提供的命令对新旧 info 文件合并，从而得到合并后的代码覆盖率报告。

## Android 代码覆盖率统计

Android 代码覆盖率报告生成，和 iOS 类似，可以进行横向对比：Android 中和 gcno 对应的文件为 jacoco，和 gcda 对应的是 ec 文件。

## RN 代码覆盖率统计

RN 代码覆盖率报告生成，主体流程是类似的：首先对 js 代码进行插桩，客户端运行插桩后的 js 代码，生成代码执行文件，结合源码，可以最终生成 js 代码覆盖率报告。实际操作过程有一些区别。

首先我们介绍两个 js 覆盖率统计的相关工具：

[istanbul](https://istanbul.js.org/)：较主流的 js 代码覆盖率工具，具备 js 代码插桩，对运行在 server 端的文件进行自动插桩，代码覆盖率文件合并 等功能，但通常和单元测试结合生成代码覆盖率。了解更多可以参考 [链接](http://www.ruanyifeng.com/blog/2015/06/istanbul.html)。

[istanbul-middleware](https://github.com/gotwarlost/istanbul-middleware)： istanbul 的组件之一，目前已经独立进行维护，主要侧重于满足功能测试代码覆盖率的需求，middleware 提供了数个针对 istanbul 覆盖率收集及报告生成的 http 接口 ，可以将功能测试的覆盖率报告以网页形式展现。

通过上述的工具，我们可以开始 RN 中的 js 代码覆盖率统计：

#### 1、js 代码手动插桩

使用 istanbul 中 nyc instrument 命令，可以对 js 文件进行批量处理，生成插桩后的代码，然后对源文件进行覆盖。这里我们对比可以看到和 iOS 插桩过程的细微差别：iOS 插桩方式是使用方无感，js 需要我们编写脚本进行批量处理，覆盖源文件。

#### 2、启动 istanbul-middleware 的 web server

在 istanbul-middleware 目录中启动服务：

![list app](/images/tech/mobileCoverage/start_server.png)

现在我们可以通过访问  **http://localhost:8889/coverage**  获取代码覆盖率的报告，但 RN 代码并没有加载，现在我们只能看到一个空页面。

#### 3、代码执行文件定时回传 server

修改 App.js 文件，添加 “将代码覆盖率执行文件” 回传 Server 端：

```
setInterval(function() {
  fetch('http://10.100.148.107:8889/coverage/client', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(window.__coverage__)
  })
  .then(function() {
    console.log("success!")
  })
}, 2000);

```

注意需要替换上传文件的地址为 web server 的 ip 地址

#### 4、启动应用，加载本机 RN 代码

运行 react-native ios-run，启动 RN 服务，然后启动 iOS App，从本地加载 RN js bundle，测试 1 中插桩模块的 js 代码

#### 5、查看覆盖率报告生成

打开浏览器，访问 http://localhost:8889/coverage 页面，我们可以看到 4 中的执行结果了：

![list app](/images/tech/mobileCoverage/js_coverage.png)

## 结语

上面对移动端代码覆盖率统计工程体系的搭建进行了初步探讨，但还有不少细节点未展开讨论：如代码修改后的多个包覆盖率文件如何进行合并？RN 如何实现 “增量” 覆盖率统计？这些主题后续有机会再将实践经验进行总结分享，希望这篇文章能给大家在 “移动应用质量提升” 方面带来一些新的思路。 

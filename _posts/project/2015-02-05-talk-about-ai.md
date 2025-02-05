---
layout: post
title: 聊聊人工智能
category: project
description: Talk about the Artificial Intelligence.
published: true
---

关于聊天机器人（ChatGPT）、人工智能的话题，去年（2024）一直想找时间来写写。 作为技术圈内人，从国内外大厂发布各种 AI 产品的密集程度，可以直接感受到这个赛道有多么火热。

直到春节期间，农村开超市的亲戚主动找我了解 DeepSeek 怎么玩，我爸在饭桌上跟我大谈怎么用豆包，DeepSeek 最近多火。我意识到 AI 应用的时代马上要到来了。

## 什么是人工智能？

ChatGPT 或 DeepSeek 并不等同于人工智能，`人工智能（AI，Artificial Intelligence）` 实际上是一个比较泛化的概念：

> 人工智能（AI） 是指让机器具备类似人类的智能，能够执行感知、理解、决策、学习等任务的技术。
> 
> AI 主要通过 算法 + 数据 + 计算能力 来模拟人类思维，让机器能自动完成识别、分析、预测、创造等任务，而不依赖人工干预。

我在网上找了一张图方便大家一览全貌：

![list app](/images/tech/ai/ai_arch.png)

- 最上一层为 AI 技术具体应用的业务场景。
- 第二、三层，为 AI 的应用方向，比如自然语言的处理、计算机视觉、语音识别与处理、机器人与自动化，即 AI 可以解决哪些问题，处理哪些任务。
- 最下面三层为技术实现，从上至下，越来越基础：从软件框架、到算法、到计算硬件。具体的技术细节本文暂不作展开。

## 为什么突然火爆？

人工智能（AI）近两年爆火，主要是因为计算能力的提升和关键技术的突破（对应上图中最底层基础技术的突破），使 AI 从实验室研究真正进入大规模商业化。

过去 AI 发展受限的核心问题之一是 “计算资源不足”，AI 计算的本质是大量的矩阵运算，训练大型 AI 模型需要执行数万亿次计算，如果计算资源不够，训练会变得极其缓慢或无法完成。

但近年来，GPU & TPU 的不断优化（NVIDIA A100、H100；Google TPU v4），使得训练超大规模模型成为可能。

另外，Google 在 2017 年提出了深度学习新架构 Transformer，在自然语言处理上，比传统的 RNN 架构速度更快、效果更强，比如在超长文本的处理上。也得益于此项核心技术的革新，带来了 ChatGPT 等 NLP 应用的横空出世，给 AI 技术带来了前所未有的关注和资本投入。

## 目前发展到哪儿？

在聊目前 AI 发展到哪儿之前，我们先了解下`AGI`和`ANI`的概念：

### AGI 和 ANI

AGI（Artificial General Intelligence，通用人工智能）

>像人类一样拥有通用认知能力，可以跨领域学习、推理、创造新知识。
>
>可以自由适应新任务，而不需要专门训练。

ANI（Narrow AI，狭义人工智能）

>只能完成特定任务的 AI，比如语音识别、图像识别、自动驾驶等。
>
>不能通用推理，也不能跨领域思考

虽然目前 ChatGPT、DeepSeek 等一些 AI 应用开始不断给大众带来惊喜，给行业带来变革。但事实上它们仍旧属于 ANI，它擅长于文本的理解和生成，而这些能力仍然基于数据预训练，而非完全自主思考。

即使 Tesla Autopilot 在自动驾驶领域已经做的让人惊艳，但在机械/机器人控制领域距 AGI 还有很长的路要走，看看那些笨笨的机器人你就明白我的意思了。

## AI 未来将去向何方？

在 2025 年初，我们也预测下未来 AI 技术的应用落地，很有可能那一天会提前到来。当然，在 AGI 时代全面到来之前，人类需要做好充足的准备。

| AI 广泛应用  | 未来发展 | 案例 |
|------|------|------|
| AI 应用软件爆发 | 1-2年 | 创作助手（文章、短视频、图片）、聊天机器人 |
| NLP 领域 AGI 应用| 3-5年 | 生活助理 |
| 特定领域机器人 | 3-5年 | 送快递、无人驾驶、仓库搬运 |
|  AGI 机器人 | 5年、10年 or 20年 | |


## English Version

I had been meaning to write about chatbots (ChatGPT) and artificial intelligence throughout last year (2024). As someone in the tech industry, I could clearly feel how hot this field has become just by looking at the rapid release of various AI products from major companies both domestically and internationally.

It wasn’t until the Spring Festival that I truly realized the AI era was upon us—when a relative who runs a grocery store in the countryside actively asked me how to use DeepSeek, and my dad enthusiastically talked at the dinner table about how he was using Doubao and how popular DeepSeek had become.

## What is Artificial Intelligence?

ChatGPT or DeepSeek is not synonymous with Artificial Intelligence (AI). In fact, AI is a broad and generalized concept:

>Artificial Intelligence (AI) refers to the technology that enables machines to exhibit human-like intelligence, allowing them to perform tasks such as perception, comprehension, decision-making, and learning.
>
>AI primarily relies on algorithms, data, and computing power to simulate human thought processes, enabling machines to autonomously complete tasks such as recognition, analysis, prediction, and creation—without human intervention.

I found a diagram online that provides an overview:

![list app](/images/tech/ai/ai_arch.png)

- The top layer represents the specific business applications of AI technology.
- The second and third layers illustrate AI application domains, such as natural language processing, computer vision, speech recognition and processing, robotics, and automation—highlighting the problems AI can solve and the tasks it can handle.
- The bottom three layers detail the technical foundations of AI, moving from high-level software frameworks to algorithms and, finally, to computational hardware. The deeper the layer, the more fundamental it is. This article will not delve into specific technical details.

## Why Has AI Suddenly Exploded?

The surge in AI over the past two years can be attributed to breakthroughs in computing power and foundational technologies (as seen in the bottom layers of the diagram above), which have transitioned AI from laboratory research into large-scale commercialization.

One of the biggest obstacles to AI development in the past was insufficient computational resources. AI computations rely heavily on matrix operations, and training large AI models requires executing trillions of calculations. Without adequate computing resources, training would be either extremely slow or entirely unfeasible.

However, advancements in GPU & TPU optimization (e.g., NVIDIA A100, H100; Google TPU v4) have made training ultra-large models feasible.

Additionally, in 2017, Google introduced the Transformer architecture, which revolutionized deep learning. Compared to traditional RNN architectures, Transformer models demonstrated significantly faster and more effective natural language processing, particularly for long-text handling. This breakthrough paved the way for the emergence of ChatGPT and other NLP applications, bringing AI into the spotlight and attracting unprecedented attention and investment.

## Where Are We Now?

Before discussing the current state of AI, let's first define two key concepts: AGI and ANI.

### AGI vs. ANI
AGI (Artificial General Intelligence)

>AI with human-like cognitive abilities that can learn, reason, and create new knowledge across various domains.
>
>It can adapt to new tasks without specialized training.

ANI (Artificial Narrow Intelligence)

>AI that can only perform specific tasks, such as speech recognition, image recognition, and autonomous driving.
>
>It lacks general reasoning and cross-domain thinking capabilities.

Although applications like ChatGPT and DeepSeek continue to surprise the public and revolutionize industries, they still fall under ANI. These systems excel at text understanding and generation, but their capabilities are fundamentally based on pre-trained data rather than true autonomous reasoning.

Even Tesla's Autopilot has made impressive strides in autonomous driving, but robotics and mechanical control are still far from achieving AGI. Just take a look at today’s clumsy robots, and you’ll see what I mean.

## Where Is AI Headed?

As we enter early 2025, we can predict the trajectory of AI’s applications. The day when AI fully integrates into our lives may arrive sooner than expected. However, before we reach the AGI era, humanity must be well-prepared.

| AI Adoption	 | Future Development Timeline | Examples |
|------|------|------|
| AI-powered software boom	 | 1–2 years | Content creation assistants (articles, short videos, images), chatbots |
| AGI applications in NLP	| 3–5 years | Personal AI assistants |
| Specialized AI-driven robotics	 | 3–5 years | Autonomous delivery, self-driving cars, warehouse automation |
|  AGI robots	 | 5, 10, or even 20 years	 | |

While we are still years (or decades) away from true AGI, the rapid evolution of AI applications is already reshaping industries. The question is not if AI will transform our lives but rather how soon and to what extent.









---
layout: post
title: 端智能和大模型技术如何在移动端业务中落地
description: How On-Device Intelligence and LLM Are Applied in Mobile Business
category: blog
published: ture
---

## 基本概念
在人工智能领域，模型是算法的实现：一系列数据的输入，通过模型推理，映射到最终输出。由于模型的训练和推理过程对计算资源（GPU)、内存容量的需求较高，通常人工智能的工程链路部署在服务端，比如目前大模型基本都部署在后端。
而端智能技术则是将人工智能能力部署在边缘设备（手机、IOT设备、工业传感器等）上的技术，和传统后端 AI 模型不同，端智能技术强调在设备本地进行数据处理和决策，无需将所有数据上传到“云端”进行计算决策。

## 业务场景落地

### 端智能

聊到技术在业务中的落地，我们首先应该明确技术的优势特性，以及利用这些优势在对应业务场景能否较其他技术方案做的更好。

关于端智能技术，突出优势有两点：

1. 用户隐私保护，由于推理过程发生在本地，用户敏感数据不需要上传至服务端消费；

2. 实时响应，不需要依赖服务端获取决策结果，减少一次网络请求服务端响应的过程，可以提供用户更流畅的交互体验；

针对第 1 点，可以思考端侧有哪些用户隐私数据：相册图片、用户交互行为、IM 中输入的文字、生物特征信息（面容、指纹），对应的落地场景推演：

相册图片 -> 图片背景橡皮擦 [CleanUp](https://i.ifeng.com/c/8cQzutsNdpo)

![list app](/images/tech/onDeviceIntelligence/clean_up.png)

用户交互行为-> 感知用户侧异常，如 按钮显示不完整点击不响应、弹窗不能隐藏阻塞交互、白屏/黑屏 ...

针对第 2 点“实时响应”，我们可以考虑业务中有哪些复杂决策场景希望实时返回结果，不进行网络请求即同步提供用户交互的。
下面拿一个具体案例来说明，在支付场景，在用户退出流程时，很多产品的界面上会弹出弹窗，弹窗的类型可能有很多种：身份核验方式推荐弹窗、商品标签弹窗、切换支付方式弹窗、营销优惠弹窗 等等。通常退出时应该出什么弹窗，在用户进入支付流程后服务端即下发，服务端是基于历史数据作的弹窗类型推荐。但问题在于在当笔支付流程中，服务端推荐可能是不准确的，比如用户当下忘记了密码，多次输入密码校验不过，基于历史数据下发营销优惠，肯定不是用户预期的，这时推荐“切换核验方式弹窗”更能帮助用户完成当笔支付。基于上述推演，可以考虑对上述流程进行优化：服务端根据历史数据粗排，下发所有用户可能使用的弹窗数据列表，当用户退出支付流程时，端侧再根据本次支付的用户交互和动线信息进行“智能决策”。可能你会有疑问，为什么不在用户退出时请求服务端返回最终的弹窗类型呢？因为在这个场景，期望提供用户同步的交互，用户退出时弹出 loading 交互提示等待显然是不可接受的。

![list app](/images/tech/onDeviceIntelligence/cashier_retain.png)

### 大模型

关于后端大模型的应用场景，本文不作重点讨论，基于大模型强大的能力，业界已经有丰富的落地场景呈现：基于文本理解和生成的 ChatGPT、垂直领域搜索、企业客服，基于多模态能力的内容创造（图片、视频）等等。

## 端智能和大模型结合

再回到移动端，假设在未来算力、资源和性能问题有具备较好的解决方案，结合大模型强大的能力，端侧大模型可以创造出哪些价值？
如何整合端智能和大模型的优势？数据隐私安全、实时性；文本、图片、音视频多模态理解和生成能力。
苹果在 Apple Intelligence 中已经给出了一些他的实践：

全新的 Writing Tools：帮助用户进行文本纠错、润色、核心概念总结和提炼...

![list app](/images/tech/onDeviceIntelligence/writing_tools.png)

Genmoji：在聊天场景，根据聊天内容上下文，提供个性化的 Emoji 生成和提示，让你的聊天体验更加愉悦。

![list app](/images/tech/onDeviceIntelligence/gen_emoj.png)

可能你有个人隐私方面的担忧。放心，苹果已经考虑到了，这一切都发生在你的移动设备上，你的个人数据没有上传服务端进行存储和使用。

## 结语

本文关于端智能和大模型技术在移动端的业务落地进行了一些引导式的讨论，关于业务场景的分类和对应案例，未做完整的梳理和展开，后续有机会可以继续延展。另外，读者有兴趣也可以针对自己熟悉的业务场景的技术应用进行进一步深入探索。
最后，让我们再试试大模型在文章翻译上的表现，你觉得翻译的怎么样？

## Basic Concepts

In the field of artificial intelligence, the model represents the implementation of algorithms: input data is processed through model inference and mapped to final outputs. Given that both model training and inference require significant computational resources (e.g., GPUs) and memory capacity, AI pipelines are often deployed on servers. Currently, large models are predominantly hosted on the backend.
On-device intelligence, by contrast, refers to deploying AI capabilities on edge devices (e.g., smartphones, IoT devices, industrial sensors). Unlike traditional server-based AI models, on-device intelligence focuses on processing data and making decisions locally on the device, without the need to upload all data to the cloud for computation and decision-making.

## Application Scenarios

### On-Device Intelligence

When discussing technology applications in business scenarios, it’s essential to understand the key advantages of the technology and determine whether it outperforms alternative solutions in specific scenarios.

The notable advantages of on-device intelligence include:

* User Privacy Protection: Since inference occurs locally, sensitive user data doesn’t need to be uploaded to the server.
* Real-Time Responsiveness: It avoids relying on server-side responses, eliminating the latency caused by network requests and enabling smoother user interactions.

For user privacy protection, consider what types of sensitive data are stored on the device:

* Photo albums: Applications like photo background erasers (e.g., [Cleanup](https://i.ifeng.com/c/8cQzutsNdpo)) process images locally.
* User interactions: Detecting anomalies such as unresponsive buttons, blocked pop-ups, or white/black screens during user interactions.
* Input text in IM apps: Handling and analyzing text input for features like smart suggestions or typo corrections.
* Biometric information: Processing facial recognition and fingerprint data locally.

![list app](/images/tech/onDeviceIntelligence/clean_up.png)

For real-time responsiveness, consider scenarios requiring complex decision-making with instantaneous results, without sending network requests.

***Example Use Case: Payment Scenarios***

In payment processes, when a user exits a payment flow, various types of pop-ups might appear, such as:

* Identity verification suggestions,
* Product label prompts,
* Payment method switching options,
* Promotional offers.

Typically, the server pre-sends these pop-up options based on historical data. However, server-side recommendations might not always align with the user's current situation. For example: If a user forgets their password and fails multiple authentication attempts, a promotional offer based on historical data would be irrelevant.Instead, recommending an "alternative verification method" pop-up would be more helpful. An optimized approach could involve:

* The server pre-ranking and sending all potentially relevant pop-ups based on historical data.
* The device making real-time intelligent decisions based on the user’s actions and flow in the current session.

Why not send a real-time request to the server for the final pop-up decision?
In such scenarios, the goal is to provide synchronous user interactions. Displaying a "loading" spinner while awaiting a server response would detract from the user experience.

### Large Models

This article doesn’t focus on backend large model applications, as there are already numerous well-established use cases showcasing their power:

* Text understanding and generation: ChatGPT, vertical domain search, enterprise customer service.
* Multi-modal capabilities: Content creation (images, videos).

## Combining On-Device Intelligence and Large Models

Looking toward the future, assuming challenges around computing power, resources, and performance are resolved, what value could large models on devices bring?

How can we combine the strengths of on-device intelligence and large models?

* Data privacy and security: Leveraging on-device processing to safeguard user data.
* Real-time responsiveness: Delivering instant results for complex interactions.
* Multi-modal capabilities: Harnessing text, image, and audio-visual understanding and generation.

Apple’s Implementation of On-Device Intelligence and Large Models

Apple has demonstrated practical examples of these concepts through Apple Intelligence:

* Writing Tools: Assisting users with text correction, refinement, and summarization of core concepts.

![list app](/images/tech/onDeviceIntelligence/writing_tools.png)

* Genmoji: In chat scenarios, generating personalized emoji suggestions based on the context of conversations, enhancing the chatting experience.

![list app](/images/tech/onDeviceIntelligence/gen_emoj.png)

Concerned about privacy? Rest assured, all processes occur locally on your device, ensuring your personal data isn’t uploaded to servers for storage or processing.

## Conclusion

This article provided an introductory discussion on the application of on-device intelligence and large model technology in mobile business scenarios. While the categorization of business scenarios and corresponding examples wasn’t fully elaborated, this topic has significant potential for further exploration. Readers are encouraged to delve deeper into technical applications within their familiar business contexts.
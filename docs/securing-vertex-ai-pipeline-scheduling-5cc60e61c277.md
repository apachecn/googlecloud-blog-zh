# 保护顶点人工智能流水线调度

> 原文：<https://medium.com/google-cloud/securing-vertex-ai-pipeline-scheduling-5cc60e61c277?source=collection_archive---------3----------------------->

![](img/1d910bfd4e245b786a87dad001de0f16.png)

[Vertex AI Pipelines](https://cloud.google.com/vertex-ai/docs/pipelines/introduction) 提供了一种在谷歌云上以无服务器方式编排机器学习工作负载的好方法。它基于 [KubeFlow Pipelines](https://www.kubeflow.org/docs/components/pipelines/) ，这是一个使用容器构建机器学习(ML)管道的开源平台，使 Vertex AI 中的管道开源兼容。

然而，与 KubeFlow 管道不同，顶点管道没有用于调度管道运行的内置机制。对于许多用例，调度流水线运行的能力是 ML 自动化的关键要素(例如，调度批量预测流水线、调度模型再训练等)。

公开发布的顶点人工智能文档提供了一种使用云调度器服务来调度顶点人工智能流水线作业的方法。

1.  云调度器，用于调度 HTTP/S 请求，这将触发云功能
2.  云函数将使用顶点 AI SDK 触发顶点管道

![](img/ebbfa00fd496fc416b940d70d0317355.png)

图 1: Google 提出的顶点流水线调度方法

在这个公开记录的方法中，我们可以使用云调度程序安全地调用带有 OIDC 令牌的云函数。

在这种使用云功能的方法中，您需要发送带有身份验证标识令牌的请求。Cloud Scheduler 允许您指定多种身份验证头，从而为您实现了自动化。OpenID 连接(OIDC)令牌是在请求头中提供令牌的最常用方式。

将创建一个带有 HTTP 触发器的云函数。Cloud Scheduler 将使用相同的 http 端点来触发使用 OIDC 令牌的云功能，如下所示(图 2)。

![](img/b72f6e4fcedd1288c538ed8cfad54d82.png)

图 2:使用 OIDC 设置的时间表定义

在云功能方面，您可以使用 [VPC 服务控制](https://cloud.google.com/functions/docs/securing/using-vpc-service-controls)为您的云功能添加额外的网络级安全层。VPC 服务控制强制对云函数 API 的调用将失败，除非它们来自服务边界内。

但是请注意，VPC 服务控制不支持具有以下目标的云调度程序作业:

*   应用引擎
*   超文本传送协议

因此，来自云调度程序的所有调用都将失败，因为它们将被视为来自服务边界之外。

因此，我们必须使我们的云函数面向公众，如下所示(图 3):

![](img/a234963416e63e2f7dcc974ce65fd520.png)

图 3:云功能的网络配置

上述方法的安全性限制:

*   面向公众的云功能端点使其易受攻击(如 DDOS)。

为了克服这种安全限制，您可以按照下面提到的特定顺序使用 GCP 服务。

*   云调度程序作业向发布/订阅发送消息
*   发布/订阅主题以触发云功能
*   一个云函数，将使用顶点 AI SDK 触发顶点 AI 管道

![](img/aa43d321263037cec57ab157642e3c64.png)

图 4:触发顶点 AI 流水线的安全方法

在云函数中，发布/订阅触发器使函数能够被调用，以响应通过云调度程序传递的[发布/订阅](https://cloud.google.com/pubsub/docs)消息。每当有消息发布到指定主题，就会触发您的云功能。

![](img/fa91df338a5896be72f58b9af9e2db4a.png)

图 5:将发布/订阅作为触发器的云函数

[Pub/Sub](https://cloud.google.com/vpc-service-controls/docs/supported-products#table_pubsub) 由 VPC 服务控制部门全力支持。VPC 服务控制保护适用于发布/订阅情况下的所有管理员操作、发布者操作和订阅者操作。

源自发布/订阅的所有事件将被视为来自服务边界内，因此不需要使我们的云功能面向公众，如图 6 所示。

![](img/8f31b8b47efeadf397ba3b07d0e68968.png)

图 6:云功能的网络配置

这就是如何通过利用云调度器、发布/订阅和云功能服务，以更安全的方式调用顶点 AI 管道。

………..感谢您阅读博客，祝您有美好的一天！…………
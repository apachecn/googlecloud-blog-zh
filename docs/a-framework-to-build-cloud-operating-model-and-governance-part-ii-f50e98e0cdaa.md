# 构建云运营模式和治理的框架—第二部分

> 原文：<https://medium.com/google-cloud/a-framework-to-build-cloud-operating-model-and-governance-part-ii-f50e98e0cdaa?source=collection_archive---------3----------------------->

# 第一部分总结

在[第一部分](/google-cloud/a-framework-to-build-cloud-operating-model-and-governance-part-i-7f87bf66b241)中，我们讨论了在本地运行的治理和运营模式的历史背景，强调了在云中按原样采用时产生的一些差距，最后我们提出了对开发人员和治理的一些要求，并以帮助构建治理的支柱结束了云中的治理阶段。

所以，让我们在第二部分继续讨论…

# 着陆区

让我们介绍一下着陆区这个术语。有许多可用的定义，但在我看来，它是一个工程团队构建产品/应用程序的云环境。着陆区是一个具有预先配置的网络、安全、护栏、预算、IAM、可观察性等的包。基于工程团队所需的通用设计模式。有许多定义将着陆区描述为一个完整的云工作环境，但在我看来这是一个非常单一的概念。因此，我总是认为“a”着陆区和“a”工程/产品团队之间存在 1:1 的关系，该团队通过各种产品为该组织的客户提供价值。一个组织有许多这样的团队，因此需要一个以上的着陆区。默认情况下，所有着陆区相互隔离(非重叠爆炸半径)。谷歌云中的“着陆区”和“项目”有许多相似之处。

# 云层的灵活性和稳定性

再看云。一般来说，与应用层相比，云基础的变化不太频繁。开发人员一周几次更改、构建和部署单个应用程序，而您并不经常更改 VPC 结构或混合连接或项目结构。这并不意味着基础是静态的(创建一次，永不改变)。对于一个典型的组织来说，稳定性在基础层更重要(#1)，而敏捷性在应用层更重要(#3)。从治理的角度来看，问题出现了；一个组织如何为开发人员提供最大可能的敏捷性和独立性(在第三层)，同时保持所有其他层的稳定和敏捷(与敏捷框架相混淆)。

# 大局

让我们试着把所有的东西都整合到一个大的画面中，以衍生出一个框架来构建一个治理和运营模型。

这是一个真正的协作模型，混合了集中和分散的组件。它允许专业化团队使用代码构建治理，并将其提供给消费者。

# 云基础层

专家知道构建和运行一个基础部分需要什么。在该模型中，专家构建模块，为工程团队创建云环境和着陆区的基础部分。专家团队可以来自不同的领域，如安全、网络、基础设施等。在基础层，稳定性和控制更重要，因此设置它的模块可以是私有的，并且只能通过自助服务门户或基础构建/发布管道来触发(如果需要集中审批)。

![](img/b6b3a3b8cf3487dfd85798b73bf5efe4.png)

*   示例 1:像 Lia 这样的网络专家可以构建模块来创建 VPC、分配子网、在 VPC 中创建防火墙等。这些模块在创建时附带了护栏以及其他治理需求，例如，在模块中分配治理所需的元数据“标签”,启用 VPC 流日志、防火墙日志等。然后，网络专家决定哪些模块可以由工程团队在其基础设施构建管道中直接使用，哪些模块是私有的，只能由集中式工作流使用。这个决定可以基于云蛋糕模型(如上)做出。
*   示例 2:基础工程师 Siva 构建资源层次结构的代码、确保启用计费的项目、在相关 VPC 中为其分配子网(使用由网络专家团队创建的模块)、为项目分配标签、启用 CCoE 批准的服务、启用审计日志并创建日志同步(使用由安全团队中的 Jack 创建的模块)以将审计日志拉至集中位置等。这些代码随后被用于为 Tim 和 Ashley 创建项目，以便在 Google Cloud 上使用。

一般来说，具有协作和共同创造的运营模式的集中治理非常适合基础层。这将支持工程团队在云中加速。

*   设计时治理:类似于代码开发
*   构建时治理:类似于代码部署、护栏、组织策略等。
*   运行时治理:CSPM、可观察性、组织策略、护栏等。

从基础的角度来看，可以使用项目工厂方法在 Google Cloud 中构建治理。一个示例项目工厂可能如下所示。

![](img/afe9344b88cc7e4bafb24562126c4bc9.png)

# 云服务层

在谷歌启用云服务只需点击一个按钮或运行一个 [terraform 模块](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/google_project_service)。例如，使用 TF 模块在项目中启用发布/订阅看起来像这样。

```
resource "google_project_service" "pubsub_api_enable" {
  project = "your-project-id"
  service = "pubsub.googleapis.com"
  disable_dependent_services = true
}
```

治理可以决定组织中应该允许哪些服务(基于可用的护栏、云路线图、合规性等)。)，然后使用这样的模块在所有项目中启用那些服务。作为项目创建过程的一部分，默认启用所有允许的服务是很重要的，以确保工程师不必创建额外的请求来启用服务。

为每个服务定义护栏(治理姿态),然后将它们作为服务实现过程的一部分。这应包括:

*   IAM(授予各种角色的默认角色，可能的 VPC 服务控制等。)
*   网络控制(默认防火墙规则和策略等。)
*   可观察性(要观察的指标、审计日志等。)
*   演员表
*   组织政策
*   更多…

应该采用基于社区的方法来构建服务供应模块。在现实世界中，这些模块在 git repo 中进行版本控制，主/主分支受到保护。组织中的任何开发人员都应该能够创建一个 pull 请求来贡献或建议更改。

资产供应模块应该实现这些护栏。例如，发布/订阅预配模块还应该将允许的 IAM 角色分配给所需的组。

![](img/b803ad4a56124e54df73c64cee651085.png)

*   示例 1: Noah 是一名基础设施工程师，他创建了一个构建托管实例组(GCE)的模块。治理团队的 Mike 对该模块进行了审核，以确保所有护栏都已安装到位。此模块需要开发人员提供一些信息来调配资源。这个模块是从 Jennifer 开发的发布管道中调用的，用于在 Google Cloud 环境中提供 MIG。
*   示例 2(图中未显示):数据库专家 Maya 创建了一个构建云 SQL 实例的模块，并提交到相关 IaC 存储库的“变更分支”,并创建了一个 pull 请求，供 Noah 查看该模块。Noah 和 Mike 一起检查了这个模块，以确保所有的防护栏都在适当的位置，然后提交到主分支，以便其他开发人员(如 Jennifer)可以使用它。Jennifer 可以从她的模块中引用这个模块来提供一个云 SQL 实例。

开发人员可以通过将参数传递给 IaC 模块来自由使用这些模块，从而在云环境中以一致的方式提供资源，并在默认情况下实现正确级别的护栏。要考虑的事情:这里的一个常见问题是，如何确保开发人员使用批准的模块来提供资源，而不是构建他们自己的模块(可能没有集成所有的护栏)。这就是治理的第三阶段变得极其重要的地方。这个阶段需要确保在云环境中调配的资源处于防护栏之下。任何漂移检测都必须通知项目所有者，或者破坏资源。这就是护栏在环境中的实施方式。

像这样的治理框架可以建立在协作文化的基础上，但仍然要确保从运营模式的角度来看，这使得开发和产品团队可以通过遵循既定护栏的自动化来随意调配资源。

# 结论

随着越来越多的人采用云，开发团队通常希望在云环境中有更多的自主权来调配资源。传统的治理和运营方法可能在这样的联合环境中不起作用。传统的高压治理需要大量的审查和批准，而在本地环境中，由于各种原因，基础设施的供应和使用受到严格控制；在云环境中，应该给开发/工程团队更多的自由来控制他们自己的基础设施。这可以通过自动化、基于代码的策略和防护、协作和使用正确类型的工具来实现。

在未来的帖子中，我计划更多地讨论 GKE 的治理和运营模式，并通过几个代码示例来展示本文中提出的一些想法在现实生活中是如何工作的。

希望这篇文章能为在你的组织中建立正确的治理和运营模式提供一些思路。
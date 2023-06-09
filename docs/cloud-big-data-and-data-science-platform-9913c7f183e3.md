# 云、大数据和数据科学平台

> 原文：<https://medium.com/google-cloud/cloud-big-data-and-data-science-platform-9913c7f183e3?source=collection_archive---------0----------------------->

你好。今天，我将谈论云、大数据和数据科学，以及谷歌云如何成为您永远需要的一个平台。在这篇文章中，我将重点关注“数据公司”的基础设施需求，这些公司将数据作为其核心资产。什么是数据公司？任何由数据驱动、基于数据做出决策、基于数据改进产品的公司都是数据公司。数据公司的例子包括谷歌、网飞、优步、Airbnb 和 Spotify。他们都有相同的 DNA，收集大量数据，处理数据，发现数据中的模式，通过服务提供价值并赚钱。Spotify、优步和 Airbnb 等现代数据公司更是如此。最大的音乐公司 Spotify 没有一张专辑；最大的运输公司优步没有一辆运输车辆；最大的住宿公司 Airbnb 没有一间小屋。数据是他们的核心资产。

为了有效地处理数据，您需要几个平台。

*   云平台可大规模供应资源。
*   处理数据的大数据平台。
*   从数据中学习模式的机器学习平台。
*   实时传递价值的流媒体平台。

当你使用像 AWS / IBM / Azure 这样的云提供商时，你是在使用大量的系统。你的云提供商(AWS / IBM / Azure)提供硬件资源；用于处理数据的大数据平台(Hadoop / Spark)。你还需要有流媒体平台(Kafka / AWS Kinesis)、机器学习平台(IBM Watson / Nervana)和容器平台或集群管理器(Cloud Foundry / Mesos)。拥有如此多的平台是一个令人头疼的维护问题。不仅如此，他们彼此不理解或不能很好地合作。

当你使用谷歌云时，你的所有平台需求都得到了满足。

*   云:Google Cloud 提供所需的资源(虚拟机、存储桶、数据库、IAM、监控工具等)。在云之上，Google Cloud 提供了多种服务，如 App Engine、Cloud Shell (Bastion as Service ),并负责 SSH 密钥管理等完整性，从而减少了开发运维需求。与 AWS 和微软产品相比，虚拟机启动速度快 5 倍，网络结构快一个数量级，云存储和磁盘快约 2 倍，本地固态硬盘快约 5 倍。创新的持续折扣让您可以根据自己的需求灵活地采购资源，而不会被长期锁定。
*   大数据:谷歌云最大的优势是它的大数据堆栈。Dataflow(轻松处理 Pb 级数据)、Pub/Sub(一天 10 次通过它发送“互联网”)、Bigtable(用于存储 Pb 级数据的互联网规模 NoSQL 数据存储)、BigQuery(相当于在定制硬件上运行的 Parquet 列存储+ Presto 查询引擎+ Airpal for Web UI+ NoOps)领先其他行业领先工具 5 到 10 年。
*   流媒体:云发布/订阅是一个互联网规模的消息系统，具有灵活的推送或拉取式订阅。这是一项全球服务，也就是说，你可以在一个地区向它发送信息，在另一个地区接收信息，而不需要任何额外费用。它可以瞬间扩展到数百万条消息。默认情况下，通过加密网络数据和静态数据来提供数据安全性和保护。它可以与各种接收器和源一起工作，包括数据流(用于流处理)、云存储(用于持久存储)、云日志、云监控等。,
*   数据科学:谷歌云在数据科学方面提供一系列服务。它提供了像[视觉 API](https://cloud.google.com/vision/) 、[语音 API](https://cloud.google.com/speech/) 、[翻译 API](https://cloud.google.com/translate/) 这样的机器学习 API，让开发者很容易上手。云 ML，TensorFlow 的托管版本，将使数据科学家能够训练 ML 模型，而不必担心基础设施。如果你喜欢开发新的模型，你可以使用 TensorFlow，这是开源的。如果你想以一种特别的方式探索数据，你可以试试云数据实验室，一个托管版本的 IPython 笔记本。Datalab 与 BigQuery 和云存储集成得非常好。
*   容器:Google Cloud Container Engine (GKE)是 Kubernetes 的托管版本，是最流行的容器化应用程序开源管理解决方案。Kubernetes 在 GitHub 上有近 1 K 开发者，30 K 提交，15 K 明星。它具有自动装箱、自我修复、水平扩展、无缝服务发现、内置负载平衡器、自动转出和回滚等功能。Kubernetes 使大型集装箱管理变得容易。

当您使用 Google Cloud 时，您使用的是一个统一的平台，可以满足您对云、大数据、流、数据科学和容器系统的所有需求。你不需要与那些彼此不能很好合作的众多系统斗争。想象一下节省的时间和释放的生产力！
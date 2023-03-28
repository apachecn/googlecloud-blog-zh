# GitHub 和开源数据的力量

> 原文：<https://medium.com/google-cloud/github-and-the-power-of-open-source-data-b43b34c1beb6?source=collection_archive---------1----------------------->

![](img/336bcef1da7ed9194a0283a6c4697831.png)

昨天， [Github 宣布将 280 万个开源存储库的活动数据作为公共数据集在 Google Big Query](https://github.com/blog/2201-making-open-source-data-more-available) 中公开。这将允许用户对 Github 数据执行 SQL 查询，并提供关于开源项目的复杂的、接近实时的分析。

昨天的声明是 GitHub 致力于开源数据的又一个例子。2012 年，GitHub 宣布发布 [GitHub Archive](https://www.githubarchive.org/) 项目，该项目提供了关于开发者使用 GitHub 方式的一系列初步分析和见解。从很多方面来说，BigQuery 数据集可以被认为是 GitHub 归档项目的扩展。

GitHub 的 BigQuery 数据集是一个非常有价值的数据源，有助于理解开源项目的特征。通过使用 BigQuery 分析数据，我们可以确定特定项目、贡献者、用户偏好等有趣的使用模式。然而，我认为 GitHub 的声明具有更深远的意义，如果我们想想如果其他公司遵循同样的道路会取得什么样的成就。

昨天，GitHub 不仅仅发布了一堆历史数据。通过利用 BigQuery，GitHub 在一个平台上发布了数据，该平台针对分析工作负载进行了优化，并且还可以与市场上一些最流行的分析平台进行互操作。

我当然为 GitHub 的深思熟虑的方式鼓掌，他们处理他们的私有数据源的发布。我认为这个版本肯定会扩大关于开源数据的讨论。以下是我认为值得考虑的几个有趣的点:

# 其他公司应该考虑开源数据集

如果更多的公司效仿 GitHub，我们很快就会有一个公共市场，在这个市场中，可以使用简单的 SQL 结构实时聚合数据。想象一下，GitHub 的项目数据与 LinkedIn 的数据相结合，将开发人员的工作经历与其开源承诺联系起来。

# API 不总是足够的

许多公司通过 API 提供数据，但这不足以实现复杂的分析。首先，更多的 API 只提供特定数据资产的当前视图，而不关注历史数据。此外，大多数 API 不使用与分析工具兼容的数据访问协议。

# 访问和组合数据源的通用平台

开源数据不仅仅是发布一堆 CSV 文件供下载。通过利用像 Google 的 BigQuery 这样的平台，公司现在可以使用简单的 SQL 结构组合和聚合数据，并将这些查询作为新的数据源公开，这些数据源又可以在其他查询中使用，以实现更复杂的分析工作负载。

# 开放源数据并获取更多情报

数据是任何公司最宝贵的资产之一。从这个角度来看，我们可以认为许多机构公开他们的数据来源是疯狂的。然而，通过提供数据并允许数据科学家将其与其他公共或私有数据源相结合，组织可以获得前所未有的关于其业务的新见解和情报。

# 几十年前，我们听到了关于代码的争论

从隐私到知识产权挑战，有许多论据可以用来反对开源数据。在很大程度上，这些论点与几十年来反对开源代码的论点非常相似。虽然今天开源代码的好处是不可否认的，但几十年前人们并没有很好地理解它。同样，我相信在我们充分利用这些好处之前，开源数据将不得不经历几次迭代。
# 在应用中管理支付:整合 Stripe

> 原文：<https://medium.com/google-cloud/managing-payments-in-your-app-incorporating-stripe-973300ee4e6a?source=collection_archive---------2----------------------->

要让我的钩针图案店开张，还有很多事情要做。 [*到目前为止*](/google-cloud/managing-payments-in-your-app-setting-up-inventory-8b4d0f86f82d) *，我有一个功能，把库存数据添加到云 Firestore。现在我们需要一种获得报酬的方式！*

你是这个系列的新手吗？查看 [*第一篇博客*](https://bit.ly/33qptv1) *的介绍和目录！*

# 付款是怎么回事？

![](img/0a4000ecb4dea7f0831ca3fe089986fe.png)

[*图像来源*](https://unsplash.com/photos/imlD5dbcLM4)

我本打算为这一部分想出一个更好的标题，但我决定采用这个听起来像一个糟糕的单口相声的标题。但玩笑到此为止，因为这是严肃的。

将支付整合到应用程序中不同于实现其他功能，因为它需要了解规则和法规。拿顾客的钱是很严肃的事情！(我知道我说了两次“认真”。但它确实很严重，我查了“严重”的同义词，它们都有点离题。)

# 强客户认证

在欧洲销售产品时要考虑的一个重要法规是强客户认证，即 SCA。SCA 是欧盟对支付的一项要求，可确保电子支付通过多因素身份认证来执行。这是为了提高电子支付的安全性。

我们都熟悉各种帐户的多因素身份认证，通常涉及发送短信、共享预定义列表中的代码或批准已知良好设备的操作。SCA 还试图将这种安全级别添加到电子支付中。

> 实施付款管理时考虑法规

如果你觉得特别有兴趣，你可以自己通读一下指令！说这份文件有很多内容是轻描淡写的。但是，尽管确保您理解 SCA 的含义很重要，但它不应该成为您的专业领域。你的重点应该是你的软件！

> 确保您的支付处理器符合您可能开展业务的任何领域的最新支付要求。

Stripe 有一系列关于法规、产品和商业见解的指南。

![](img/44fb4e6b5e5421f527f4257c3ebf9259.png)

# 条带检验

Checkout 创建了一个安全的 Stripe 托管支付页面，让您可以快速收款。更好的是，结帐支付流程将自动添加新的支付方式，并帮助您遵守监管要求，而无需对您的集成进行任何更改。

> *条纹检出符合监管要求*

使用 Stripe Checkout 的第一步是创建代表您的产品和产品价格的产品和价格对象。您可以在条带控制台中或通过 API 来完成此操作。就个人而言，我喜欢使用 API，所以这就是我将向您展示的内容。这最适合我们的特定用例，因为您可以直接运行该函数，而不必来回查看商品和价格列表来将它们添加到控制台。

![](img/d2ad22ded0c4faf7cc2ff20223c046ce.png)

*糟糕，他忘记在卡片背面签名了！* [*图片来源*](https://unsplash.com/photos/Q59HmzK38eQ)

# 准备好了吗？

现在，您对复杂的支付处理有了一点了解，您已经准备好构建项目的这一部分了！以下是接下来要采取的一些步骤:

*   查看[条带文档](https://bit.ly/3fpxpih)
*   查看下一篇博文
*   查看[第一篇博客](https://bit.ly/33qptv1)中所有帖子的链接
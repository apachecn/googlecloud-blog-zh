# 使用 Google 登录验证用户

> 原文：<https://medium.com/google-cloud/authenticating-users-with-google-sign-in-cb257d4fc722?source=collection_archive---------2----------------------->

我讨厌处理用户认证，所以我很乐意让用户管理和认证成为别人的问题。像谷歌这样的“人家”。安全地处理用户信息，支持各种多因素认证，实现帐户恢复，同时避免帐户劫持…这些都是谷歌比我处理得好得多！

对我来说不幸的是，我发现让谷歌为我做这件事很令人困惑。哦，在谷歌的网站上有关于如何做的准确而详细的信息。很多信息。关于很多不同的方法。主要是使用库，至少对我来说，很难调试我做错的事情。所以我决定从基础做起，详细地完成让我的服务器端网络应用程序使用谷歌登录所需的基本步骤。这篇博文展示了我是如何做到的，如果你愿意，你也可以这样做。

# 逻辑步骤

我喜欢为我的应用程序管理会话，所以我真正的问题是在我创建这样的会话之前验证用户的身份。其步骤如下:

1.  浏览器向我的应用程序发送请求。我的服务器检查是否有当前活动的会话，如它设置和信任的 cookie 所指示的。如果有也没关系，我的 app 返回请求的页面。否则，我的应用程序**会返回一个网页**，声明用户需要认证才能进一步使用该网站。该页面有一个链接，用户应该点击这样做。(或者，我的应用程序可以只发送一个链接的重定向响应，但这很突然，可能会让用户感到困惑。)
2.  *用户点击链接，浏览器向谷歌发送页面请求。那个链接是到 accounts.google.com 的一个谷歌页面。该链接的确切格式将在本文稍后描述。谷歌将返回网页，并根据需要处理响应，以验证用户身份。如果认证成功，Google 将**返回一个指向我的站点页面的重定向响应**。*
3.  *用户的浏览器通过向我的应用程序发送请求来处理重定向。*我的服务器使用重定向 URL 中的信息从 Google 检索用户信息。这种检索是从我的网络服务器到谷歌，而不是从用户的浏览器。如果一切顺利，我的服务器现在知道用户是谁，所以服务器创建一个会话，然后**向用户返回一个响应**，其中包含一个头，为该会话设置一个 cookie。这个响应**可能是一个网页，或者重定向**到用户最初请求的页面。但是现在用户的浏览器有了有效的会话 cookie，用户可以访问我的站点了。

在这三个步骤中，我的服务器必须处理**步骤 1** ，在这里它得到一个请求，这个请求可能有也可能没有与之相关联的活动会话，如果没有，就必须用一个页面进行响应，或者用正确的 URL 进行重定向。该 URL 将用户的浏览器发送到谷歌进行**步骤 2**；这是谷歌要处理的，所以我的应用程序没有参与。最后，谷歌将浏览器重定向回我的应用程序进行**步骤 3** ，我的服务器必须使用重定向 URL 中的信息来获取用户的身份。

那么我们来看看一个 app 是如何处理**步骤 1** 和**步骤 3** 的。但是首先，有一个**步骤 0** 。当任何人要求时，谷歌都不会确认用户身份。想要使用 Google Sign-in 的应用程序需要首先向 Google 注册。

让我们来看看让您的 web 服务器使用 Google sign-in 所需的步骤。

# 第 0 步——向谷歌注册应用程序

你需要在 console.cloud.google.com[创建一个谷歌云平台项目。为此，你可以使用 Gmail 或 G Suite 帐户，或者在被要求时创建一个普通的 Google Cloud 帐户。你可能必须提供一张信用卡，但有一个慷慨的免费试用，如果没有事先通知，试用结束时你不会收到账单，而且在任何情况下，这种注册不需要使用任何目前需要花钱的服务。](https://console.cloud.google.com/)

一旦有了项目，使用控制台左侧的菜单选择**API&服务**，然后选择 **OAuth 同意屏幕**。除非您打算只验证您控制的 G Suite 域中的用户，否则您的应用程序将被视为外部应用程序，因此您将选择该应用程序并单击**创建**。在页面中填入关于应用程序的信息(您可以随意命名)并保存。然后您需要转到**凭证**选项卡(您可能会被自动引导到那里),并为您的 web 应用程序创建一个 **OAuth 客户端 ID** 。作为其中的一部分，你应该注册一个**授权的重定向 URL** 。这是您的应用程序中的 URL，将在**步骤 3** 中调用。

当您完成此步骤时，您将从您创建的凭证中获得一个**客户端 ID** 和**客户端秘密**。您还将为步骤 3 选择并注册**重定向 _URI** 。您将在下面的步骤中使用它们。

# 步骤 1 —提供登录 URL

当您的服务器收到不包含有效会话 cookie 的请求时，它会回复一个包含指向特定登录地址的链接的页面，或者回复一个指向相同地址的重定向响应。这一步的任务是创建地址。

下面是地址，显示时分成几行，但实际应用程序都在一行上:

```
[https://accounts.google.com/signin/oauth?](https://accounts.google.com/signin/oauth?)
response_type=code&
client_id=CLIENT_ID&
scope=openid%20email&
redirect_uri=REDIRECT_URI&
state=STATE
```

大写字母的单词都需要替换为正确的值。步骤 0 为**客户端 ID** 和**重定向 URI** 提供了这些值。**状态**的值几乎可以是你想要的任何字符串——应用程序在**步骤 3** 中指向的 URL 将包括它，所以这是一种将信息从这一步传递到那一步的方式。我通常把最初请求的页面的路径放在这里。

`response_type=code`参数要求最终的重定向请求包含一个代码，该代码可用于检索有关登录结果的信息。`scope=open_id%20email`表示应用程序想要的信息是登录用户的电子邮件地址。

# 步骤 2 —验证用户

这是谷歌的问题，不是你的问题。但是谷歌会检查一下:

*   要求这样做的应用程序已经在 Google 上注册了(客户端标识**)**
*   将用户送回的位置(**重定向 _URI** )是为该应用程序注册的

谷歌还会告诉用户你注册应用程序时给它起的名字，以及一个应用程序隐私政策的链接，如果有的话。最终，如果用户同意并通过验证，Google 将向用户的浏览器发送一个重定向响应，然后浏览器将向您的服务器发出请求，开始**步骤 3** 。

# 步骤 3 —检索用户信息

当用户的浏览器向谷歌响应指定的 **REDIRECT_URI** 发出请求时，这一步开始。该请求将包括查询参数，并且具有以下形式:

```
REDIRECT_URI?state=STATE&code=CODE.
```

你的应用程序的服务器将使用这些查询参数值(**状态**和**代码**)来获取用户的身份信息。**状态**的值就是服务器在**步骤 1** 中发回的值。这有利于保持从这一步到下一步的连续性，因为就服务器而言，这个请求可能来自当前正在进行身份验证的任何用户。当被告知用户需要首先进行身份验证时，服务器没有其他方法可以知道用户正在尝试访问应用程序中的哪个页面。

**代码**值本身不包含任何用户信息，但是服务器可以用它来获取这些信息。为此，服务器(用户的浏览器)将向`[https://oauth2.googleapis.com/token](https://oauth2.googleapis.com/token)`发出 HTTP POST 请求，所需的信息将作为响应的主体返回。POST 请求的主体应该具有内容类型**application/x-www-form-urlencoded**，并包括以下值(与前面一样，为了清楚起见，这被分成多行，但是请求主体应该都在一行中):

```
code=CODE&
client_id=CLIENT_ID&
client_secret=CLIENT_SECRET&
redirect_uri=REDIRECT_URI&
grant_type=authorization_code
```

**代码**是从用户浏览器发送的查询参数中提取的值。**客户端 ID** 、**客户端秘密**和**重定向 URI** 是来自步骤 0 的值。并且`grant_type=authorization_code`告诉谷歌你的应用服务器正在提供什么样的代码。

对这个 POST 请求的响应将是一个包含用户信息的**应用程序/json** 主体。这个 JSON 对象中的一个字段叫做 **id_token** ，它的值是一个 [JSON Web Token](https://en.wikipedia.org/wiki/JSON_Web_Token) (JWT)。这是一段经过数字签名的数据，其中包含有关用户的字段，包括:电子邮件地址、发布者(在本例中为`[https://accounts.google.com](https://accounts.google.com)`)、受众(即该令牌的目标接收者，即您的应用程序)和有效期。您可以自己完成所有的加密和解析工作，但是 Python 库`[google.oauth2.id_token](https://google-auth.readthedocs.io/en/latest/reference/google.oauth2.id_token.html)`可以处理这些工作，包括验证数字签名是否有效以及是否来自 Google。

一旦您的服务器有了这些信息，它就可以将用户的信息保存在一个会话对象中，并设置一个指向该会话的 cookie。身份验证工作已经完成。

# 信息流

你的应用服务器和谷歌登录服务之间的信息以两种方式流动:间接通过用户的浏览器，以及直接从你的应用服务器到谷歌。间接信息是在浏览器请求的 URL 的查询参数中提供的，要么是对你的应用程序将浏览器发送给谷歌的响应，要么是谷歌将浏览器定向回来。这些查询参数包括**客户端 ID** 、**重定向 _URI** 、**状态**和**代码**。

敏感信息不能通过这种方式(通过浏览器)传递，因为它可能会被截取并在预期的上下文之外使用。这就是为什么谷歌只是将一个不透明的代码发送回你的应用，而不是用户的电子邮件地址。敏感信息必须在你的应用服务器和谷歌服务之间直接安全地传递。这是通过**步骤 3** 中的 POST 请求和响应实现的。该连接返回用户的身份，谷歌只愿意在用户批准后提供给你的注册应用，并传递 **CLIENT_SECRET** ，你的服务器用它向谷歌证明自己的身份。

# 示例应用程序

我在 GitHub 上分享了一个示例应用程序的[源代码，展示了所有这些如何在一个极小的 web 应用程序中工作。你可以为谷歌云平台注册一个项目，然后运行这个代码来尝试。我为 Google App Engine 写了代码，但是它应该可以在任何地方运行。它可以在云上运行或计算引擎，或者不同的云提供商，或者您自己的数据中心，甚至您自己的测试桌面。暂时可以](https://github.com/engelke/google-signin-demo)[试试这里](https://engelke-signin-demo.uc.r.appspot.com)。

*原载于 2020 年 7 月 14 日*[*http://engelke . dev*](https://engelke.dev/2020/07/14/authenticating-users-with-google-sign-in/)*。*
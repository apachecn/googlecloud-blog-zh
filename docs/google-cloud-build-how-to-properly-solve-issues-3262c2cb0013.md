# 谷歌云构建如何妥善解决问题

> 原文：<https://medium.com/google-cloud/google-cloud-build-how-to-properly-solve-issues-3262c2cb0013?source=collection_archive---------1----------------------->

几周前，我开始使用云构建来建立一个自动部署。我已经使用 gcloud app deploy 从本地机器部署了应用程序，我认为是时候建立一个适当的部署和发布管道了。

当我使用云构建开始我的第一次部署时，我得到了以下错误:

```
ERROR: gcloud crashed (UnicodeDecodeError): 'ascii' codec can't decode byte 0xe2 in position 13: ordinal not in range(128)
```
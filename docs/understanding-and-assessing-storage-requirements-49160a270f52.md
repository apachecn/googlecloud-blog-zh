# 了解和评估存储要求

> 原文：<https://medium.com/google-cloud/understanding-and-assessing-storage-requirements-49160a270f52?source=collection_archive---------6----------------------->

*存储是通过计算技术将数字数据保存在数据存储设备中的过程。存储可以根据许多参数进行分类，因此有许多类型的存储！*

浏览存储环境可能会很棘手，尤其是如果业务存储需求最近发生了变化，并且您不确定下一步该做什么。

遗留数据平台已经构建了许多年，非常多样，理解起来也非常复杂。因此，了解存储的基本特征对于评估增强和转变数据存储和管理方式的道路上的存储需求变得十分必要。

# 要考虑的关键标准

*   **数据格式** —数据目前是什么格式，迁移后应该是什么格式(结构化/半结构化/非结构化)？
*   **功能** —数据的用途是什么(缓存/交易/分析等)？
*   **容量** —您需要存储多少数据？
*   **可扩展性** —从现在起 5 年后，您需要存储多少数据？
*   **性能** —您希望的吞吐量、IOPS &延迟是多少？
*   **备份和恢复** —您将在哪里备份文件，多久备份一次？
*   预算——你要花多少钱？

# 存储系统与数据库

## 存储/文件系统

用于存储任意的、可能不相关的数据和数据库的非结构化数据存储建立在文件系统提供的一般数据存储服务之上。

**文件系统更好，如果:**

*   您喜欢对数据使用版本控制(DBs 的噩梦)
*   您有大量频繁增长的数据(通常是日志文件)
*   您希望其他应用程序在没有 API 的情况下访问您的数据(如文本编辑器)
*   你想存储大量的二进制内容(图片或 MP3)

## 数据库ˌ资料库

通常用于存储相关的结构化数据，具有良好定义的数据格式，以高效的方式进行插入、更新和/或检索(取决于应用)。

**DB 表在以下情况下更好:**

*   您希望存储许多结构完全相同的行(没有块浪费)
*   您需要按多个值(索引表)快速查找/排序
*   你需要原子事务(数据安全)
*   您的用户将一直读/写相同的数据(更好的锁定)

# 存储访问类型

最好根据应用程序访问存储的方式来定制和优化应用程序的数据迁移方法。典型的存储输入/输出(I/O)访问类型解释如下:

## 文件存储器

用于以文件和文件夹的形式组织和存储数据的分层存储方法。

**协议** : NFS、CIFS 或中小企业

**优势**:简化共享文件的访问和管理，全局文件锁定

**限制**:元数据的固定文件系统属性，有限的可扩展性

**用例**:分层文件系统，在多个用户之间共享数据

## 块存储器

固定大小的块将部分数据存储在分层系统中，并在需要时重新组装。

**协议**:光纤通道，ISCI，FCoE

**优势**:最低延迟，一致的性能

**限制**:昂贵，无元数据功能

**用例**:数据库的结构化数据、事务性、底层架构

## 对象存储

称为对象的唯一可识别和独特的单元将数据存储在平面文件系统中。

**协议** : REST 和 SOAP over HTTP

**优势**:经济高效，能够处理丰富的元数据，大规模分析，高度可扩展

**限制**:不适合频繁变化的数据，因为整个对象必须重写

**用例**:静态非结构化数据、大量读取数据、富媒体文件、备份文件

# 存储设备配置类型

为了存储任何形式的数据，用户都需要存储设备。数据存储设备主要分为以下几类:

## 分布式天线系统

直接区域存储，也称为直接连接存储(DAS ),指的是存储设备通常位于直接区域，并直接连接到通过通用接口(如 SATA、PCIe、USB 或 Thunderbolt)访问该区域的计算机。

**优点:**设置简单，成本低，性能高

**局限性:**可访问性有限，可扩展性有限，没有集中管理和备份

**用例:**预算限制，一个简单的存储解决方案，适用于只需要本地共享数据的小型企业。

## 美国国家科学院（National Academy of Sciences）

NAS(网络连接存储)解决方案通常部署为通过局域网(LAN)连接的文件级数据存储设备，为网络上的一组客户端提供数据访问。

**优势:**高可扩展性，更大的可访问性和更高的性能

**限制:**增加局域网流量、性能限制、安全性和可靠性

**使用案例:**文件存储和共享、大数据、中小型企业和需要最少维护、可靠且灵活的存储系统的组织。

## 存储区域网

存储区域网络(SAN)是专用的高性能存储系统，在服务器和存储设备之间传输块级数据。

**优势:**改进的性能、更大的可伸缩性、改进的可用性和弹性。

**局限性:**昂贵，设置和维护更复杂

**使用案例:**数据库管理系统，虚拟化，用于数据中心或大型企业组织的关键任务文件或应用程序。

## 最新进展

## 统一 SAN

像任何存储技术一样，San 正在经历一场转变。例如，供应商现在提供一种称为统一 SAN 的产品，它可以在单个解决方案中同时支持块级和文件级存储。

## vSAN

其他技术也在出现，以弥补 NAS 和 SAN 之间的差距。一个例子是 VMware vSphere，它使在同一个群集中使用 NAS 和 SAN 存储成为可能，因为 vSAN 是 VMware 的虚拟 SAN 技术。

## 云

尽管 NAS 和 SAN 系统已成为大多数企业应用程序的支柱，但许多组织现在正转向云来满足其存储需求。许多优点中的一些是:

*   云供应商通过按需购买订购模式提供按需存储服务，有助于避免过度配置和大量前期成本。
*   云平台高度可扩展，易于管理，提供计量资源，并包含内置冗余。

**谷歌云**在存储和数据库服务以及灵活的解决方案方面提供了业界最佳选择，帮助您将数据迁移到云，同时按照自己的节奏进行现代化和创新！

**敬请关注更多关于存储迁移到 GCP 的后续报道！**
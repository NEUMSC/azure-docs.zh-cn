---
title: "使用 Azure Log Analytics 跟踪更改 | Microsoft 文档"
description: "Log Analytics 中的更改跟踪解决方案可帮助用户确定在环境中发生的软件和 Windows 服务更改。"
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f8040d5d-3c89-4f0c-8520-751c00251cb7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: becb179da6bc6b6df629a07d3ddb5d50edbaa577
ms.lasthandoff: 03/14/2017


---
# <a name="track-software-changes-in-your-environment-with-the-change-tracking-solution"></a>使用更改跟踪解决方案跟踪环境中的软件更改

本文可帮助你使用 Log Analytics 中的更改跟踪解决方案，以便轻松标识环境中的更改。 该解决方案会跟踪对 Windows 和 Linux 软件、Windows 文件、Windows 服务和 Linux 守护程序的更改。 标识配置更改可帮助你查明操作问题。

安装该解决方案，以更新已安装代理的类型。 系统会读取对受监视服务器上的已安装软件、Windows 服务和 Linux 守护程序的更改，然后将数据发送到云中的 Log Analytics 服务进行处理。 逻辑应用于接收的数据，云服务记录数据。 通过使用“更改跟踪”仪表板上的信息，你可以轻松看到服务器基础结构中所做的更改。

## <a name="installing-and-configuring-the-solution"></a>安装和配置解决方案
使用以下信息安装和配置解决方案。

* 在想要监视更改的每台计算机上，都必须装有 [Windows](log-analytics-windows-agents.md)、[Operations Manager](log-analytics-om-agents.md) 或 [Linux](log-analytics-linux-agents.md) 代理。
* 从 [Azure 应用商店](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ChangeTrackingOMS?tab=Overview)或使用[从解决方案库中添加 Log Analytics 解决方案](log-analytics-add-solutions.md)中所述的过程，将更改跟踪解决方案添加到 OMS 工作区。  无需进一步的配置。

### <a name="configure-windows-files-to-track"></a>配置要跟踪的 Windows 文件
使用以下步骤，配置要在 Windows 计算机上跟踪的文件。

1. 在 OMS 门户中，单击“设置”（齿轮符号）。
2. 在“设置”页上，依次单击“数据”和“Windows 文件跟踪”。
3. 在“Windows 文件更改跟踪”下，键入整个路径（包括要跟踪的文件的文件名），然后单击“添加”符号。 例如：C:\Program Files (x86)\Internet Explorer\iexplore.exe 或 C:\Windows\System32\drivers\etc\hosts。
4. 单击“保存” 。  
   ![Windows 文件更改跟踪](./media/log-analytics-change-tracking/windows-file-change-tracking.png)

### <a name="limitations"></a>限制
更改跟踪解决方案当前不支持以下项：

* 文件夹（目录）
* 递归
* 通配符
* 路径变量
* 网络文件系统

其他限制：

* “最大文件大小”列和值在当前实现中未使用。
* 如果在 30 分钟收集周期内收集 2500 多个文件，则解决方案性能可能会下降。
* 当网络流量较高时，更改记录可能需要最多六个小时才能显示。
* 如果在计算机关闭的情况下修改配置，计算机可能会发布属于以前配置的文件更改。

## <a name="change-tracking-data-collection-details"></a>“更改跟踪”数据收集详细信息
“更改跟踪”使用已启用的代理来收集软件清单和 Windows 服务元数据。

下表显示“更改跟踪”的数据收集方法和其他数据收集方式的详细信息。

| 平台 | 直接代理 | SCOM 代理 | Linux 代理 | Azure 存储空间 | 是否需要 SCOM？ | 通过管理组发送的 SCOM 代理数据 | 收集频率 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Windows 和 Linux |![是](./media/log-analytics-change-tracking/oms-bullet-green.png) |![是](./media/log-analytics-change-tracking/oms-bullet-green.png) |![是](./media/log-analytics-change-tracking/oms-bullet-green.png) |![否](./media/log-analytics-change-tracking/oms-bullet-red.png) |![否](./media/log-analytics-change-tracking/oms-bullet-red.png) |![是](./media/log-analytics-change-tracking/oms-bullet-green.png) | 耗时 5 分钟到 50 分钟，具体取决于更改类型。 有关详细信息，请参阅下文。 |


下表显示了各种更改类型的数据收集频率。

| **更改类型** | **频率** | **代理在****发现****差异时是否发送差异？** |
| --- | --- | --- |
| Windows 注册表 | 50 分钟 | 否 |
| Windows 文件 | 30 分钟 | 是的。 如果 24 小时内没有更改，则会发送快照。 |
| Linux 文件 | 15 分钟 | 是的。 如果 24 小时内没有更改，则会发送快照。 |
| Windows 服务 | 30 分钟 | 是的，当发现更改时，每 30 分钟发送一次。 每 24 小时发送一次快照，无论是否有更改。 因此，即使没有更改，也会发送快照。 |
| Linux 守护程序 | 5 分钟 | 是的。 如果 24 小时内没有更改，则会发送快照。 |
| Windows 软件 | 30 分钟 | 是的，当发现更改时，每 30 分钟发送一次。 每 24 小时发送一次快照，无论是否有更改。 因此，即使没有更改，也会发送快照。 |
| Linux 软件 | 5 分钟 | 是的。 如果 24 小时内没有更改，则会发送快照。 |


## <a name="use-change-tracking"></a>使用“更改跟踪”
安装解决方案后，你可以通过使用 OMS 中“**概述**”页上的“**更改跟踪**”磁贴，查看受监视服务器的更改摘要。

![“更改跟踪”磁贴的图像](./media/log-analytics-change-tracking/change-tracking-tile.png)

你可以查看基础结构的更改，然后向下钻取以下类别的详细信息：

* 软件和 Windows 服务的配置类型所做的更改
* 应用程序的软件更改和单个服务器的更新
* 每个应用程序的软件更改总数
* Linux 包
* 单个服务器的 Windows 服务更改
* Linux 守护程序更改

![“更改跟踪”仪表板的图像](./media/log-analytics-change-tracking/change-tracking-dash01.png)

![“更改跟踪”仪表板的图像](./media/log-analytics-change-tracking/change-tracking-dash02.png)

### <a name="to-view-changes-for-any-change-type"></a>查看任何更改类型的更改
1. 在“**概述**”页上单击“**更改跟踪**”磁贴。
2. 在“**更改跟踪**”仪表板上，查看其中一个更改类型边栏选项卡中的摘要信息，然后在“**日志搜索**”页中单击一条信息，以查看其详细信息。
3. 在任何日志搜索页上，都可以按时间、详细结果和日志搜索历史记录查看结果。 还可以按方面进行筛选以缩减搜索结果。

## <a name="next-steps"></a>后续步骤
* 使用 [Log Analytics 中的日志搜索](log-analytics-log-searches.md)可查看详细的更改跟踪数据。


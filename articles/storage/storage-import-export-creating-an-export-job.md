---
title: "为 Azure 导入/导出创建导出作业 | Microsoft Docs"
description: "了解如何为 Microsoft Azure 导入/导出服务创建导出作业。"
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 613d480b-a8ef-4b28-8f54-54174d59b3f4
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
translationtype: Human Translation
ms.sourcegitcommit: 432752c895fca3721e78fb6eb17b5a3e5c4ca495
ms.openlocfilehash: 3440d7e80e6a806c4119dff687fb9c590b8f0d14
ms.lasthandoff: 03/30/2017


---

# <a name="creating-an-export-job-for-the-azure-importexport-service"></a>为 Azure 导入/导出服务创建导出作业
使用 REST API 为 Microsoft Azure 导入/导出服务创建导出作业的过程包括以下步骤：

-   选择要导出的 Blob。

-   获取寄送位置。

-   创建导出作业。

-   通过支持的承运人将空驱动器寄送到 Microsoft。

-   使用包裹信息更新导出作业。

-   接收从 Microsoft 寄回的驱动器。

 有关导入/导出服务的概述以及演示如何使用 [Azure 门户](https://portal.azure.com/)创建和管理导入和导出作业的教程，请参阅[使用 Windows Azure 导入/导出服务将数据传输到 Blob 存储](storage-import-export-service.md)。

## <a name="selecting-blobs-to-export"></a>选择要导出的 Blob
 若要创建导出作业，需提供要从存储帐户导出的 Blob 的列表。 可通过多种方法选择要导出的 Blob：

-   可以使用相对 Blob 路径选择单个 Blob 及其所有快照。

-   可以使用相对 Blob 路径选择单个 Blob（不包括其快照）。

-   可以使用相对 Blob 路径和快照时间选择单个快照。

-   可以使用 Blob 前缀选择具有给定前缀的所有 Blob 和快照。

-   可以导出存储帐户中的所有 Blob 和快照。

 有关指定要导出的 Blob 的详细信息，请参阅[放置作业](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate)操作。

## <a name="obtaining-your-shipping-location"></a>获取寄送位置
在创建导出作业之前，需要通过调用[获取位置](https://portal.azure.com)或[列出位置](/rest/api/storageimportexport/listlocations)操作获取寄送位置名称和地址。 `List Locations` 返回位置及其邮寄地址的列表。 可以从返回的列表中选择一个位置，然后将硬盘驱动器寄送到该地址。 也可以使用“`Get Location`”操作直接获取特定位置的寄送地址。

遵循以下步骤获取寄送位置：

-   指定存储帐户位置的名称。 可在经典门户中存储帐户的“仪表板”上的“位置”字段下找到该值，或者使用服务管理 API 操作[获取存储帐户属性](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties)来查询该值。

-   通过调用“`Get Location`”操作来检索可用于处理此存储帐户的位置。

-   如果位置的 `AlternateLocations` 属性包含位置本身，则可以使用此位置。 否则，请使用某个备用位置再次调用“`Get Location`”操作。 原始位置可能会出于维护目的而暂时关闭。

## <a name="creating-the-export-job"></a>创建导出作业
 若要创建导出作业，请调用[放置作业](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate)操作。 需要提供以下信息：

-   作业的名称。

-   存储帐户名称。

-   上一步骤中获取的寄送位置名称。

-   作业类型（导出）。

-   应在导出作业完成后将驱动器寄送到的回邮地址。

-   要导出的 Blob（或 Blob 前缀）的列表。

## <a name="shipping-your-drives"></a>寄送驱动器
 接下来，根据选择的要导出的 Blob 和驱动器大小，使用 Azure 导入/导出工具确定需寄送的驱动器数目。 有关详细信息，请参阅 [Azure 导入/导出工具参考](storage-import-export-tool-how-to-v1.md)。

 将驱动器打包到一个包裹中，然后将其寄送到在上一步骤中获取的地址。 记下包裹的跟踪号供下一步使用。

> [!NOTE]
>  必须通过支持的、可提供包裹跟踪号的承运人寄送驱动器。

## <a name="updating-the-export-job-with-your-package-information"></a>使用包裹信息更新导出作业
 获取跟踪号后，请调用[更新作业属性](/rest/api/storageimportexport/jobs#Jobs_Update)操作更新作业的承运人名称和跟踪号。 可以选择性地指定驱动器数量、回邮地址和寄送日期。

## <a name="receiving-the-package"></a>接收包裹
 处理导出作业后，驱动器将连同加密的数据一起回邮给你。 可以通过调用[获取作业](/rest/api/storageimportexport/jobs#Jobs_Get)操作检索每个驱动器的 BitLocker 密钥。 然后，可以使用该密钥解锁驱动器。 每个驱动器上的驱动器清单文件包含驱动器上的文件列表以及每个文件的原始 Blob 地址。

## <a name="next-steps"></a>后续步骤

* [使用导入/导出服务 REST API](storage-import-export-using-the-rest-api.md)


---
title: "使用指标监视 Azure IoT 中心 | Microsoft Docs"
description: "如何使用 Azure IoT 中心度量值评估和监视 IoT 中心的总体运行状况。"
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: a47108fd-f994-4105-b21d-5b8f697b699c
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/22/2017
ms.author: nberdy
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 37b2a82d7f6043224e68219fde753eef73078ffd
ms.openlocfilehash: a736be397e07a1ea98819f6e016df8c0c7c637eb
ms.lasthandoff: 03/02/2017


---
# <a name="understand-iot-hub-metrics"></a>了解 IoT 中心指标
IoT 中心度量值提供更棒的数据，清晰显示 Azure 订阅中的 Azure IoT 资源状态。 通过 IoT 中心度量值，可评估 IoT 中心服务及其所连接的设备的总体运行状况。 面向用户的统计信息非常重要，因为它们可以帮助了解 IoT 中心的情况，并可以帮助在不联系 Azure 支持人员的情况下解决根本问题。

默认启用度量值。 可在 Azure 门户中查看 IoT 中心度量值。

## <a name="how-to-view-iot-hub-metrics"></a>如何查看 IoT 中心度量值
1. 创建 IoT 中心。 有关如何创建 IoT 中心的说明，请参阅[入门][lnk-get-started]指南。
2. 打开 IoT 中心的边栏选项卡。 在此处单击“指标”。
   
    ![][1]
3. 在“度量值”边栏选项卡中，可查看 IoT 中心的度量值并创建度量值的自定义视图。 单击“诊断设置”，即可选择将指标数据发送到事件中心终结点或 Azure 存储帐户。
   
    ![][2]

## <a name="iot-hub-metrics-and-how-to-use-them"></a>IoT 中心度量值及其用法
IoT 中心提供了多个度量值，使你可以大致了解中心的运行状况以及已连接的设备总数。 可以结合多个度量值的信息，更清楚地了解 IoT 中心的状态。 下表描述了每个 IoT 中心所跟踪的度量值，以及每个度量值与 IoT 中心总体状态的关联。

|度量值|指标显示名称|计价单位|聚合类型|说明|
|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|遥测消息发送尝试次数|计数|总计|尝试发送到 IoT 中心的、设备到云的遥测消息数|
|d2c.telemetry.ingress.success|已发送的遥测消息数|计数|总计|成功发送到 IoT 中心的、设备到云的遥测消息数|
|c2d.commands.egress.complete.success|完成的命令数|计数|总计|设备已成功完成的云到设备命令的数目|
|c2d.commands.egress.abandon.success|放弃的命令数|计数|总计|设备放弃的云到设备命令的数目|
|c2d.commands.egress.reject.success|拒绝的命令数|计数|总计|设备拒绝的云到设备命令的数目|
|devices.totalDevices|设备总数|计数|总计|已注册到 IoT 中心的设备数目|
|devices.connectedDevices.allProtocol|已连接的设备|计数|总计|已连接到 IoT 中心的设备数目|
|d2c.telemetry.egress.success|发送的遥测消息数|计数|总计|已成功将消息写入到终结点的次数（总数）|
|d2c.telemetry.egress.dropped|丢弃的消息数|计数|总计|因为与任何路由都不匹配并且回退路由被禁用而被丢弃的消息的数目|
|d2c.telemetry.egress.orphaned|孤立的消息数|计数|总计|不匹配任何路由（包括回退路由）的消息计数|
|d2c.telemetry.egress.invalid|无效的消息数|计数|总计|由于与终结点不兼容而未传递的消息计数|
|d2c.telemetry.egress.fallback|符合回退条件的消息数|计数|总计|已写入到回退终结点的消息数|
|d2c.endpoints.egress.eventHubs|已传递到事件中心终结点的消息数|计数|总计|已成功将消息写入到事件中心终结点的次数|
|d2c.endpoints.latency.eventHubs|事件中心终结点的消息延迟|毫秒|平均值|消息进入 IoT 中心与进入事件中心终结点之间的平均延迟（毫秒）|
|d2c.endpoints.egress.serviceBusQueues|已传递到服务总线队列终结点的消息数|计数|总计|已成功将消息写入到服务总线队列终结点的次数|
|d2c.endpoints.latency.serviceBusQueues|服务总线队列终结点的消息延迟|毫秒|平均值|消息进入 IoT 中心与进入服务总线队列终结点之间的平均延迟（毫秒）|
|d2c.endpoints.egress.serviceBusTopics|已传递到服务总线主题终结点的消息数|计数|总计|已成功将消息写入到服务总线主题终结点的次数|
|d2c.endpoints.latency.serviceBusTopics|服务总线主题终结点的消息延迟|毫秒|平均值|消息进入 IoT 中心与进入服务总线主题终结点之间的平均延迟（毫秒）|
|d2c.endpoints.egress.builtIn.events|已传递到内置终结点的消息数（消息/事件）|计数|总计|已成功将消息写入到内置终结点的次数（消息/事件）|
|d2c.endpoints.latency.builtIn.events|内置终结点的消息延迟（消息/事件）|毫秒|平均值|消息进入 IoT 中心与进入内置终结点（消息/事件）之间的平均延迟（毫秒） |
|d2c.twin.read.success|设备的成功克隆读取数|计数|总计|由设备发起的所有成功的克隆读取的计数。|
|d2c.twin.read.failure|设备的失败克隆读取数|计数|总计|由设备发起的所有失败的克隆读取的计数。|
|d2c.twin.read.size|设备的克隆读取的响应大小|字节|平均值|由设备发起的所有成功的克隆读取的平均大小、最小大小和最大大小。|
|d2c.twin.update.success|设备的成功克隆更新数|计数|总计|由设备发起的所有成功的克隆更新的计数。|
|d2c.twin.update.failure|设备的失败克隆更新数|计数|总计|由设备发起的所有失败的克隆更新的计数。|
|d2c.twin.update.size|设备的克隆更新的大小|字节|平均值|由设备发起的所有成功的克隆更新的平均大小、最小大小和最大大小。|
|c2d.methods.success|成功的直接方法调用数|计数|总计|所有成功的直接方法调用的计数。|
|c2d.methods.failure|失败的直接方法调用数|计数|总计|所有失败的直接方法调用的计数。|
|c2d.methods.requestSize|直接方法调用的请求大小|字节|平均值|所有成功的直接方法请求的平均大小、最小大小和最大大小。|
|c2d.methods.responseSize|直接方法调用的响应大小|字节|平均值|所有成功的直接方法响应的平均大小、最小大小和最大大小。|
|c2d.twin.read.success|后端的成功克隆读取数|计数|总计|由后端发起的所有成功的克隆读取的计数。|
|c2d.twin.read.failure|后端的失败克隆读取数|计数|总计|由后端发起的所有失败的克隆读取的计数。|
|c2d.twin.read.size|后端的克隆读取的响应大小|字节|平均值|由后端发起的所有成功的克隆读取的平均大小、最小大小和最大大小。|
|c2d.twin.update.success|后端的成功克隆更新数|计数|总计|由后端发起的所有成功的克隆更新的计数。|
|c2d.twin.update.failure|后端的失败克隆更新数|计数|总计|由后端发起的所有失败的克隆更新的计数。|
|c2d.twin.update.size|后端的克隆更新的大小|字节|平均值|由后端发起的所有成功的克隆更新的平均大小、最小大小和最大大小。|
|twinQueries.success|成功的克隆查询|计数|总计|所有成功的克隆查询的计数。|
|twinQueries.failure|失败的克隆查询|计数|总计|所有失败的克隆查询的计数。|
|twinQueries.resultSize|克隆查询结果大小|字节|平均值|所有成功的克隆查询的结果大小的平均值、最小值和最大值。|
|jobs.createTwinUpdateJob.success|克隆更新作业创建成功数|计数|总计|克隆更新作业创建成功的所有次数。|
|jobs.createTwinUpdateJob.failure|克隆更新作业创建失败数|计数|总计|克隆更新作业创建失败的所有次数。|
|jobs.createDirectMethodJob.success|方法调用作业的创建成功数|计数|总计|直接方法调用作业创建成功的所有次数。|
|jobs.createDirectMethodJob.failure|方法调用作业的创建失败数|计数|总计|直接方法调用作业创建失败的所有次数。|
|jobs.listJobs.success|对列出作业的成功调用数|计数|总计|对列出作业的所有成功调用的计数。|
|jobs.listJobs.failure|对列出作业的失败调用数|计数|总计|对列出作业的所有失败调用的计数。|
|jobs.cancelJob.success|成功的作业取消数|计数|总计|用来取消作业的调用成功的次数。|
|jobs.cancelJob.failure|失败的作业取消数|计数|总计|用来取消作业的调用失败的次数。|
|jobs.queryJobs.success|成功的作业查询数|计数|总计|对查询作业的所有成功调用的计数。|
|jobs.queryJobs.failure|失败的作业查询数|计数|总计|对查询作业的所有失败调用的计数。|
|jobs.completed|已完成的作业|计数|总计|所有已完成的作业的计数。|
|jobs.failed|失败的作业数|计数|总计|所有失败的作业的计数。|

## <a name="next-steps"></a>后续步骤
现已大致了解了 IoT 中心度量值，请单击此链接，深入了解如何管理 Azure IoT 中心：

* [操作监视][lnk-monitor]

若要进一步探索 IoT 中心的功能，请参阅：

* [IoT 中心开发人员指南][lnk-devguide]
* [使用 IoT 网关 SDK 模拟设备][lnk-gateway]

<!-- Links and images -->
[1]: media/iot-hub-metrics/enable-metrics-1.png
[2]: media/iot-hub-metrics/enable-metrics-2.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-operations-monitoring]: iot-hub-operations-monitoring.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md


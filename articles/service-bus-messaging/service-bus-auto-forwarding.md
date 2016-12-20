---
title: "自动转发服务总线消息传送实体 | Microsoft 文档"
description: "如何将队列或订阅链接到另一个队列或主题。"
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: f7060778-3421-402c-97c7-735dbf6a61e8
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/29/2016
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: a20450442a8471534e4cd3faab9167d1db65d9b3


---
# <a name="chaining-service-bus-entities-with-auto-forwarding"></a>使用自动转发链接服务总线实体
通过自动转发功能可将队列或订阅连接到作为相同命名空间组成部分的另一个队列或主题。 启用自动转发时，服务总线会自动删除放置在第一个队列或订阅（源）中的消息，并将其放入第二个队列或主题（目标）中。 请注意，仍可将消息直接发送到目标实体。 此外，无法将子队列（例如死信队列）连接到另一个队列或主题。

## <a name="using-auto-forwarding"></a>使用自动转发
可通过在源的 [QueueDescription][QueueDescription] 或 [SubscriptionDescription][SubscriptionDescription] 对象上设置 [QueueDescription.ForwardTo][QueueDescription.ForwardTo] 或 [SubscriptionDescription.ForwardTo][SubscriptionDescription.ForwardTo] 属性来启用自动转发，如以下示例所示。

```
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

创建源实体时，目标实体必须存在。 如果目标实体不存在，则当创建源实体时，服务总线将返回异常。

可使用自动转发扩大单个主题。 服务总线将[给定主题的订阅数](service-bus-quotas.md)限制为 2,000。 可通过创建二级主题来容纳其他订阅。 请注意，即使不受服务总线的订阅数限制，添加二级主题也可提高主题的整体吞吐量。

![自动转发方案][0]

自动转发还可用于分离消息发送方与接收方。 例如，考虑一个由以下三个模块组成的 ERP 系统：订单处理、库存管理和客户关系管理。 每个这些模块生成的消息会排入到相应的主题中。 Alice 和 Bob 是两名销售代表，他们想了解与其客户相关的所有消息。 若要接收这些消息，Alice 和 Bob 会各自在每个自动转发所有消息到其队列中的 ERP 主题上创建一个个人队列和帐户。

![自动转发方案][1]

如果 Alice 处于度假期间，则其个人队列（而不是 ERP）会填满。 此方案中，由于销售代表未接收到任何消息，因此没有任何 ERP 主题会达到配额。

## <a name="auto-forwarding-considerations"></a>自动转发注意事项
如果目标实体累积了多条消息并超出配额，或禁用了目标实体，则源实体会将消息添加到其[死信队列](service-bus-dead-letter-queues.md)，直到目标中存在可用空间（或重新启用了该实体）。 这些消息将继续位于死信队列中，因此你必须从死信队列显式接收和处理它们。

将各个主题连接到一起以获取具有多个订阅的复合主题时，推荐第一级别主题上具有中等数量的订阅，第二级别主题上具有多个订阅。 例如，一个第一级别主题包含 20 个订阅，其中每个订阅连接到一个包含 200 个订阅的第二级别主题，则这个第一级别的主题允许的吞吐量高于另一个包含 200 个订阅（其中每个订阅连接到一个包含 20 个订阅的第二级别主题）的第一级别主题。

服务总线对于每条转发的消息收取一个操作的费用。 例如，将一条消息发送到一个包含 20 个订阅（每个订阅配置为将消息自动转发到另一队列或主题）的主题，如果所有第一级别的订阅都接收到此消息的副本，则会作为 21 次操作进行计费。

若要创建连接到另一个队列或主题的订阅，则订阅创建者必须具有源和目标实体的**管理**权限。 将消息发送到源主题仅需要源主题的**发送**权限。

## <a name="next-steps"></a>后续步骤
有关自动转发的详细信息，请参阅以下参考主题：

* [SubscriptionDescription.ForwardTo][SubscriptionDescription.ForwardTo]
* [QueueDescription][QueueDescription]
* [SubscriptionDescription][SubscriptionDescription]

若要了解有关服务总线性能提升的详细信息，请参阅[分区消息传送实体][分区消息传送实体]。

[QueueDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.forwardto.aspx
[SubscriptionDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.forwardto.aspx
[QueueDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx
[SubscriptionDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.aspx
[0]: ./media/service-bus-auto-forwarding/IC628631.gif
[1]: ./media/service-bus-auto-forwarding/IC628632.gif
[分区消息传送实体]: service-bus-partitioning.md



<!--HONumber=Nov16_HO3-->


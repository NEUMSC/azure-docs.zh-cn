---
title: "在 Eclipse 中调试 Azure 的 Java Web 应用 | Microsoft Docs"
description: "本教程演示如何使用用于 Eclipse 的 Azure 工具包调试在 Azure 上运行的 Java Web 应用。"
services: app-service\web
documentationcenter: java
author: selvasingh
manager: erikre
editor: 
ms.assetid: 321d2d19-9ce0-4165-8555-b6b15e671393
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 12/22/2016
ms.author: asirveda;robmcm
translationtype: Human Translation
ms.sourcegitcommit: ff60ebaddd3a7888cee612f387bd0c50799496ac
ms.openlocfilehash: 8a4f22442d030dae602171555bafb3733d61ebd7
ms.lasthandoff: 01/05/2017


---
# <a name="debug-a-java-web-app-on-azure-in-eclipse"></a>在 Eclipse 中调试 Azure 的 Java Web 应用
本教程介绍如何使用[用于 Eclipse 的 Azure 工具包]调试在 Azure 上运行的 Java Web 应用。 为简单起见，本教程将使用基本的 Java Server Page (JSP) 示例，但是在 Azure 中进行调试时对于 Java servlet 步骤是类似的。

完成本教程后，在 Eclipse 中调试你的应用程序时，该应用程序如下图所示：

![][01]

## <a name="prerequisites"></a>先决条件
* Java 开发人员工具包 (JDK) 1.8 或更高版本。
* Eclipse IDE for Java EE Developers, Indigo 或更高版本。 可从 <http://www.eclipse.org/downloads/> 下载。
* 分发基于 Java 的 Web 服务器或应用程序服务器，例如 Apache Tomcat 或 Jetty。
* Azure 订阅，可从 <https://azure.microsoft.com/en-us/free/> 或 <http://azure.microsoft.com/pricing/purchase-options/> 获取。
* Azure Toolkit for Eclipse。 有关详细信息，请参阅[安装用于 Eclipse 的 Azure 工具包]。
* 已创建并部署到 Azure 应用服务的动态 Web 项目；如需示例，请参阅[在 Eclipse 中创建用于 Azure 的 Hello World Web 应用]。

## <a name="to-debug-a-java-web-app-on-azure"></a>调试 Azure 上的 Java Web 应用
若要完成本节中的这些步骤，可以使用现有的已在 Azure 中部署为 Java Web 应用的动态 Web 项目，或者可以下载[动态 Web 项目示例]，然后按照[在 Eclipse 中创建用于 Azure 的 Hello World Web 应用]中的步骤在 Azure 中部署该项目。 

1. 打开 Eclipse。
2. 配置用于远程调试的超时值：
   
   1. 单击 Eclipse 中的“Windows”菜单，然后单击“首选项”。
   2. 展开“Java”节点，然后选择“调试”。
   3. 将“调试器超时(毫秒)”和“启动超时(毫秒)”设置均配置为 `120000`。
      
       ![][02]
   4. 单击“确定”以关闭“首选项”对话框。
3. 在 Eclipse 的项目资源管理器视图中，右键单击已部署到 Azure 的动态 Web 项目。 显示上下文菜单时，选择“调试方式”，然后单击“Azure Web 应用”。
   
    ![][03]
4. 如果是首次调试动态 Web 项目，将打开“调试配置”对话框；可以接受“连接”选项卡上由工具包指定的默认值。 在“源”选项卡中，依次单击“添加”、“Java 项目”，选择“动态 Web 项目”，然后单击“确定”。 完成这些步骤后，单击“调试”。
   
    ![][04]
5. 当出现“是否立即启用远程 Web 应用中的远程调试?”提示时，单击“确定”。
6. 当系统提示“Web 应用现在可以进行远程调试”，请单击“确定”。
   
    ![][05]
7. 再次出现“调试配置”对话框时，请单击“调试”。
8. 将打开 Windows 命令提示符或 Unix shell，并准备调试所需的连接；需等到远程 Java Web 应用连接成功才能继续操作。 如果使用的是 Windows，则如下图所示。
   
    ![][06]
9. 将断点插入 JSP 页面，然后在浏览器中打开 Java Web 应用的 URL：
   
   1. 在 Eclipse 中打开 **Azure 资源管理器**。
   2. 导航到“Web 应用”以及要调试的 Java Web 应用。
   3. 右键单击 Web 应用，然后单击“在浏览器中打开”。
   4. Eclipse 现在将进入调试模式。

## <a name="next-steps"></a>后续步骤
有关将 Azure 与 Java 配合使用的详细信息，请参阅 [Azure Java 开发人员中心]。

有关创建 Azure Web 应用的其他信息，请参阅 [Web 应用概述]。

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[用于 Eclipse 的 Azure 工具包]: ../azure-toolkit-for-eclipse.md
[安装用于 Eclipse 的 Azure 工具包]: ../azure-toolkit-for-eclipse-installation.md
[在 Eclipse 中创建用于 Azure 的 Hello World Web 应用]: ./app-service-web-eclipse-create-hello-world-web-app.md
[动态 Web 项目示例]: http://go.microsoft.com/fwlink/?LinkId=817337

[Azure Java 开发人员中心]: https://azure.microsoft.com/develop/java/
[Web 应用概述]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-debug-java-web-app-in-eclipse/01-debug-java-web-app-in-eclipse.png
[02]: ./media/app-service-web-debug-java-web-app-in-eclipse/02-configure-eclipse-remote-debug.png
[03]: ./media/app-service-web-debug-java-web-app-in-eclipse/03-debug-as.png
[04]: ./media/app-service-web-debug-java-web-app-in-eclipse/04-debug-configurations.png
[05]: ./media/app-service-web-debug-java-web-app-in-eclipse/05-ready-for-remote-debugging.png
[06]: ./media/app-service-web-debug-java-web-app-in-eclipse/06-windows-command-prompt-connection-successful-to-remote.png


---
title: "如何将 io.js 与 Azure App Service Web Apps 配合使用"
description: "了解如何将 Azure App Service 中的 Web 应用与 io.js 配合使用。"
services: app-service\web
documentationcenter: nodejs
author: rmcmurray
manager: erikre
editor: 
ms.assetid: d6320725-ffcb-4ad7-ba63-fc72fa2f2808
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/22/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: 5c14bd2ae2effd554f52662961c2e219433f8cd8


---
# <a name="how-to-use-iojs-with-azure-app-service-web-apps"></a>如何将 io.js 与 Azure App Service Web Apps 配合使用
与 Joyent 的 Node.js 项目相比，流行的 Node 分叉 [io.js] 具有许多不同的特性，包括更加开放的监管模型、更快的发行周期，和更快地采纳新的和试验性的 JavaScript 功能。

虽然 [Azure 应用服务](http://go.microsoft.com/fwlink/?LinkId=529714) Web 应用预装了许多 Node.js 版本，但它还允许使用用户提供的 Node.js 二进制文件。 本文将讨论在应用服务 Web 应用上启用 io.js 的两种方法：使用扩展的部署脚本（自动将 Azure 配置为使用最新的可用 io.js 版本），以及手动上载 io.js 二进制文件。 

<a id="deploymentscript"></a>

## <a name="using-a-deployment-script"></a>使用部署脚本
在部署 Node.js 应用后，应用服务 Web 应用将运行大量的小命令来确保正确配置环境。 如果使用部署脚本，则可以将此过程自定义为包含 io.js 的下载和配置。

GitHub 上提供了 [io.js 部署脚本](https://github.com/felixrieseberg/iojs-azure)。 若要在 Web 应用上启用 io.js，只需将 **.deployment**、**deploy.cmd** 和 **IISNode.yml** 复制到应用程序文件夹的根目录，并将其部署到 Web 应用。  

第一个文件 **.deployment** 指示 Web 应用在部署后要运行 **deploy.cmd**。 此脚本将针对 Node.js 应用程序运行所有常见步骤，但还会下载最新版本的 io.js。 最后，**IISNode.yml** 将 Web 应用配置为使用所下载的 io.js 二进制文件，而不是预装的 Node.js 二进制文件。

> [!NOTE]
> 若要更新使用的 io.js 二进制文件，只需重新部署你的应用程序 - 每次部署应用程序后，脚本将下载 io.js 的新版本。
> 
> 

<a id="manualinstallation"></a>

## <a name="using-manual-installation"></a>使用手动安装
手动安装自定义 io.js 版本只包括两个步骤。 首先，直接从 [io.js 分发包]中下载 **win-x64** 二进制文件。 需要两个文件 - **iojs.exe** 和 **iojs.lib**。 将这两个文件保存到 Web 应用上的某个文件夹中，例如，保存在 **bin/iojs** 中。

若要将 Web Apps 配置为使用 **iojs.exe** 而不是预装的 Node 版本，请在应用程序的根目录中创建一个 **IISNode.yml** 文件，并添加以下行。

    nodeProcessCommandLine: "D:\home\site\wwwroot\bin\iojs\iojs.exe"

<a id="nextsteps"></a>

## <a name="next-steps"></a>后续步骤
在本文中，你已学习了如何使用提供的部署脚本以及手动安装方法，将 io.js 与应用服务 Web 应用配合使用。 

> [!NOTE]
> io.js 正在紧张的开发中，其更新频率超过了 Node.js。 许多 Node.js 模块可能并不适用于 io.js - 若要进行故障排除，请查阅 [GitHub 上的 io.js]。
> 
> 

## <a name="whats-changed"></a>发生的更改
* 有关从网站更改为 App Service 的指南，请参阅 [Azure App Service 及其对现有 Azure 服务的影响](http://go.microsoft.com/fwlink/?LinkId=529714)

> [!NOTE]
> 如果想要在注册 Azure 帐户之前开始使用 Azure App Service，请转到[试用 App Service](https://azure.microsoft.com/try/app-service/)，可以通过该页面在 App Service 中立即创建一个生存期较短的入门 Web 应用。 不需要使用信用卡，也不需要做出承诺。
> 
> 

[io.js]: https://iojs.org
[io.js 分发包]: https://iojs.org/dist/
[GitHub 上的 io.js]: https://github.com/iojs/io.js
[io.js Deployment Script]: https://github.com/felixrieseberg/iojs-azure



<!--HONumber=Feb17_HO3-->



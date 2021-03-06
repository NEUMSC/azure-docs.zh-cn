---
title: "创建 Azure 应用程序网关 - Azure CLI 2.0 | Microsoft 文档"
description: "了解如何在 Resource Manager 中使用 Azure CLI 2.0 创建应用程序网关"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c2f6516e-3805-49ac-826e-776b909a9104
ms.service: application-gateway
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 1481fcb070f383d158c5a6ae32504e498de4a66b
ms.openlocfilehash: 68d3e3ee9b35f2d6d88cde68365cef91d9683462
ms.lasthandoff: 03/01/2017


---
# <a name="create-an-application-gateway-by-using-the-azure-cli-20"></a>使用 Azure CLI 2.0 创建应用程序网关

> [!div class="op_single_selector"]
> * [Azure 门户](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Azure 经典 PowerShell](application-gateway-create-gateway.md)
> * [Azure Resource Manager 模板](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI 1.0](application-gateway-create-gateway-cli.md)
> * [Azure CLI 2.0](application-gateway-create-gateway-cli.md)

Azure 应用程序网关是第&7; 层负载均衡器。 它在不同服务器之间提供故障转移和性能路由 HTTP 请求，而不管它们是在云中还是本地。 应用程序网关具有以下应用程序传递功能：HTTP 负载平衡、基于 Cookie 的会话相关性、安全套接字层 (SSL) 卸载、自定义运行状况探测，以及多站点支持。

## <a name="cli-versions-to-complete-the-task"></a>用于完成任务的 CLI 版本

可以使用以下 CLI 版本之一完成任务：

* [Azure CLI 1.0](application-gateway-create-gateway-cli-nodejs.md) - 适用于经典部署模型和资源管理部署模型的 CLI。
* [Azure CLI 2.0](application-gateway-create-gateway-cli.md) - 适用于资源管理部署模型的下一代 CLI

## <a name="prerequisite-install-the-azure-cli-20"></a>先决条件：安装 Azure CLI 2.0

若要执行本文中的步骤，需要[安装适用于 Mac、Linux 和 Windows 的 Azure 命令行接口 (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2)。

> [!NOTE]
> 如果没有 Azure 帐户，则需要注册一个。 可以[在此处注册免费试用帐户](../active-directory/sign-up-organization.md)。

## <a name="scenario"></a>方案

在此方案中，将学习如何使用 Azure 门户创建应用程序网关。

此方案将：

* 创建包含两个实例的中型应用程序网关。
* 创建名为 AdatumAppGatewayVNET 且包含 10.0.0.0/16 保留 CIDR 块的虚拟网络。
* 创建名为 Appgatewaysubnet 且使用 10.0.0.0/28 作为其 CIDR 块的子网。
* 配置进行 SSL 卸载的证书。

![方案示例][scenario]

> [!NOTE]
> 针对应用程序网关进行的其他配置（包括自定义运行状况探测、后端池地址以及其他规则）是在对应用程序网关配置以后配置的，不是在初始部署期间配置的。

## <a name="before-you-begin"></a>开始之前

Azure 应用程序网关需要自己的子网。 在创建虚拟网络时，请确保保留足够的地址空间，以便设置多个子网。 将应用程序网关部署到子网后，只能向该子网添加其他应用程序网关。

## <a name="log-in-to-azure"></a>登录 Azure

打开 **Microsoft Azure 命令提示符**，然后登录。 

```azurecli
az login -u "username"
```

>[注意] 还可以使用不带开关的 `az login` 进行设备登录，登录时不需要在 aka.ms/devicelogin 输入代码。

键入前述示例后，将提供代码。 在浏览器中导航到 https://aka.ms/devicelogin，继续登录过程。

![显示设备登录信息的 cmd][1]

在浏览器中，输入收到的代码。 将重定向至登录页。

![用于输入代码的浏览器][2]

输入代码后即已登录，关闭浏览器以继续完成方案。

![已成功登录][3]

## <a name="create-the-resource-group"></a>创建资源组

在创建应用程序网关前，会创建资源组以包含应用程序网关。 以下显示该命令。

```azurecli
az resource group create --name myresourcegroup --location "West US"
```

## <a name="create-a-virtual-network-and-subnet"></a>创建虚拟网络和子网

创建资源组后，会为应用程序网关创建虚拟网络。  在以下示例中，地址空间 10.0.0.0/16 定义用于虚拟网络，10.0.0.0/28 用于子网，如前面的方案说明中所示。

```azurecli
az network vnet create \
--name AdatumAppGatewayVNET \
--address-prefix 10.0.0.0/16 \
--subnet-name Appgatewaysubnet \
--subnet-prefix 10.0.0.0/28 \
--resource-group AdatumAppGateway \
--location eastus
```

## <a name="create-the-application-gateway"></a>创建应用程序网关

创建虚拟网络和子网后，即已满足应用程序网关的先决条件。 此外，以下步骤还需要之前导出的 .pfx 证书和证书密码：用于后端的 IP 地址是后端服务器的 IP 地址。 这些值可以是虚拟网络中的专用 IP、公共 IP 或后端服务器的完全限定域名。

```azurecli
az network application-gateway create \
--name AdatumAppGateway \
--location eastus \
--resource-group AdatumAppGatewayRG \
--vnet-name AdatumAppGatewayVNET \
--vnet-address-prefix 10.0.0.0/16 \
--subnet Appgatewaysubnet \
--subnet-address-prefix 10.0.0.0/28 \
--servers 10.0.0.4 10.0.0.5 \
--cert-file /mnt/c/Users/username/Desktop/application-gateway/fabrikam.pfx \
--cert-password P@ssw0rd \
--capacity 2 \
--sku-tier Standard \
--sku-name Standard_Small \
--http-settings-cookie-based-affinity Enabled \
--http-settings-protocol Http \
--frontend-port 443 \
--routing-rule-type Basic \
--http-settings-port 80

```

> [!NOTE]
> 如需在创建过程中能够提供的参数的列表，请运行以下命令：**az network application-gateway create --help**。

此示例会创建基本的应用程序网关，提供的默认设置适用于侦听器、后端池、后端 http 设置以及规则。 它还会配置 SSL 卸载。 预配成功后，即可根据部署修改这些设置。
如果在之前的步骤中已使用后端池定义 Web 应用程序，则在创建后，负载均衡即会开始。

## <a name="next-steps"></a>后续步骤

访问[创建自定义运行状况探测](application-gateway-create-probe-portal.md)，了解如何创建自定义运行状况探测

访问[配置 SSL 卸载](application-gateway-ssl-arm.md)，了解如何配置 SSL 卸载并从 Web 服务器中剥离开销较高的 SSL 解密

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png


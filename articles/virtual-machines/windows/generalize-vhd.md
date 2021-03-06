---
title: "通用化 Windows VM 以便在 Azure 中使用 | Microsoft Docs"
description: "了解如何使用 Sysprep 通用化 Windows VM，使其能够与 Resource Manager 部署模型配合使用。"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: a745d400-c8be-48b4-a891-4a18495ef3fd
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/20/2016
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: b360a6cf0f8eb37099b02ffba8721ab3ec1a5f10
ms.lasthandoff: 03/31/2017


---
# <a name="generalize-a-windows-virtual-machine-using-sysprep"></a>使用 Sysprep 通用化 Windows 虚拟机
本部分说明如何通用化可用作映像的 Windows 虚拟机。 Sysprep 将删除所有个人帐户信息及其他某些数据，并准备好要用作映像的计算机。 有关 Sysprep 的详细信息，请参阅[如何使用 Sysprep：简介](http://technet.microsoft.com/library/bb457073.aspx)。

确保 Sysprep 支持计算机上运行的服务器角色。 有关详细信息，请参阅 [Sysprep 对服务器角色的支持](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> 如果在首次将 VHD 上载到 Azure 之前运行 Sysprep，请确保先[准备好 VM](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)，然后再运行 Sysprep。 
> 
> 

1. 登录到 Windows 虚拟机。
2. 以管理员身份打开“命令提示符”窗口。 将目录切换到 **%windir%\system32\sysprep**，然后运行 `sysprep.exe`。
3. 在“系统准备工具”对话框中，选择“进入系统全新体验(OOBE)”，确保已选中“通用化”复选框。
4. 在“关机选项”中选择“关机”。
5. 单击 **“确定”**。
   
    ![启动 Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. Sysprep 在完成运行后会关闭虚拟机。 

> [!IMPORTANT]
> 将 VHD 上传到 Azure 或从 VM 创建映像完成之前不要重启 VM。 如果 VM 意外重启，请运行 Sysprep 将其再次通用化。
> 
> 

## <a name="next-steps"></a>后续步骤
* 如果 VM 位于本地，则现在可以[将 VHD 上载到 Azure](upload-image.md)。
* 如果 VM 已在 Azure 中，现在可以[从通用 VM 创建映像](capture-image.md)。



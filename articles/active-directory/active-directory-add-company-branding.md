---
title: "向 Azure Active Directory 中的登录页和访问面板页添加公司品牌"
description: "了解如何在 Azure 登录页和访问面板页中添加公司品牌"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: f74621b4-4ef0-4899-8c0e-0c20347a8c31
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/07/2017
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: 6d4fbfe97288fcb76628b45649b8b678152198a9
ms.lasthandoff: 04/07/2017


---
# <a name="add-company-branding-to-sign-in-and-access-panel-pages"></a>向登录页和访问面板页添加公司品牌
许多组织希望在所管理的网站和服务上应用一致的外观。 Azure Active Directory 提供了此功能，允许 IT 专业人员使用自己的公司徽标和图像来自定义以下网页的外观：

* **登录页** - 当你的员工和商业客户登录到 Office 365 或其他使用 Azure AD 的应用程序时，将显示此页。
* **访问面板页** - 访问面板是一个基于 Web 的门户，用于查看和启动 Azure AD 管理员授权你访问的基于云的应用程序。 访问面板可以在以下网站找到：[https://myapps.microsoft.com](https://myapps.microsoft.com)。

本主题介绍如何自定义登录页和访问面板页。

> [!NOTE]
> * 只有在升级到 Azure Active Directory 高级或基本版后，或者在拥有 Office 365 许可证后，才可以使用公司品牌功能。 有关详细信息，请参阅“Azure Active Directory 版本”。
> 
> * 在中国，使用 Azure Active Directory 全球实例的客户可以使用 Azure Active Directory 高级和基本版。 由中国 21Vianet 运营的 Microsoft Azure 服务目前不支持 Azure Active Directory 高级和基本版。 有关详细信息，请在 Azure Active Directory 论坛上与我们联系。


## <a name="customizing-the-sign-in-page"></a>自定义登录页
你的用户通常会在尝试访问你的组织订阅的云应用程序和服务时，与 Azure AD 登录页交互。

在对登录页进行品牌更改的情况下，可能需要长达一小时的时间才会向最终用户显示所做的更改。

当用户访问特定于租户的 URL（例如 https://outlook.com/contoso.com）时，公司品牌元素就会显示在 Azure AD 登录页上。

当用户访问泛型 URL （例如 www.office.com）上的服务时，此时登录页还不会包含公司品牌信息，因为系统不知道用户是谁。 但是，在用户输入其用户 ID 或选择用户磁贴以后，公司品牌就会显示。

> [!NOTE]
> * 在已配置品牌的 Azure 经典门户的“Active Directory” > “目录” > “域”部分中，你的域名必须显示为“活动”。
> * 登录页品牌不会带到个人 Microsoft 帐户的登录页。 如果你的员工或商业客户使用个人 Microsoft 帐户登录，则其登录页不会反映出你组织的品牌。
>

以下屏幕截图说明了登录页是如何自定义的。

### <a name="scenario-1-contoso-employee-goes-to-a-generic-app-url-for-example-wwwofficecom"></a>方案 1：Contoso 员工访问某个泛型应用 URL（例如 www.office.com）

在此示例中，Contoso 用户通过泛型 URL 登录到移动应用程序或 Web 应用程序。 左侧的插图将始终代表该应用，而右侧的交互窗格将会进行更新，显示相应的 Contoso 品牌元素。

![自定义之前和之后的 Office 365 登录页][1]

### <a name="scenario-2-contoso-employee-goes-to-contoso-app-thats-restricted-to-internal-users"></a>方案 2：Contoso 员工访问仅限内部用户使用的 Contoso 应用

在此示例中，Contoso 用户使用特定于公司的 URL 登录到内部应用程序。 左侧插图代表公司品牌（Contoso）。 右侧的交互窗格锁定为 Contoso，方便员工完成登录。

![受限应用登录页][2]

### <a name="scenario-3-contoso-employee-goes-to-a-contoso-app-thats-open-to-external-users"></a>方案 3：Contoso 员工访问对外部用户开放的 Contoso 应用

在此示例中，用户登录到 Contoso 的 LoB 应用程序，但用户可以是 Contoso 员工，也可以不是。 左侧的插图代表资源所有者 (Contoso)，就像上面的示例 \#2 一样。 但这一次，右侧的交互窗格未锁定为 Contoso，表示外部用户也可以登录。

![没有访问限制的登录][3]

### <a name="scenario-4-fabrikam-business-guest-goes-to-contoso-app-thats-open-to-external-users"></a>方案 4：Fabrikam 商业客户访问对外部用户开放的 Contoso 应用

在此示例中，Contoso 用户使用特定于公司的 URL 登录到内部应用程序。 左侧插图代表公司品牌（Contoso）。 右侧的交互窗格锁定为 Contoso，方便员工完成登录。

![以外部用户身份登录][4]


## <a name="what-elements-on-the-page-can-i-customize"></a>我可以在页上自定义哪些元素？

可以在登录页上自定义以下元素：

![][5]

| 页面元素 | 在页面上的位置 |
|:--- | --- |
| 横幅徽标 | 显示在页面右上角。 在确定用户的组织之后（通常是在输入用户名之后）替换应用徽标。 |
| 背景图 | 在登录页左侧显示为全幅图像。 使用租用型登录方案时（当用户访问其自己的组织所发布的应用程序时，或者作为组织的商业客户进行此类访问时），替换应用的背景图。<br>使用低带宽连接时，背景图替换为背景色。 在手机等窄屏上，不会显示背景图。<br>当用户调整其浏览器的大小时，背景图会被裁剪。 在设计背景图时，请将重要的可视元素保留在左上角，使之不被裁剪。 | 
| “使我保持登录状态”复选框 | 显示在“密码”框下面。 |
| 登录页文本 | 将显示在页脚上方的样本文字。 该文本可以用来向用户传达有用的信息，例如技术支持部门的电话号码，或者法律声明。 |

> [!NOTE]
> 所有元素都是可选的。 例如，如果指定了横幅徽标，但没有指定背景图，则登录页将显示徽标和目标站点的插图（例如，Office 365 加利福尼亚州高速公路图像）。
>

在登录页中，“使我保持登录状态”复选框允许用户在关闭浏览器后再重新打开时保持登录状态，不影响会话生存期。

是否显示此复选框取决于“隐藏 KMSI”设置。

![“隐藏 KMSI”设置][6]

若要隐藏复选框，可将此设置配置为“隐藏”。

> [!NOTE]
> SharePoint Online 和 Office 2010 的某些功能取决于用户能否选中此复选框。 如果将此设置配置为“隐藏”，用户可能会看到其他意外的登录提示。
>
>

你还可以本地化此页上的所有元素。 在配置“默认”的一组自定义元素后，便可以针对不同的区域设置配置其他版本。 你还可以混搭各种元素。 例如，你可以：

* 创建适用于所有区域设置的“默认”插图，然后为英文和法文创建特定版本。 当你将浏览器设置为这两种语言之一时，将出现特定的图像，对于其他所有语言，将出现默认插图。
* 为组织配置不同的徽标（例如日语或希伯来语版本）。

## <a name="access-panel-page-customization"></a>“访问面板”页自定义
“访问面板”页实质上是门户页，可让你快速访问管理员授权你访问的云应用。 在此页上，你的应用显示为可单击的应用程序磁贴。

以下屏幕截图显示了自定义之后的访问面板页示例。

![][8]

## <a name="configure-your-directory-with-company-branding"></a>使用公司品牌配置你的目录
可以针对 Azure 经典门户中的每个目录设置一组默认的可自定义元素。 保存默认设置后，管理员可以为不同的语言/区域设置添加每个元素的本地化版本。 所有可自定义元素都是可选的。

例如，如果你配置了默认横幅徽标，但没有配置大图，则登录页将在右上角显示你的徽标。 但是，会显示该站点的默认插图。

假设有以下配置：

* 默认的横幅徽标和英文登录页文本
* 特定于德语的登录页文本

如果你的语言首选项是德语，则会显示默认的横幅标志，但文本是德语。

虽然从技术上讲，你可以为 Azure AD 支持的每种语言都配置不同的设置，但由于维护和性能方面的原因，我们建议你尽量少使用不同的设置。
 
**若要将公司品牌添加到目录，请执行以下步骤：**

1. 以你想要自定义的目录的管理员身份登录到 [Azure 经典门户](https://manage.windowsazure.com) 。
2. 选择要自定义的目录。
3. 在顶部菜单栏中，单击“配置”。
4. 单击“自定义品牌”。
5. 修改要自定义的元素。 所有字段都是可选的。
6. 单击“保存” 。

最长可能需要一个小时才能显示你针对登录页品牌所做的任何新更改。

**若要添加语言特定的公司品牌，请执行以下步骤：**

1. 以你想要自定义的目录的管理员身份登录到 [Azure 经典门户](https://manage.windowsazure.com) 。
2. 选择要自定义的目录。
fs3。 在顶部菜单栏中，单击“配置”。
4. 单击“自定义品牌”。
5. 单击“为特定语言添加品牌”。
6. 选择要为其自定义徽标的语言，然后单击“下一步”。
7. 只编辑要为其配置特定于语言的覆盖的元素。 所有字段都是可选的。 如果将某一字段留空，则将改为显示自定义的默认值（如果未配置自定义的默认值，则显示 Microsoft 默认值）。
8. 单击“保存” 。

**若要从目录中删除公司品牌，请执行以下步骤：**

1. 以你想要自定义的目录的管理员身份登录到 [Azure 经典门户](https://manage.windowsazure.com) 。
2. 选择要自定义的目录。
3. 在顶部菜单栏中，单击“配置”。
4. 单击“自定义品牌”。
5. 在“自定义品牌”页上，选择“编辑现有品牌设置”，然后转到下一页。
6. 根据要删除哪些元素，执行以下一项或多项操作：

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 在“横幅徽标”下，选择“删除上载的徽标”。

    b.保留“数据库类型”设置，即设置为“共享”。 在“磁贴徽标”下，选择“删除上载的徽标”。

    c. 删除所有文本框中的文本。

    d.单击“下一步”。 单击“下一步”。

    e.在“新建 MySQL 数据库”边栏选项卡中，接受法律条款，然后单击“确定”。 删除所有文本框中的文本。
7. 单击“保存”按钮以删除这些元素。
8. 如有必要，请再次单击“自定义品牌”，然后对需要删除的所有特定于语言的品牌重复这些步骤。
    当单击“自定义品牌”并看到未配置任何现有设置的“自定义默认品牌”表单时，所有品牌设置均已删除。


## <a name="customizable-elements"></a>可自定义的元素
公司徽标用于登录页和访问面板页，而其他元素仅用在登录页上。 下表提供了不同的可自定义元素的详细信息。

| Name | 说明 | 约束 | 建议 |
| --- | --- | --- | --- |
| 横幅徽标 |横幅徽标显示在登录页和访问面板上。 |<p>JPG 或 PNG</p><p>60x280 像素</p><p>10 KB</p> |<p>使用你组织的完整徽标（包括象形图和标识）</p><p>使其高度保持在 30 像素以下，以免在移动设备上产生滚动条</p><p>使其大小保持在 4 KB 以下</p><p>使用透明的 PNG（不要想当然地认为“登录”页的背景始终为白色）</p> |
| 磁贴徽标 | 目前不使用 |<p>JPG 或 PNG</p><p>120x120 像素</p><p>10 KB</p> |<p>使其保持简单（没有小文本），因为可能会将此图像的大小调整至 50% |
| </p> | | | |
| 登录用户名标签 | 目前不使用 |<p>Unicode 文本，最多 50 个字符</p><p>仅纯文本（没有链接或 HTML 标记）</p> |<p>使其保持简单扼要</p><p>询问用户他们通常如何引用你为他们提供的工作或学校帐户。</p> |
| 登录页样本文字 |此样本文字显示在登录页表单下方，可用于提供其他说明，或者告知用户到何处寻求帮助和支持。 |<p>Unicode 文本，最多 256 个字符</p><p>仅纯文本（没有链接或 HTML 标记）</p> |使其保持在 250 个字符以下（大约 3 行文本） |
| 登录页背景图 | 当用户访问特定于租户的 URL 时显示在登录页左侧（如果是 RtL 语言，则显示在右侧）的大图。 |<p>JPG 或 PNG</p><p>1420x1200</p><p>500 KB</p> |<p>1420x1200 像素</p><p>重要说明：使其保持尽可能小，最好在 200 KB 以下。 如果此图像太大，则在未缓存此图像时，将影响“登录”页的性能</p><p>为了适应不同的屏幕纵横比，此图像差不多总是需要裁剪。 请将主要的视觉元素保留在左上角。</p> |
| 登录页背景色 | 使用低带宽连接时，此纯色将代替背景图。 | 必须是十六进制格式的 RGB 颜色（示例：\#FFFFFF） | 建议选取横幅徽标的主要颜色。 |

## <a name="next-steps"></a>后续步骤
* [Azure Active Directory 高级版入门](active-directory-get-started-premium.md)
* [查看访问和使用情况报告](active-directory-view-access-usage-reports.md)

<!--Image references-->
[1]: ./media/active-directory-add-company-branding/signin-page_before-customization.png
[2]: ./media/active-directory-add-company-branding/signin-page-restricted-app.png
[3]: ./media/active-directory-add-company-branding/signin-page-open-access.png
[4]: ./media/active-directory-add-company-branding/signin-page-external-guest.png
[5]: ./media/active-directory-add-company-branding/which-elements-can-i-customize.png
[6]: ./media/active-directory-add-company-branding/hide-kmsi.png
[8]: ./media/active-directory-add-company-branding/APBranding.png
[9]: ./media/active-directory-add-company-branding/hidekmsi.png


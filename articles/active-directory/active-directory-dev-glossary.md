<properties
   pageTitle="Azure Active Directory 开发人员词汇表 | Microsoft Azure"
   description="常用的 Azure Active Directory 开发人员概念和功能术语列表。"
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.date="07/31/2016"
   wacn.date=""/>

# Azure Active Directory 开发人员词汇表
本文包含一些核心 Azure Active Directory (AD) 开发人员概念的定义，帮助你了解 Azure AD 的应用程序开发。

## <a name="access-token"></a>访问令牌 
由[授权服务器](#authorization-server)颁发的一种[安全令牌](#security-token)，可供[客户端应用程序](#client-application)用来访问[受保护的资源服务器](#resource-server)。该令牌的形式通常是 [JSON Web 令牌 (JWT)][JWT]，包含[资源所有者](#resource-owner)授予客户端的授权，以便获得所请求的访问级别。该令牌包含所有适用的主体相关[声明](#claim)，可让客户端应用程序以它作为某种形式的凭据来访问给定的资源，并使得资源所有者不必对客户端公开其凭据。

根据提供的凭据，访问令牌有时称为“用户+应用”或“仅限应用”。例如，如果客户端应用程序：

- 使用[“授权代码”授权](#authorization-grant)，则最终用户先以资源所有者的身份进行身份验证，将授权委托给客户端来访问资源。然后，客户端在获取访问令牌时进行身份验证。令牌有时可以更具体地称为“用户+应用”令牌，因为它同时代表授权客户端应用程序的用户，以及应用程序。
- 使用[“客户端凭据”授权](#authorization-grant)，则客户端提供唯一的身份验证，在没有资源所有者身份验证/授权的情况下运行，因此该令牌有时可以称为“仅限应用”令牌。

有关详细信息，请参阅 [Azure AD Token Reference][AAD-Tokens-Claims]（Azure AD 令牌参考）。

## <a name="application-manifest"></a>应用程序清单  
[Azure 经典门户][AZURE-classic-portal]提供的一项功能，可生成以 JSON 表示的应用程序标识配置，作为其关联 [Application][AAD-Graph-App-Entity] 实体和 [ServicePrincipal][AAD-Graph-Sp-Entity] 实体的更新机制。有关详细信息，请参阅 [Understanding the Azure Active Directory application manifest][AAD-App-Manifest]（了解 Azure Active Directory 应用程序清单）。

## <a name="application-object"></a>应用程序对象  
当你在 [Azure 经典门户][AZURE-classic-portal]中注册/更新应用程序时，门户将为该租户同时创建/更新应用程序对象和对应的[服务主体对象](#service-principal-object)。应用程序对象可全局（在其能够访问的所有租户中）*定义*应用程序的标识配置，并可作为模板来*派生*出其对应的服务主体对象，以便在运行时于本地（在特定租户）使用。

有关详细信息，请参阅 [Application and Service Principal Objects][AAD-App-SP-Objects]（应用程序和服务主体对象）。

## <a name="application-registration"></a>应用程序注册  
若要允许某个应用程序与标识和访问管理功能集成并将这些功能委托给 Azure AD，必须向 Azure AD [租户](#tenant)注册该应用程序。向 Azure AD 注册应用程序时，必须提供应用程序的标识配置，以允许它与 Azure AD 集成并使用如下所述的功能：

- 使用 Azure AD 标识管理和 [OpenID Connect][OpenIDConnect] 协议实现进行可靠的单一登录管理
- 通过 Azure AD 的 OAuth 2.0 [授权服务器](#authorization-server)实现，由[客户端应用程序](#client-application)以中转方式访问[受保护资源](#resource-server)
- [同意架构](#consent)根据资源所有者授权来管理客户端对受保护资源的访问。

有关详细信息，请参阅 [Integrating applications with Azure Active Directory][AAD-Integrating-Apps]（将应用程序与 Azure Active Directory 集成）。

## <a name="authorization"></a>身份验证
向访问方质询合法凭据的措施，提供创建用于标识和访问控制的安全主体的基础。例如，在 [OAuth2 授权期间](#authorization-grant)，访问方身份验证根据使用的授权填充[资源所有者](#resource-owner)或[客户端应用程序](#client-application)的角色。

## <a name="authorization"></a>授权
授权经过身份验证的安全主体执行某项操作的措施。在 Azure AD 编程模型中有两个主要用例：

- 在 [OAuth2 授权](#authorization-grant)流程中：[资源所有者](#resource-owner)向[客户端应用程序](#client-application)授权，允许客户端访问资源所有者的资源。
- 在客户端访问资源期间：与[资源服务器](#resource-server)实现的机制一样，使用[访问令牌](#access-token)中提供的[声明](#claim)值作为依据做出访问控制决策。

## <a name="authorization-code"></a>授权代码
在四个 OAuth2 [授权](#authorization-grant)之一的“授权代码”流程中，由[授权终结点](#authorization-endpoint)提供给[客户端应用程序](#client-application)的短期“令牌”。为响应[资源所有者](#resource-owner)的身份验证，将授权代码返回给客户端应用程序，指出资源所有者已将授权委托给客户端应用程序，由应用程序代表它访问资源。在执行流程的过程中，稍后会将授权代码兑换为[访问令牌](#access-token)。

## <a name="authorization-endpoint"></a>授权终结点
[授权服务器](#authorization-server)实现的终结点之一，用来与[资源所有者](#resource-owner)交互，以便在 OAuth2 授权流程期间提供[授权](#authorization-grant)。根据使用的授权流程，实际提供的授权可能不同，这包括[授权代码](#authorization-code)或[安全令牌](#security-token)。

有关详细信息，请参阅 OAuth2 规范的 [authorization grant types][OAuth2-AuthZ-Grant-Types]（授权类型）和 [authorization endpoint][OAuth2-AuthZ-Endpoint]（授权终结点）部分以及 [OpenIDConnect specification][OpenIDConnect-AuthZ-Endpoint]（OpenIDConnect 规范）。

## <a name="authorization-grant"></a>授权授予
授予[客户端应用程序](#client-application)的凭据，代表[资源所有者](#resource-owner)对其受保护资源访问权限的[授权](#authorization)。根据客户端类型/要求，客户端应用程序可以使用 [OAuth2 授权框架定义的四种授权类型][OAuth2-AuthZ-Grant-Types]之一来获取授权：“授权代码授予”、“客户端凭据授予”、“隐式授予”和“资源所有者密码凭据授予”。根据使用的授权流程类型，返回给客户端的凭据是[访问令牌](#access-token)或[授权代码](#authorization-code)（稍后用于交换访问令牌）。

## <a name="authorization-server"></a>授权服务器
根据 [OAuth2 授权框架][OAuth2-Role-Def]的定义，这是在成功验证[资源所有者](#resource-owner)并获取其授权之后，负责向[客户端](#client-application)颁发访问令牌的服务器。[客户端应用程序](#client-application)在运行时根据 OAuth2 定义的[授权授予](#authorization-grant)，通过其[授权](#authorization-endpoint)和[令牌](#token-endpoint)终结点来与授权服务器交互。

对于 Azure AD 应用程序集成，Azure AD 将为 Azure AD 应用程序和 Microsoft 服务 API（例如 [Microsoft 图形 API][Microsoft-Graph]）实现授权服务器角色。

## <a name="claim"></a>声明
[安全令牌](#security-token)包含声明，声明将有关某个实体（例如[客户端应用程序](#client-application)或[资源所有者](#resource-owner)）的断言提供给另一个实体（例如[资源服务器](#resource-server)）。声明是中继令牌主体（例如，由[授权服务器](#authorization-server)进行身份验证的安全主体）相关事实的名称/值对。给定令牌中的声明依赖于几个变量，包括令牌类型、用于验证主体身份的凭据类型和应用程序配置等。

有关详细信息，请参阅 [Azure AD token reference][AAD-Tokens-Claims]（Azure AD 令牌参考）。

## <a name="client-application"></a>客户端应用程序  
根据 [OAuth2 授权框架][OAuth2-Role-Def]的定义，这是代表[资源所有者](#resource-owner)发出受保护资源请求的应用程序。“客户端”一词并不代表任何特定的硬件实现特征（例如，应用程序是在服务器、台式机还是其他设备上执行）。

客户端应用程序向资源所有者请求[授权](#authorization)，以参与 [OAuth2 授权](#authorization-grant)流程，并可代表资源所有者访问 API/数据。OAuth2 授权框架根据客户端是否能够维护其凭据的机密性[定义两种类型的客户端][OAuth2-Client-Types]：“机密”和“公共”。应用程序可实现在 Web 服务器上运行的 [Web 客户端（机密）](#web-client)、安装在设备上的[本机客户端（公共）](#native-client)应用程序，以及在设备浏览器中运行的[基于用户代理的客户端（公共）](#user-agent-based-client)。

## <a name="consent"></a>同意
[资源所有者](#resource-owner)授权给[客户端应用程序](#client-application)，让它获得特定[权限](#permissions)来代表资源所有者访问受保护资源的过程。根据客户端请求的权限，要求管理员或用户同意分别允许其组织/个人数据的访问权限。请注意，在[多租户](#multi-tenant-application)方案中，应用程序的[服务主体](#service-principal-object)也会记录在同意方用户的租户中。

## <a name="ID-token"></a>ID 令牌
[授权服务器](#authorization-server)的[授权终结点](#authorization-endpoint)提供的 [OpenID Connect][OpenIDConnect-ID-Token] [安全令牌](#security-token)，其中包含与最终用户[资源所有者](#resource-owner)的身份验证相关的[声明](#claim)。与访问令牌一样，ID 令牌也以数字签名的 [JSON Web 令牌 (JWT)][JWT] 来表示。不过，与访问令牌不同的是，ID 令牌的声明并不用于与资源访问相关的用途（具体地说，是访问控制）。

有关详细信息，请参阅 [Azure AD token reference][AAD-Tokens-Claims]（Azure AD 令牌参考）。

## <a name="multi-tenant-application"></a>多租户应用程序
在 Azure AD 中注册的[客户端应用程序](#client-application)类别，旨在允许来自任何 Azure AD [租户](#tenant)中所预配的用户帐户的登录和[同意](#consent)，包括客户端最初注册所在的租户以外的租户。相反，注册为单租户的应用程序只允许来自应用程序注册所在相同租户中预配的用户帐户的登录。[本机客户端](#native-client)应用程序默认为多租户，而 [Web 客户端](#web-client)应用程序则可以在单租户和多租户之间做出选择。

有关详细信息，请参阅 [How to sign in any Azure AD user using the multi-tenant application pattern][AAD-Multi-Tenant-Overview]（如何使用多租户应用程序模式将任何 Azure AD 用户登录）。

## <a name="native-client"></a>本机客户端
设备上本机安装的[客户端应用程序](#client-application)类型。由于所有代码都在设备上执行，因此设备因为无法隐私/秘密地存储凭据而被视为“公共”客户端。有关详细信息，请参阅 [OAuth2 client types and profiles][OAuth2-Client-Types]（OAuth2 客户端类型和配置文件）。

## <a name="permissions"></a>权限
[客户端应用程序](#client-application)通过声明权限请求来获取[资源服务器](#resource-server)访问权限。有两种权限类型：

- “委托的”权限，其可根据登录的[资源所有者](#resource-owner)的委托授权请求[基于范围](#scopes)的访问，并在运行时请求清单以作为客户端[访问令牌](#access-token)中的[“scp”声明](#claim)。
- “应用程序”权限，其可根据客户端应用程序的凭据/标识请求[基于角色](#roles)的访问，并在运行时请求清单以作为客户端访问令牌中的[“角色”声明](#claim)。

权限也会在[同意](#consent)过程中出现，让管理员或资源所有者有机会允许/拒绝客户端对其租户中的资源进行访问。

在 [Azure 经典门户][AZURE-classic-portal]的“应用程序”/“配置”选项卡中，在“对其他应用程序的权限”下面根据需要选择“委托的权限”和“应用程序权限”（后者需要全局管理员角色的成员资格），即可配置权限请求。[公共客户端](#client-application)无法维护凭据，因此它只能请求委托的权限，而[机密客户端](#client-application)则可以请求委托的权限和应用程序权限。客户端的[应用程序对象](#application-object)将声明的权限存储在其 [requiredResourceAccess 属性][AAD-Graph-App-Entity]中。

## <a name="resource-owner"></a>资源所有者
根据 [OAuth2 授权框架][OAuth2-Role-Def]的定义，这是能够授予受保护资源访问权限的实体。如果资源所有者是个人，则称为最终用户。例如，当[客户端应用程序](#client-application)想要通过 [Microsoft 图形 API][Microsoft-Graph] 访问用户的邮箱时，需要从邮箱的资源所有者获取权限。

## <a name="resource-server"></a>资源服务器
根据 [OAuth2 授权框架][OAuth2-Role-Def]的定义，这是托管受保护资源的服务器，能够接受并响应出示[访问令牌](#access-token)的[客户端应用程序](#client-application)所发出的受保护资源请求。它也称为受保护的资源服务器或资源应用程序。

资源服务器使用 OAuth 2.0 授权框架公开 API，并通过[范围](#scopes)和[角色](#roles)强制实施其受保护资源的访问权限。示例包括可访问 Azure AD 租户数据的 Azure AD 图形 API，以及可访问邮件、日历和文档等数据的 Office 365 API，这两者也可从 [Microsoft 图形 API][Microsoft-Graph] 来访问。

与客户端应用程序一样，资源应用程序的标识配置是通过 Azure AD 租户中的[注册](#application-registration)来建立的，可提供应用程序和服务主体对象。Microsoft 提供的某些 API（例如 Azure AD 图形 API）在预配期间将预先注册的服务主体设置为在所有租户中可用。

## <a name="roles"></a>角色
与[范围](#scopes)一样，角色提供某种方式让[资源服务器](#resource-server)控制其受保护资源的访问权限。有两种类型的角色：“用户”角色为需要资源访问权限的用户/组实现基于角色的访问控制，“应用程序”角色为需要访问权限的[客户端应用程序](#client-application)实现相同的访问控制。

角色是资源定义的字符串（例如“开支审批人”或“只读”），可在 [Azure 经典门户][AZURE-classic-portal]中通过资源的[应用程序清单](#application-manifest)进行管理，并且存储在资源的 [appRoles 属性][AAD-Graph-Sp-Entity]中。客户端应用程序请求“应用程序”角色的访问[权限](#permissions)，并在运行时于客户端[访问令牌](#access-token)的[“角色”声明](#claim)中向资源出示此权限。Azure AD 管理员可通过 [Azure 经典门户][AZURE-classic-portal]将用户分配到“用户”角色。

有关 Azure AD 图形 API 公开的应用程序角色的详细介绍，请参阅 [Graph API Permission Scopes][AAD-Graph-Perm-Scopes]（图形 API 权限范围）。有关分步实现示例，请参阅 [Role based access control in cloud applications using Azure AD][Duyshant-Role-Blog]（使用 Azure AD 在云应用程序中执行基于角色的访问控制）。

## <a name="scopes"></a>范围
与[角色](#roles)一样，范围提供某种方式让[资源服务器](#resource-server)控制其受保护资源的访问权限。对于已获得资源所有者委托资源访问权限的[客户端应用程序](#client-application)，范围可用于实现[基于范围][OAuth2-Access-Token-Scopes]的访问控制。

范围是资源定义的字符串（例如“Mail.Read”或“Directory.ReadWrite.All”），可在 [Azure 经典门户][AZURE-classic-portal]中通过资源的[应用程序清单](#application-manifest)进行管理，并且存储在资源的 [oauth2Permissions 属性][AAD-Graph-Sp-Entity]中。客户端应用程序请求访问[权限](#permissions)，并在运行时于客户端[访问令牌](#access-token)的[“scp”声明](#claim)中向资源出示此权限。

命名约定最佳实践是使用“resource.operation.constraint”格式。有关 Azure AD 图形 API 公开的范围的详细介绍，请参阅 [Graph API Permission Scopes][AAD-Graph-Perm-Scopes]（图形 API 权限范围）。有关 Office 365 服务公开的范围，请参阅 [Office 365 API permissions reference][O365-Perm-Ref]（Office 365 API 权限参考）。

## <a name="security-token"></a>安全令牌
包含 OAuth2 令牌或 SAML 2.0 断言等声明的已签名文档。在 OAuth 2.0 [授权授予](#authorization-grant)方案中，[访问令牌](#access-token) (OAuth2) 和 [ID令牌](OpenID Connect) 都是安全令牌类型，并且这两种类型都实现为 [JSON Web 令牌 (JWT)][JWT]。

## <a name="service-principal-object"></a>服务主体对象
当你在 [Azure 经典门户][AZURE-classic-portal]中注册/更新应用程序时，门户将为该租户同时创建/更新[应用程序对象](#application-object)和对应的服务主体对象。应用程序对象可全局（在关联的应用程序已获授予访问权限的所有租户中）*定义*应用程序的标识配置，并可作为模板来*派生*出其对应的服务主体对象，以便在运行时于本地（在特定租户）使用。

有关详细信息，请参阅 [Application and Service Principal Objects][AAD-App-SP-Objects]（应用程序和服务主体对象）。

## <a name="sign-in"></a>登录
[客户端应用程序](#client-application)启动最终用户身份验证并在验证之后捕获相关状态，以便获取[安全令牌](#security-token)并将应用程序会话局限在该状态的过程。状态可以包括多种项目，例如用户配置文件信息，以及派生自令牌声明的信息。

应用程序的登录功能通常用于实现单一登录 (SSO)，并可以在此前面加上“注册”功能，作为入口点来让最终用户获取应用程序的访问权限（在首次登录时）。注册功能用于收集和保存用户特定的其他状态，并且可能需要[用户同意](#consent)。

## <a name="sign-out"></a>注销
使最终用户变成未身份验证状态的过程，解除用户在[登录](#sign-in)期间与[客户端应用程序](#client-application)会话关联的状态

## <a name="tenant"></a>租户
Azure AD 目录的实例称为 Azure AD 租户。它提供各种功能，包括集成式应用程序的注册表服务、用户帐户和已注册应用程序的身份验证，以及支持各种协议（包括 OAuth2 和 SAML）所需的 REST 终结点。终结点包括[授权终结点](#authorization-endpoint)、[令牌终结点](#token-endpoint)以及[多租户应用程序](#multi-tenant-application)使用的“通用”终结点。

有关访问租户的各种方式的详细信息，请参阅 [How to get an Azure Active Directory tenant][AAD-How-To-Tenant]（如何获取 Azure Active Directory 租户）。

## <a name="token-endpoint"></a>令牌终结点
[授权服务器](#authorization-server)为了支持 OAuth2 [授权](#authorization-grant)而实现的终结点之一。根据具体的授予，令牌终结点可用于提供[访问令牌](#access-token)（并可能提供“刷新”令牌）给[客户端](#client-application)，以及在将授权与 [OpenID Connect][OpenIDConnect] 协议配合使用时提供 [ID 令牌](#ID-token)。

## <a name="user-agent-based-client"></a>基于用户代理的客户端
一种[客户端应用程序](#client-application)，例如单页应用程序 (SPA)，可从 Web 服务器下载代码并在用户代理（例如 Web 浏览器）中执行。由于所有代码都在设备上执行，因此设备因为无法隐私/秘密地存储凭据而被视为“公共”客户端。有关详细信息，请参阅 [OAuth2 client types and profiles][OAuth2-Client-Types]（OAuth2 客户端类型和配置文件）。

## <a name="user-principal"></a>用户主体
与服务主体对象用于表示应用程序实例的方式一样，用户主体对象是另一种类型的安全主体，它代表用户。Azure AD Graph [用户实体][AAD-Graph-User-Entity]定义用户主体对象的架构。用户实体由用户相关的属性组成，例如名字和姓氏、用户主体名称、目录角色中的成员资格等等，可提供所需的用户标识配置，供 Azure AD 在运行时建立用户主体。用户主体用于在记录[同意](#consent)委托时代表已经身份验证的用户做出访问控制决策等操作。

## <a name="web-client"></a>Web 客户端
一种[客户端应用程序](#client-application)，它在 Web 服务器中执行所有代码，因为可以在服务器中隐私/秘密地存储凭据，所以被视为“机密”客户端。有关详细信息，请参阅 [OAuth2 client types and profiles][OAuth2-Client-Types]（OAuth2 客户端类型和配置文件）。

## 后续步骤
[Azure AD Developer's Guide][AAD-Dev-Guide]（Azure AD 开发人员指南）是用于所有 Azure AD 开发相关主题的门户，包括[应用程序集成][AAD-How-To-Integrate]的概述和 [Azure AD 身份验证与支持的身份验证方案][AAD-Auth-Scenarios]基本知识。

欢迎使用以下 Disqus 意见部分提供反馈，帮助我们改进与制作内容。

<!--Image references-->


<!--Reference style links -->

[AAD-App-Manifest]: /documentation/articles/active-directory-application-manifest/
[AAD-App-SP-Objects]: /documentation/articles/active-directory-application-objects/
[AAD-Auth-Scenarios]: /documentation/articles/active-directory-authentication-scenarios/
[AAD-Integrating-Apps]: /documentation/articles/active-directory-integrating-applications/
[AAD-Dev-Guide]: /documentation/articles/active-directory-developers-guide/
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-To-Integrate]: /documentation/articles/active-directory-how-to-integrate/
[AAD-How-To-Tenant]: /documentation/articles/active-directory-howto-tenant/
[AAD-Multi-Tenant-Overview]: /documentation/articles/active-directory-devhowto-multi-tenant-overview/
[AAD-Security-Token-Claims]: /documentation/articles/active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: /documentation/articles/active-directory-token-and-claims/
[AZURE-classic-portal]: https://manage.windowsazure.cn
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[Microsoft-Graph]: https://graph.microsoft.io
[O365-Perm-Ref]: https://msdn.microsoft.com/zh-cn/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Endpoint]: https://tools.ietf.org/html/rfc6749#section-3.1
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-AuthZ-Endpoint]: http://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken

<!---HONumber=Mooncake_0822_2016-->
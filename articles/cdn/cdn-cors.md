---
title: "将 Azure CDN 与 CORS 一起使用 | Microsoft Docs"
description: "了解如何将 Azure 内容交付网络 (CDN) 与跨域资源共享 (CORS) 一起使用。"
services: cdn
documentationcenter: 
author: camsoper
manager: erikre
editor: 
ms.assetid: 86740a96-4269-4060-aba3-a69f00e6f14e
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: casoper
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: e4f7e947ab3e9ee67224edc9cad07d82524af2b7


---
# <a name="using-azure-cdn-with-cors"></a>将 Azure CDN 与 CORS 一起使用
## <a name="what-is-cors"></a>什么是 CORS？
CORS（跨域资源共享）是一项 HTTP 功能，使在一个域中运行的 Web 应用程序能够访问另一个域中的资源。 为了减少跨站点脚本攻击的可能性，所有现代 Web 浏览器都实现了称为[同源策略](http://www.w3.org/Security/wiki/Same_Origin_Policy)的安全限制。  这可以防止网页调用其他域中的 API。  CORS 提供了一种安全方式，允许一个域（源域）调用另一个域中的 API。

## <a name="how-it-works"></a>工作原理
1. 浏览器发送带有 **Origin** HTTP 标头的 OPTIONS 请求。 此标头的值是为父页面提供服务的域。 当来自 https://www.contoso.com 的页面尝试访问 fabrikam.com 域中的用户数据时，以下请求标头将发送到 fabrikam.com： 
   
   `Origin: https://www.contoso.com`
2. 服务器会响应以下内容：
   
   * 响应中含有 **Access-Control-Allow-Origin** 标头，指示允许使用哪些源站点。 例如：
     
     `Access-Control-Allow-Origin: https://www.contoso.com`
   * 如果服务器不允许跨源请求，则为错误页
   * 带有通配符的 **Access-Control-Allow-Origin** 标头，允许所有域：
     
     `Access-Control-Allow-Origin: *`

对于复杂的 HTTP 请求，首先执行“预检”请求，以在发送整个请求之前确定其是否具有权限。

## <a name="wildcard-or-single-origin-scenarios"></a>通配符或单个源场景
当 **Access-Control-Allow-Origin** 标头设置为通配符 (*) 或单个源时，Azure CDN 上的 CORS 将自动工作，无需其他配置。  CDN 将缓存第一个响应，且后续请求将使用相同的标头。

如果你的源上设置 CORS 之前已对 CDN 发出请求，则需要清除终结点内容上的内容，以使用 **Access-Control-Allow-Origin** 标头重新加载内容。

## <a name="multiple-origin-scenarios"></a>多个源场景
如果你需要 CORS 允许一个特定的源列表，情况会稍微复杂一些。 当 CDN 缓存第一个 CORS 源的 **Access-Control-Allow-Origin** 标头时，会出现问题。  当不同的 CORS 源发出后续请求时，CDN 将为缓存的 **Access-Control-Allow-Origin** 标头提供服务，但不匹配。  有多种方法可纠正此问题。

### <a name="azure-cdn-premium-from-verizon"></a>Verizon 提供的高级 Azure CDN
启用此功能的最佳方法是使用 **Verizon 提供的高级 Azure CDN**，其中提供了一些高级功能。 

你需要[创建规则](cdn-rules-engine.md)，以检查请求中的 **Origin** 标头。  如果是有效的源，你的规则将使用请求中提供的源设置 **Access-Control-Allow-Origin** 标头。  如果不允许使用在 **Origin** 标头中指定的源，你的规则应忽略 **Access-Control-Allow-Origin** 标头，这将导致浏览器拒绝请求。 

有两种方法可以使用规则引擎来执行此操作。  在这两种情况下，来自文件的源服务器的 **Access-Control-Allow-Origin** 标头被完全忽略，并且 CDN 的规则引擎会完全管理允许的 CORS 源。

#### <a name="one-regular-expression-with-all-valid-origins"></a>一个具有所有有效源的正则表达式
在这种情况下，你将创建一个正则表达式，其中包含你要允许的所有源： 

    https?:\/\/(www\.contoso\.com|contoso\.com|www\.microsoft\.com|microsoft.com\.com)$

> [!TIP]
> **Verizon 提供的 Azure CDN** 使用 [兼容 Perl 的正则表达式](http://pcre.org/)作为其正则表达式的引擎。  你可以使用[正则表达式 101](https://regex101.com/) 等工具验证正则表达式。  请注意，“/”字符在正则表达式中有效且无需转义，但是转义该字符被认为是最佳实践，并且一些正则表达式验证器也期望对其进行转义。
> 
> 

如果正则表达式匹配，你的规则将使用发送请求的源替换源中的 **Access-Control-Allow-Origin** 标头（如有）。  你还可以添加其他 CORS 标头，例如 **Access-Control-Allow-Methods**。

![带正则表达式的规则示例](./media/cdn-cors/cdn-cors-regex.png)

#### <a name="request-header-rule-for-each-origin"></a>为每个源请求标头规则。
与其使用正则表达式，不如用 **Request Header Wildcard** [匹配条件](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1).为每个要允许的源创建单独的规则。 与正则表达式方法一样，规则引擎单独设置 CORS 标头。 

![没有正则表达式的规则示例](./media/cdn-cors/cdn-cors-no-regex.png)

> [!TIP]
> 在上面的示例中，使用的通配符 * 告知规则引擎匹配 HTTP 和 HTTPS。
> 
> 

### <a name="azure-cdn-standard"></a>标准 Azure CDN
在标准 Azure CDN 配置文件中，允许多个源但不使用通配符源的唯一机制是使用[查询字符串缓存](cdn-query-string.md)。  你需要为 CDN 终结点启用查询字符串设置，然后对每个允许的域的请求使用唯一的查询字符串。 这样做将导致 CDN 缓存每个唯一查询字符串的单独对象。 然而，这种方法并不理想，因为它将导致在 CDN 上缓存的同一文件出现多个副本。  




<!--HONumber=Nov16_HO3-->


---
title: "使用 Hive ODBC 驱动程序将 Excel 连接到 Hadoop | Microsoft Docs"
description: "了解如何设置和使用针对 Excel 的 Microsoft Hive ODBC 驱动程序以在 HDInsight 群集中查询数据。"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
tags: azure-portal
editor: cgronlun
ms.assetid: a7665a14-0211-4521-b3e7-3b26e8029cc0
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/06/2017
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: 6407c371bc51461a05429fabaf38d3f9bc80d32c
ms.openlocfilehash: 1f0b951e7b3ffc328604ce799590d478da36915d
ms.lasthandoff: 02/07/2017


---
# <a name="connect-excel-to-hadoop-with-the-microsoft-hive-odbc-driver"></a>使用 Microsoft Hive ODBC 驱动程序将 Excel 连接到 Hadoop
[!INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

Microsoft 的大数据解决方案可将 Microsoft 商业智能 (BI) 组件与已由 Azure HDInsight 部署的 Apache Hadoop 群集相集成。 此集成的一个例子是，能够使用 Microsoft Hive 开放式数据库连接 (ODBC) 驱动程序将 Excel 连接到 HDInsight 中的 Hadoop 群集的 Hive 数据仓库。

还可以使用用于 Excel 的 Microsoft Power Query 外接程序从 Excel 连接与 HDInsight 群集和其他数据源（包括其他非 HDInsight Hadoop 群集）关联的数据。 有关安装和使用 Power Query 的信息，请参阅[利用 Power Query 将 Excel 连接到 HDInsight][hdinsight-power-query]。

> [!NOTE]
> 尽管可以对基于 Linux 或 Windows 的 HDInsight 群集使用本文中的步骤，但客户端工作站需要 Windows。
> 
> 

**先决条件**：

在开始阅读本文前，你必须具有：

* **一个 HDInsight 群集**。 若要创建 HDInsight 群集，请参阅 [Azure HDInsight 入门][hdinsight-get-started]。
* 装有 Office 2013 Professional Plus、Office 365 Pro Plus、Excel 2013 Standalone 或 Office 2010 Professional Plus 的**工作站**。

## <a name="install-microsoft-hive-odbc-driver"></a>安装 Microsoft Hive ODBC 驱动程序
从[下载中心][hive-odbc-driver-download]下载并安装 Microsoft Hive ODBC 驱动程序。

此驱动程序可以安装在 32 位或 64 位版本的 Windows 7、Windows 8、Windows 10、Windows Server 2008 R2 和 Windows Server 2012 上，并将允许连接到 Azure HDInsight（1.6 及更高版本）和 Azure HDInsight Emulator（1.0.0.0 及更高版本）。 你应安装与你将在其中使用 ODBC 驱动程序的应用程序版本匹配的版本。 在本教程中，将通过 Office Excel 使用此驱动程序。

## <a name="create-hive-odbc-data-source"></a>创建 Hive ODBC 数据源
下列步骤演示如何创建 Hive ODBC 数据源。

1. 在 Windows 8 或 Windows 10 中，按 Windows 键打开“开始”屏幕，然后键入“数据源”。
2. 单击“设置 ODBC 数据源(32 位)”或“设置 ODBC 数据源(64 位)”，具体取决于 Office 版本。 如果你使用的是 Windows 7，请从“管理工具”选择“ODBC 数据源(32 位)”或“ODBC 数据源(64 位)”。 这将启动“ODBC 数据源管理器”对话框。
   
    ![ODBC 数据源管理器][img-hdi-simbahiveodbc-datasource-admin]
3. 从“用户 DNS”单击“添加”打开“新建数据源”向导。
4. 选择“Microsoft Hive ODBC 驱动程序”，然后单击“完成”。 这将启动“Microsoft Hive ODBC 驱动程序 DNS 安装程序”对话框。
5. 键入或选择以下值：
   
   | 属性 | 说明 |
   | --- | --- |
   |  数据源名称 |为你的数据源提供名称 |
   |  主机 |输入 &lt;HDInsightClusterName>.azurehdinsight.net。 例如，myHDICluster.azurehdinsight.net |
   |  端口 |使用 <strong>443</strong>。 （此端口已从 563 更改为 443。） |
   |  数据库 |使用“默认”。<strong></strong> |
   |  Hive 服务器类型 |选择“Hive Server 2”<strong></strong> |
   |  机制 |选择“Azure HDInsight 服务”<strong></strong> |
   |  HTTP 路径 |将此字段留空。 |
   |  用户名 |输入 HDInsight 群集用户的用户名。 这是在群集设置过程中创建的用户名。 如果你使用了“快速创建”选项，则默认用户名是 <strong>admin</strong>。 |
   |  密码 |输入 HDInsight 群集用户的密码。 |
   
    </table>
   
    在单击“高级选项”时，需要注意某些重要参数：
   
   | 参数 | 说明 |
   | --- | --- |
   |  使用本机查询 |选择此项时，ODBC 驱动程序将不会尝试将 TSQL 转换为 HiveQL。 仅当你 100% 确定提交的是纯 HiveQL 语句时，才应使用此项。 连接 SQL Server 或 Azure SQL 数据库时，应将此项保留为未选中状态。 |
   |  每块提取的行数 |提取大量记录时，可能需要调整此参数以确保最佳性能。 |
   |  默认字符串列长度、二进制列长度、十进制列小数位数 |数据类型长度和精度可能会影响返回数据的方式。 由于精度损失和/或截断，可能会返回不正确的信息。 |

    ![高级选项][img-HiveOdbc-DataSource-AdvancedOptions]

1. 单击“测试”以测试数据源。 正确配置数据源时，将显示“测试成功完成!”。
2. 单击“确定”关闭“测试”对话框。 现在，新的数据源应该在“ODBC 数据源管理器”中列出。
3. 单击“确定”退出向导。

## <a name="import-data-into-excel-from-hdinsight"></a>将数据从 HDInsight 导入 Excel
下列步骤介绍如何使用在上述步骤中创建的 ODBC 数据源将数据从 Hive 表中导入到 Excel 工作簿。

1. 在 Excel 中打开新工作簿或现有工作簿。
2. 在“数据”选项卡中，依次单击“从其他数据源”、“从数据连接向导”以启动“数据连接向导”。
   
    ![打开数据连接向导][img-hdi-simbahiveodbc.excel.dataconnection]
3. 选择“ODBC DSN”作为数据源，然后单击“下一步”。
4. 从 ODBC 数据源中，选择你在上一步中创建的数据源名称，然后单击“下一步”。
5. 在向导中重新输入群集的密码，然后单击“测试”，以根据需要再次验证配置。
6. 单击“确定”关闭“测试”对话框。
7. 单击 **“确定”**。 等待“选择数据库和表”对话框打开。 这可能需要几秒钟时间。
8. 选择要导入的表，然后单击“下一步”。 *hivesampletable* 是 HDInsight 群集随附的示例 Hive 表。  如果你还没有创建表，则可以选择它。 有关运行 Hive 查询和创建 Hive 表的详细信息，请参阅[将 Hive 与 HDInsight 配合使用][hdinsight-use-hive]。
9. 单击“完成” 。
10. 在“导入数据”对话框中，可更改或指定查询。 为此，请单击“属性”。 这可能需要几秒钟时间。
11. 单击“定义”选项卡，然后在“命令文本”文本框中的 Hive select 语句后面追加“LIMIT 200”。 此修改将返回的记录集限制为 200。
    
    ![连接属性][img-hdi-simbahiveodbc-excel-connectionproperties]
12. 单击“确定”关闭“连接属性”对话框。
13. 单击“确定”关闭“导入数据”对话框。  
14. 重新输入密码，然后单击“确定”。 需要几秒钟时间才能将数据导入到 Excel 中。

## <a name="next-steps"></a>后续步骤
在本文中，你已了解如何使用 Microsoft Hive ODBC 驱动程序将来自 HDInsight 服务的数据检索到 Excel 中。 同样地，你也可以将来自 HDInsight 服务的数据检索到 SQL 数据库中。 也可以将数据上载到 HDInsight 服务中。 若要了解更多信息，请参阅以下文章：

* [使用 HDInsight 分析航班延误数据][hdinsight-analyze-flight-data]
* [将数据上载到 HDInsight][hdinsight-upload-data]
* [将 Sqoop 与 HDInsight 配合使用][hdinsight-use-sqoop]

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md

[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698

[img-hdi-simbahiveodbc-datasource-admin]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png
[img-HiveOdbc-DataSource-AdvancedOptions]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png
[img-hdi-simbahiveodbc-excel-connectionproperties]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveODBC.Excel.ConnectionProperties1.png
[img-hdi-simbahiveodbc.excel.dataconnection]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png


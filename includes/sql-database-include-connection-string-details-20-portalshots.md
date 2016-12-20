<!--
includes/sql-database-include-connection-string-20-portalshots.md

Latest Freshness check:  2015-09-02 , GeneMi.

## Connection string
-->


### <a name="obtain-the-connection-string-from-the-azure-portal"></a>从 Azure 门户获取连接字符串
使用 [Azure 门户](https://portal.azure.com/)获取客户端程序与 Azure SQL 数据库进行交互所需的连接字符串：

1. 单击“浏览” > “SQL 数据库”。
   
    ![选择 SQL][1-select-sql]
2. 在“SQL 数据库”边栏选项卡左上角附近的筛选器文本框中输入数据库的名称。
   
    ![选择数据库][2-select-database]
3. 单击数据库所对应的行。
4. 显示数据库的边栏选项卡后，为了方便查看，可以单击标准的最小化控件，以便折叠用于浏览和数据库筛选的边栏选项卡。
5. 记下 **SQL 数据库**名称和**服务器名称**。  用户名是 yourusername@yourserver.
   
    ![获取连接详细信息][3-get-connection-details]
6. 将连接详细信息粘贴到客户端程序代码中。  你需要将 {your_password_here} 替换为实际密码。

<!--
Could not find a good link for PHP

For more information, see:<br/>[Connection Strings and Configuration Files](https://msdn.microsoft.com/library/ms378428.aspx).
-->


<!-- Image references. -->

[1-select-sql]: ./media/sql-database-include-connection-string-20-portalshots/connection-string-select-sql.png

[2-select-database]: ./media/sql-database-include-connection-string-20-portalshots/connection-string-select-database.PNG

[3-get-connection-details]: ./media/sql-database-include-connection-string-20-portalshots/connection-string-details.PNG


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->


<!--HONumber=Dec16_HO2-->


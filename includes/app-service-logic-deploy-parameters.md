使用 Azure 资源管理器，可以定义在部署模板时想要指定的值的参数。 该模板具有一个名为 Parameters 的部分，其中包含所有参数值。
你应该为随着要部署的项目或要部署到的环境而变化的值定义参数。 不要为永远保持不变的值定义参数。 每个参数值可在模板中用来定义所部署的资源。 

在定义参数时，请使用 **allowedValues** 字段来指定用户在部署过程中可以提供哪些值。 如果在部署过程中未提供任何值，请使用 **defaultValue** 字段为该参数赋值。

下面介绍模板中的每个参数。

### <a name="logicappname"></a>logicAppName
要创建的逻辑应用的名称。

    "logicAppName": {
        "type": "string"
    }

<!--HONumber=Nov16_HO3-->



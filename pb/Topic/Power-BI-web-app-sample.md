---
title: Power BI Web 应用示例
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d8402b48-3f71-4c42-996d-7a5310dbb480
---
# Power BI Web 应用示例
---

[下载 Web 应用示例](http://go.microsoft.com/fwlink/?LinkId=619279) | 在 GitHub 上查看代码：[Default.aspx.cs](http://go.microsoft.com/fwlink/?LinkId=619431) | [Redirect.aspx.cs](http://go.microsoft.com/fwlink/?LinkId=619432)

**重要说明** 若要运行 Web 应用示例，则必须在 **Azure Active Directory** (Azure AD) 中注册该示例，并获取**客户端 ID** 和**客户端密钥**。
**客户端 ID** 和**客户端密钥**用于对 ** Azure AD ** 进行身份验证。
请参阅[如何运行 Web 应用示例](#run)。
<a name="run"/>
##如何运行 Web 应用示例

Web 应用示例演示了如何使用 **Azure Active Directory** 对 Power BI Web 应用进行身份验证，以及如何进行 Power BI REST 调用来获取所有数据集。


**有关 Web 应用示例的详细信息可从[对 Web 应用进行身份验证](Authenticate-a-web-app.md)文章中获取或在 GitHub 上查看代码：[Default.aspx.cs](http://go.microsoft.com/fwlink/?LinkId=619431) | [Redirect.aspx.cs](http://go.microsoft.com/fwlink/?LinkId=619432)**

要运行该示例，你需要执行以下操作：

1. [下载 Web 应用示例](http://go.microsoft.com/fwlink/?LinkId=619279)
2. [在 Azure Active Directory 中注册 Web 应用示例](#register)
3. [设置在注册该示例应用时所获取的客户端 ID 和客户端密钥](#set)
4. [将重定向 URL 设置为你的 Web 应用的重定向 RUL](#redirect)

<a name="register"/>
###在 Azure Active Directory 中注册 Web 应用示例

若要在 Web 应用中调用 Power BI REST 操作，你需要**客户端 ID**和**客户端密钥**来对 ** Azure AD ** 进行身份验证，以便你的应用有权访问 Power BI REST 资源。
若要获取**客户端 ID** 和**客户端密钥**，请在 **Windows Azure 管理门户**中注册该示例应用。
在注册示例应用之前，下面是你需要的设置：

####Web 应用示例注册设置

| 设置| 值| 描述
|-|-|-|
| 名称| PowerBIWebSample| 这可以是任何唯一的名称。|
| 登录 Url| http://localhost:13526| 这是 Web 应用 Url。|
| 应用 ID Uri| https://{your tenant id}.onmicrosoft.com/PowerBIWebSample| 这是你的 Azure 租户 URI，后跟你的应用名称。例如，https://yourtenant.onmicrosoft.com/YourWebApp|
| 权限| 查看所有数据集| 由于 Web 应用示例只获取数据集，你只需拥有**查看所有数据集**权限即可。有关 Power BI 权限的详细信息，请参阅 [Power BI 权限](Power-BI-permissions.md)。|
####注册 Web 应用示例

若要注册 Web 应用示例，请参阅[注册 Web 应用](Register-a-web-app.md)，并使用上述设置。
在 Web 应用示例注册完成后，**Azure AD** 将为该应用生成**客户端 ID**。
对于**客户端密钥**，你需要设置**持续时间**。
在 **Azure 管理门户**中保存该应用后，Azure AD 将生成**客户端密钥**。
请确保复制该密钥；否则，在将来导航到该应用配置页时将不再显示该密钥。

若要运行该示例，则必须在 **web.config** 中设置 **ClientID** 和 **ClientSecret** 属性。
你可以从 **Azure 应用程序配置**页获取**客户端 ID**。
请参阅[如何获取 Web 应用 ID](Register-a-web-app.md#clientID)。
对于**客户端密钥**，请务必复制该密钥；否则，在将来导航到该配置页时将不再提供该密钥。
<a name="set"/>
###设置在注册该示例应用时所获取的客户端 ID 和客户端密钥

若要运行 Web 应用示例，必须按如下方式在 **web.config** 中设置 **ClientID** 和 **ClientSecret** 属性：

      <applicationSettings>
        <PBIWebApp.Properties.Settings>
          <setting name="ClientID" serializeAs="String">
            <value>{Client ID from Azure AD app registration}</value>
          </setting>
          <setting name="ClientSecretKey" serializeAs="String">
            <value>{Client Secret Key from Azure AD app registration}</value>      
      </setting>
        </PBIWebApp.Properties.Settings>
      </applicationSettings>

<a name="redirect"/>
###将重定向 URL 设置为你的 Web 应用的重定向 RUL

此示例将配置为在本地主机上运行。
但是，如果要开发 Web 应用，请确保将 web.config 中的 **RedirectUrl** 更改为你的 Web 应用 URL。
例如，http://{web_app_name}.azurewebsites.net/Redirect。

          <setting name="RedirectUrl" serializeAs="String">
            <value>http://{web_app_url}/Redirect</value>
          </setting>

<br/>
现在，可以运行 Web 应用示例了。

##相关主题

- [注册 Web 应用](Register-a-web-app.md)
- [对 Web 应用进行身份验证](Authenticate-a-web-app.md)
- [开始创建 Power BI 应用](Get-started-creating-a-Power-BI-app.md)
- [Power BI REST API 参考](Power-BI-REST-API-reference.md)





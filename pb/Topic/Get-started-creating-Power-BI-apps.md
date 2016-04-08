---
title: 开始创建 Power BI 应用
ms.custom: na
ms.prod: powerbi
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 04f06c61-2dbf-4f21-b1a5-652dc773f481
robots: noindex,nofollow
---
# 开始创建 Power BI 应用
Power BI 显示仪表板为交互式，并且可以通过许多不同数据源实时创建和更新。
借助 Microsoft Power BI REST API，你可以以编程方式访问某些 Power BI 资源。
使用该 API，你可以在任何支持调用 REST 操作的平台中创建应用。

你可以在 Apiary 上**[尝试使用 Power BI REST API](http://docs.powerbi.apiary.io/#)**而不创建应用。
若要了解有关 Power BI REST API 的详细信息，请参阅[Power BI REST API 参考](Power-BI-REST-API-reference.md)。

在开始创建 Power BI 应用之前，你需要 **Azure Active Directory** 和 **Power BI 服务**。
下面是在开始创建 Power BI 应用之前需要执行的操作：

1.  具有至少一个组织用户的 **Azure Active Directory**。
    
    *   如果你没有 Azure Active Directory (AAD)，请参阅[安装 Azure Active Directory](#setup)。
    *   若要在 Azure AD 租户中创建组织用户，请参阅[将用户添加到你的 Azure Active Directory](#newuser)。
2.  具有 **Power BI 服务**。
    注册 Power BI 的过程快速而简单，请转到[注册 Power BI](https://portal.office.com/Start?Sku=a403ebcc-fae0-4ca2-8c8c-7a907fd6c235/) 开始注册。
    
    *                 **注意** 你需要一个 ADD 组织用户才能注册 Power BI。
3.  具有 Power BI 服务之后，你需要注册你的客户端应用或 Web 应用。
    若要了解如何注册 Power BI 应用，请参阅[注册客户端应用](Register-a-client-app.md)或[注册 Web 应用](Register-a-web-app.md)。

<a name="setup"></a>

###安装 Azure Active Directory

Power BI 应用与 Azure Active Directory (AD) 集成在一起，以为你的应用提供安全的登录和授权。
若要将 Power BI 应用与 Azure AD 相集成，请使用 Azure 管理门户向 Azure AD 注册有关应用程序的详细信息。

####安装 Azure Active Directory

1.  导航到 https://manage.windowsazure.com 并使用具有 Azure 订阅的帐户登录。
2.  单击左侧窗格中的“ACTIVE DIRECTORY”****管理图标。
3.  单击页面底部的“新建”****按钮。
4.  依次选择“应用服务”****>“目录”****>“自定义创建”****。
    
                  ![新建 AD]
5.  输入名称和域名。
    对于国家或地区，请选择美国或可使用 Power BI 的国家/地区。
    
                  ![添加目录]
6.  选择“确定”图标。
    Azure Active Directory 已创建。
    <a name="newuser"></a>

####将用户添加到你的 Azure Active Directory

1.  在 Azure Active Directory 中，单击“用户”****。
    
                  ![添加用户]
2.  在页面底部，单击“添加用户”****。
    用户帐户用于注册 Power BI。
    
     ![在此处输入图像描述]
    
    1.  对于“用户类型”****，请选择“组织中的新用户”****。
    2.  输入“用户名”****。
    3.  输入 {Azure_AD_name}.onmicrosoft.com。
        Azure AD 名称与租户 ID 相同。
    4.  单击“下一步”****。
        
                      ![用户配置文件]
    5.  输入“用户配置文件”****
    6.  单击“下一步”****。
        对于“角色”，你可以使用“用户”。
    7.  单击“创建”****来创建一个临时用户。
        新用户将分配到一个临时密码，该密码必须在第一次登录时进行更改。
    8.  在“获取临时密码”页中，单击“完成”****图标。
        新的 Azure AD 用户已创建。

具有 **Azure Active Directory** 后，你需要注册 Power BI。

<a name="signup"></a>

###注册 Power BI

1.                [注册 Power BI](https://portal.office.com/Start?Sku=a403ebcc-fae0-4ca2-8c8c-7a907fd6c235/)。
2.  在“Power BI 注册”页中，输入在[将用户添加到你的 Azure Active Directory](#newuser) 步骤中创建的用户。
    格式为 {user name}@{Azure_AD_Name}.onmicrosoft.com。

注册过程将验证用户，并在你的 Azure AD 中设置 Power BI 服务。
Power BI 设置完成后，用户将可以使用 Power Bi 租户。

              **注意**：首次 Power BI 注册可能需要 30 分钟的时间进行设置。

注册 Power BI 后，便可以注册 Power BI 应用。

###注册 Power BI 应用

现在，你可以注册一个 Power BI 应用。
若要了解如何注册应用，请参阅[注册客户端应用](Register-a-client-app.md)或[注册 Web 应用](Register-a-web-app.md)。

注册应用后，你可以随时调用 Power BI REST API 操作。
我们提供了几个示例，可帮助你开始编写应用。

###Power BI REST API 示例

*                 [客户端应用身份验证示例](Client-app-authentication-sample.md)：演示如何获取客户端应用的访问令牌并执行所有 Power BI REST 操作。
*                 [ASP.NET Web 应用示例](Authenticate-a-web-app.md)：演示如何获取 Web 应用的访问令牌并执行 Power BI REST 操作。



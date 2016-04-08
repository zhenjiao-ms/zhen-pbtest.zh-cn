---
title: 注册客户端应用
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 51a06372-a9de-4ed9-bba9-64d24b065b7a
robots: noindex,nofollow
---
# 注册客户端应用
---

本文演示了如何在 **Azure Active Directory (Azure AD)** 中注册 Power BI 客户端应用。 若要允许你的应用程序访问 Power BI REST API，你需要向 Azure AD 注册客户端应用。 这样，你便可以为你的应用程序建立标识，并指定 Power BI REST 资源的权限。 有关 Power BI 权限的列表，请参阅 [Power BI 权限](Power-BI-Permissions.md)。

**重要说明：**在注册 Power BI 应用之前，你需要一个 [Azure Active Directory 租户和组织用户](Create-an-Azure-Active-Directory-tenant.md)以及一个 [Power BI 服务帐户](Sign-up-for-Power-BI-service.md)。
可采用两种方式注册你的客户端应用：使用 Power BI 应用注册工具进行注册或在 Azure 管理门户上进行注册。 因为只需填充几个字段，所以 Power BI 应用注册工具是最简单的选项。 但是，借助此工具，将需要使用 Azure 管理门户来管理你的应用设置。
###本文内容

- [使用 Azure 管理门户注册客户端应用](#client)
- [如何在 Azure 管理门户中获取客户端 ID](#clientID)

<a name="client"></a>
##使用 Azure 管理门户注册客户端应用

你需要在 **Azure Active Directory** 中注册你的客户端应用，为你的应用程序建立标识并指定针对 Power BI REST 资源的权限。 在注册客户端应用（例如控制台应用）时，你会收到一个**客户端 ID**。 应用程序在向客户请求权限时，使用该**客户端 ID** 来向用户标识自己的身份。

若要了解如何使用 Azure AD **客户端 ID** 对客户端应用进行身份验证，请参阅[对客户端应用进行身份验证](Authenticate-a-client-app.md)。

###注册客户端应用

下面介绍了如何注册客户端应用：
1. 接受 [Microsoft Power BI API 条款](https://powerbi.microsoft.com/en-us/api-terms)。
2. 登录到你的 Microsoft Azure 订阅，网址为 https://manage.windowsazure.com。
3. 在左侧服务面板中，选择“ACTIVE DIRECTORY”。****
4. 单击你所属的 Active Directory。

    ![步骤 3](../Image/Register-app-3.png)

5. 单击“应用程序”。****

    ![步骤 4](../Image/Register-app-4.png)

6. 单击“添加”。****

    ![步骤 5](../Image/Register-app-5.png)

7. 在“向我们描述你的应用程序”中，输入“名称”、选择“本机客户端应用程序”类型，然后单击“下一步”图标。****************

    ![步骤 6](../Image/Register-app-6.png)

8. 在“应用程序信息”中，输入“重定向 URI”。******** 对于客户端应用，重定向 URI 为 AAD 提供了更多详细信息（关于它将对其进行身份验证的特定应用程序）。 对于客户端应用，你可以使用以下 URI：https://login.live.com/oauth20_desktop.srf。

9.  单击“完成”图标。****
10. 在应用程序页中，选择“配置”。**** 你将看到你的**客户端 ID**。
11. 在“配置”页的“针对其他应用程序的权限”下，单击“添加应用程序”。********

    ![步骤 11](../Image/Register-app-11.png)

12. 在“针对其他应用程序的权限”中，选择“Power BI 服务”。********

    ![步骤 12](../Image/Register-app-12.png)

    **重要说明：**如果你在“针对其他应用程序的权限”列表中未看到“Power BI 服务”，你需要注册 [Power BI 服务](https://www.powerbi.com/)。******** 若要注册 Power BI 服务，你的 Azure Active Directory (AAD) 租户中至少需要一个组织用户。 如果你没有 Azure Active Directory (AAD) 租户，请参阅[安装 Azure Active Directory](Setup-Azure-Active-Directory.md) 来创建一个 Azure AD 租户，并在 Azure AD 租户中创建一个组织用户。

13. 单击“完成”图标。****
14. 在“针对其他应用程序的权限”组中，选择所有“委派权限”，然后选择一个或多个权限。******** 有关 Power BI 权限的详细信息，请参阅 [Power BI 权限](Power-BI-permissions.md)。

    ![步骤 14](../Image/Register-app-14.png)

15. 单击“保存”。****

<a name="clientID"></a>
##如何获取客户端应用 ID

在注册客户端应用（例如控制台应用）时，你会收到一个**客户端 ID**。 应用程序在向客户请求权限时，使用该**客户端 ID** 来向用户标识自己的身份。

下面介绍了如何获取客户端 ID：

1. 登录到你的 Microsoft Azure 订阅，网址为 https://manage.windowsazure.com。
2. 在左侧服务面板中，选择“ACTIVE DIRECTORY”。****
3. 单击你所属的 Active Directory。
4. 单击“应用程序”。****
5. 选择一个应用程序。
6. 在应用程序页中，选择“配置”。****
7. 在“配置”页中，复制“客户端 ID”。********

    ![步骤 1.3](../Image/Register-app-3a.png)

##创建 Power BI 应用的后续步骤

- [创建 Power BI 应用](Introduction-to-creating-a-Power-BI-app.md)
- [了解如何使用 Azure AD 进行身份验证](Authenticate-to-Power-BI-service.md)




